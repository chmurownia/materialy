# 3. Jak stworzyć własną stronę w GitHub Pages

*Krok 3 z 6.* Darmowy hosting strony prosto z repozytorium.

1. Kliknij **+ → New repository**. Jako nazwę wpisz `twoja-nazwa.github.io` (gdzie `twoja-nazwa` to Twój login) albo dowolną nazwę dla strony projektu. Ustaw repozytorium jako **Public**.
2. Dodaj plik `index.html`: *Add file → Create new file* i wklej:

    ```html
    <!DOCTYPE html>
    <html lang="pl">
    <head><meta charset="UTF-8"><title>Moja strona</title></head>
    <body>
      <h1>Cześć, świecie!</h1>
      <p>To moja pierwsza strona na GitHub Pages.</p>
    </body>
    </html>
    ```

3. Zatwierdź zmianę przyciskiem **Commit changes**.
4. Przejdź do *Settings → Pages*. W sekcji **Build and deployment → Source** wybierz **Deploy from a branch**, branch `main` i folder `/ (root)`, a potem **Save**.
5. Po 1–2 minutach strona będzie dostępna pod adresem `https://twoja-nazwa.github.io` (lub `https://twoja-nazwa.github.io/nazwa-repo/`). Postęp zobaczysz w zakładce *Actions*.
6. Każda kolejna zmiana w `index.html` (commit na `main`) automatycznie odświeża stronę.

[← Student Pack](student-pack.md){ .md-button }
[Dalej: publikacja po każdym pushu →](push-actions.md){ .md-button .md-button--primary }
