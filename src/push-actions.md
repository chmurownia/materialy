# 4. Publikacja strony po każdym pushu (GitHub Actions)

*Krok 4 z 6.* Strona aktualizuje się automatycznie po każdej zmianie wypchniętej do gałęzi `main`.

1. W *Settings → Pages → Build and deployment → Source* wybierz **GitHub Actions**.
2. W repozytorium utwórz plik `.github/workflows/static.yml` (*Add file → Create new file*, wpisz całą ścieżkę z ukośnikami) z poniższą treścią:

    ```yaml title=".github/workflows/static.yml"
    name: Publikacja po pushu

    on:                     # kiedy workflow ma się uruchamiać
      push:
        branches: [main]    # uruchom po każdym pushu do gałęzi main
      workflow_dispatch:    # opcjonalnie: ręczne uruchomienie

    permissions:
      contents: read
      pages: write
      id-token: write

    concurrency:
      group: pages
      cancel-in-progress: false

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

3. Zatwierdź plik (**Commit changes**) bezpośrednio na gałęzi `main`. To już uruchamia pierwsze wdrożenie.
4. W zakładce **Actions** zobaczysz uruchomiony workflow. Gdy będzie zielony ✅, strona jest zaktualizowana.
5. Od teraz każdy commit lub merge Pull Requesta do `main` automatycznie odświeża stronę.

!!! tip "Publikacja tylko po zmianie plików strony"

    Aby workflow nie uruchamiał się przy zmianach niezwiązanych ze stroną, dodaj w sekcji `push` filtr ścieżek, np. `paths: ["www/**"]`.

!!! info "Wolisz kontrolować moment publikacji?"

    Zobacz następny krok: [publikacja po nowym Release](release-actions.md).

[← GitHub Pages](github-pages.md){ .md-button }
[Dalej: publikacja po Release →](release-actions.md){ .md-button .md-button--primary }
