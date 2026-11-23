# 4. Publikacja strony po nowym Release (GitHub Actions)

*Krok 4 z 5.* Zamiast publikować po każdym commicie, strona zaktualizuje się dopiero po utworzeniu nowego wydania (Release).

1. W *Settings → Pages → Build and deployment → Source* wybierz **GitHub Actions**.
2. W repozytorium utwórz plik `.github/workflows/pages.yml` (*Add file → Create new file*, wpisz całą ścieżkę z ukośnikami) z poniższą treścią:

    ```yaml title=".github/workflows/pages.yml"
    name: Publikacja po Release

    on:                     # kiedy workflow ma się uruchamiać
      release:
        types: [published]   # uruchom po opublikowaniu Release
      workflow_dispatch:     # opcjonalnie: ręczne uruchomienie

    permissions:
      contents: read
      pages: write
      id-token: write

    concurrency:
      group: pages
      cancel-in-progress: true

    jobs:                        # lista zadań workflow
      deploy:                    # nazwa jedynego zadania
        runs-on: ubuntu-latest   # maszyna wirtualna z Ubuntu
        environment:             # środowisko wdrożeniowe wymagane przez Pages
          name: github-pages
          url: ${{ steps.deployment.outputs.page_url }}
        steps:
          - uses: actions/checkout@v7   # pobiera kod repozytorium na maszynę
          - uses: actions/configure-pages@v6    # przygotowuje konfigurację GitHub Pages
          - uses: actions/upload-pages-artifact@v5  # pakuje pliki do artefaktu dla Pages
            with:
              path: .            # folder ze stroną (np. www)
          - id: deployment
            uses: actions/deploy-pages@v5  # publikuje artefakt na GH Pages
    ```

3. Zatwierdź plik (**Commit changes**). Od teraz zwykłe commity *nie* zmieniają strony.
4. Opublikuj wydanie: *Releases → Create a new release*, w polu *Choose a tag* wpisz np. `v1.0.0` i wybierz *Create new tag*, dodaj tytuł, a następnie kliknij **Publish release**.
5. W zakładce **Actions** zobaczysz uruchomiony workflow. Gdy będzie zielony ✅, strona jest zaktualizowana.
6. Przy kolejnych zmianach: zrób commit, a gdy będziesz gotowy do publikacji – utwórz nowy Release (`v1.0.1`, `v1.1.0`…).

!!! warning "Ochrona środowiska"

    Jeśli workflow zgłasza błąd o ochronie środowiska `github-pages`, w *Settings → Environments → github-pages* dopuść wdrażanie z tagów (lub usuń ograniczenie do brancha `main`).

[← GitHub Pages](github-pages.md){ .md-button }
[Dalej: klonowanie repozytorium →](klonowanie.md){ .md-button .md-button--primary }
