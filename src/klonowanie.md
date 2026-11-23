# 6. Jak sklonować repozytorium (gh CLI lub git)

*Krok 6 z 6.* Pobierz kopię repozytorium na komputer – bez zapisywania tokenów w plikach.

!!! warning "Komputer szkolny"

    Nigdy nie zapisuj tokenu w plikach, w adresie repozytorium (`https://TOKEN@github.com/...`) ani w historii poleceń. Token trzymaj wyłącznie w zmiennej środowiskowej na czas sesji i wyloguj się na koniec.

## Krok 1. Wygeneruj token (PAT)

1. Wejdź w *GitHub → Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token*.
2. Ustaw **krótki czas ważności** (np. 7 dni) i dostęp tylko do wybranych repozytoriów.
3. Uprawnienia: *Contents: Read and write* (do `git push`) – do samego klonowania publicznego repo token nie jest potrzebny.
4. Skopiuj token – zobaczysz go tylko raz. Nie wklejaj go do notatnika ani plików.

## Krok 2. Ustaw token w zmiennej środowiskowej

Zmienna istnieje tylko w bieżącym oknie terminala i znika po jego zamknięciu.

=== "Windows – PowerShell"

    ```powershell
    $env:GH_TOKEN = Read-Host "Wklej token"
    ```

=== "Windows – CMD"

    ```bat
    set GH_TOKEN=wklej_token
    ```

=== "Linux / macOS (Bash)"

    ```bash
    read -rs -p "Wklej token: " GH_TOKEN; export GH_TOKEN; echo
    ```

!!! tip "Wskazówka"

    `gh` automatycznie używa zmiennej `GH_TOKEN` – nie musisz się logować osobno. Polecenie `read -s` / `Read-Host` nie wyświetla tokenu i nie zapisuje go w historii (w CMD wpisany token może pozostać w historii okna – preferuj PowerShell).

## Opcja A. Klonowanie przez gh CLI

1. Sprawdź, czy `gh` jest zainstalowany: `gh --version`.
2. Sprawdź logowanie przez zmienną: `gh auth status`.
3. Skonfiguruj git, by używał `gh` jako pomocnika uwierzytelniania (zapis w konfiguracji dotyczy tylko polecenia, nie tokenu):

    ```bash
    gh auth setup-git
    ```

4. Sklonuj repozytorium:

    ```bash
    gh repo clone chmurownia/materialy
    cd materialy
    ```

## Opcja B. Klonowanie przez git (HTTPS)

Publiczne repozytorium sklonujesz bez tokenu:

```bash
git clone https://github.com/chmurownia/materialy.git
cd materialy
```

Do `git push` lub repozytoriów prywatnych przekaż token jednorazowo przez zmienną, bez zapisywania go w adresie `remote`:

=== "PowerShell"

    ```powershell
    git -c credential.helper= -c http.extraHeader="Authorization: Basic $([Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes("x-access-token:$env:GH_TOKEN")))" push
    ```

=== "Bash"

    ```bash
    git -c credential.helper= -c http.extraHeader="Authorization: Basic $(printf 'x-access-token:%s' "$GH_TOKEN" | base64 -w0)" push
    ```

*Prościej: skonfiguruj `gh auth setup-git` (Opcja A) i używaj zwykłego `git push`.*

## Krok 3. Podstawowa praca z repozytorium

```bash
git status
git add .
git commit -m "Opis zmiany"
git push
```

Jeśli git poprosi o tożsamość (na komputerze szkolnym ustaw ją tylko lokalnie dla repozytorium):

```bash
git config user.name "Twoje Imię"
git config user.email "twoj-login@users.noreply.github.com"
```

## Krok 4. Sprzątanie po zakończeniu pracy

=== "PowerShell"

    ```powershell
    Remove-Item Env:GH_TOKEN
    ```

=== "Bash"

    ```bash
    unset GH_TOKEN
    ```

- Zamknij okno terminala.
- Usuń sklonowany folder, jeśli nie jest już potrzebny.
- Unieważnij token: *Settings → Developer settings → Personal access tokens → Delete*.
- Wyloguj się z GitHuba w przeglądarce.

## Komputer prywatny

!!! info "Własny komputer"

    Możesz zalogować się raz, a dane przechowa bezpieczny magazyn systemowy (nie plik z tokenem):

    ```bash
    gh auth login
    gh auth setup-git
    ```

    Wybierz *GitHub.com → HTTPS → Login with a web browser* – nie musisz tworzyć tokenu ręcznie. Token trafi do menedżera poświadczeń systemu (Windows Credential Manager, macOS Keychain, Linux secret service). Alternatywnie użyj kluczy SSH: `git clone git@github.com:chmurownia/materialy.git`. Nadal nie commituj tokenów do repozytorium ani nie wpisuj ich na stałe w plikach takich jak `.bashrc` czy `.env` śledzonych przez git.

[← Publikacja po Release](release-actions.md){ .md-button }
[Na stronę główną](index.md){ .md-button .md-button--primary }
