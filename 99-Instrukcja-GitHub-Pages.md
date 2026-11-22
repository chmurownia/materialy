# Instrukcja: Strona WWW na GitHub Pages

Cel: umieścić gotową stronę (pliki na Twoim laptopie) w repozytorium na GitHubie i opublikować ją pod adresem `https://TWOJ-LOGIN.github.io/NAZWA-REPO/`.

## Czego potrzebujesz

- Konta na [github.com](https://github.com) (darmowe).
- Folderu ze stroną na laptopie. W głównym folderze musi być plik **`index.html`** (dokładnie tak, małymi literami). Obok mogą leżeć `style.css`, `script.js`, folder `images/` itd.
- Zainstalowanego Gita (sprawdź w terminalu: `git --version`). Jeśli go nie ma: https://git-scm.com/downloads

> Wskazówka: w kodzie strony używaj ścieżek względnych, np. `images/foto.jpg`, a nie `C:\Users\...\foto.jpg`. GitHub rozróżnia wielkość liter: `Foto.jpg` to nie to samo co `foto.jpg`.

---

## Część 1. Utworzenie repozytorium na GitHubie

1. Zaloguj się na GitHub.
2. Kliknij **+** w prawym górnym rogu, a potem **New repository**.
3. Wypełnij:
   - **Repository name**: np. `moja-strona` (bez spacji i polskich znaków).
   - **Public** (darmowe GitHub Pages wymagają publicznego repo).
   - **Nie** zaznaczaj "Add a README file", żeby repo było puste.
4. Kliknij **Create repository**.
5. Zostaw otwartą stronę z nowym repo. Będzie na niej adres repo, np. `https://github.com/TWOJ-LOGIN/moja-strona.git`.

---

## Część 2. Wysłanie plików (metoda A: terminal, zalecana)

### Jednorazowa konfiguracja Gita (jeśli jeszcze nie robiłeś)

```bash
git config --global user.name "Twoje Imie Nazwisko"
git config --global user.email "twoj-email@example.com"
```

Użyj tego samego e-maila, który masz na GitHubie.

### Wysłanie strony

1. Otwórz terminal (macOS: Terminal; Windows: Git Bash lub PowerShell).
2. Przejdź do folderu ze stroną:

   ```bash
   cd /sciezka/do/folderu/ze/strona
   ```

   Sprawdź, czy jest tam `index.html`:

   ```bash
   ls
   ```

   (Windows PowerShell: `dir`)

3. Zainicjuj repozytorium i wyślij pliki (podmień adres na swój):

   ```bash
   git init
   git add .
   git commit -m "Pierwsza wersja strony"
   git branch -M main
   git remote add origin https://github.com/TWOJ-LOGIN/moja-strona.git
   git push -u origin main
   ```

4. Przy `git push` GitHub poprosi o logowanie. Hasło do konta **nie działa**. Użyj:
   - **Personal Access Token**: GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic) → Generate new token, zaznacz uprawnienie `repo`. Wklej token zamiast hasła.
   - albo zaloguj się przez przeglądarkę, jeśli Git o to poprosi (Git Credential Manager).

5. Odśwież stronę repo na GitHubie. Powinieneś zobaczyć swoje pliki.

## Część 2 (metoda B: bez terminala, przez przeglądarkę)

1. Wejdź do swojego pustego repo na GitHubie.
2. Kliknij **uploading an existing file**.
3. Przeciągnij **zawartość** folderu ze stroną (pliki i podfoldery, nie sam folder nadrzędny), tak aby `index.html` był w głównym katalogu repo.
4. Na dole wpisz opis zmiany i kliknij **Commit changes**.

> Ta metoda ma limit ok. 100 plików naraz i nie nadaje się do dużych projektów.

---

## Część 3. Włączenie GitHub Pages

1. W repozytorium kliknij zakładkę **Settings**.
2. W menu po lewej wybierz **Pages**.
3. W sekcji **Build and deployment**:
   - **Source**: `Deploy from a branch`
   - **Branch**: `main`, folder `/ (root)`
4. Kliknij **Save**.
5. Poczekaj 1-3 minuty. Odśwież stronę **Settings → Pages**. Pojawi się komunikat:

   > Your site is live at `https://TWOJ-LOGIN.github.io/moja-strona/`

6. Otwórz ten adres. Strona działa.

Postęp publikacji widać w zakładce **Actions** (workflow "pages build and deployment"). Zielony znaczek oznacza sukces.

---

## Część 4. Aktualizacja strony

Po każdej zmianie w plikach:

```bash
git add .
git commit -m "Opis zmiany"
git push
```

Zmiany pojawią się na stronie po 1-3 minutach. Jeśli nie widzisz zmian, odśwież z pominięciem pamięci podręcznej (Ctrl+Shift+R, na macOS Cmd+Shift+R).

---

## Najczęstsze problemy

| Problem | Przyczyna i rozwiązanie |
|---|---|
| Błąd 404 na adresie strony | Brak `index.html` w głównym katalogu repo, albo plik nazywa się inaczej (np. `Index.html`). Zmień nazwę. |
| Pages nie pokazuje opcji publikacji | Repo jest prywatne. Zmień na Public: Settings → General → Danger Zone → Change visibility. |
| Strona bez stylów lub obrazków | Złe ścieżki lub wielkość liter w nazwach plików. Użyj ścieżek względnych i sprawdź nazwy. |
| `git push` odrzucone: `Authentication failed` | Użyj tokenu zamiast hasła (Część 2, krok 4). |
| `remote origin already exists` | Wykonaj `git remote set-url origin https://github.com/TWOJ-LOGIN/moja-strona.git`. |
| `failed to push some refs` | Repo na GitHubie nie jest puste. Najprościej utwórz nowe, puste repo. |
| Zmiany nie są widoczne | Poczekaj kilka minut, sprawdź zakładkę Actions i odśwież bez cache. |

---

## Opcjonalnie: własna nazwa adresu

Adres `TWOJ-LOGIN.github.io` (bez nazwy repo) dostaniesz, jeśli repo nazwiesz dokładnie `TWOJ-LOGIN.github.io`. Własną domenę ustawisz w Settings → Pages → **Custom domain**.


