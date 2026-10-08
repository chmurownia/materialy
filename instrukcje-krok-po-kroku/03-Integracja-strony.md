# Ćwiczenie 3: Podpięcie formularza na stronie (Frontend + GitHub)

> **Teoria:** przeczytaj najpierw `../09-Integracja-Frontend-JS.md`.
>
> **Wymagania wstępne:** ukończone ćwiczenie `02` (masz Invoke URL i działający endpoint `/entries`).
>
> **Czas:** ok. 25 minut.

## Cel

Dodasz do swojej strony formularz Księgi Gości, podmienisz w nim adres API na swój, wyślesz zmianę przez Pull Request i sprawdzisz, czy wpisy zapisują się na żywo.

---

## Krok 1: Otwórz plik strony na GitHubie

1. 🖱️ Wejdź na repozytorium swojego zespołu na GitHubie.
2. 🖱️ Przejdź do folderu `docs/` i otwórz plik `ksiega.md` (jeśli go nie ma — utworzysz go w kroku 2).
3. 🖱️ Kliknij ikonę ołówka (**Edit this file**), aby edytować plik w przeglądarce.

> 💡 Możesz pracować bezpośrednio na GitHubie, bez instalowania niczego na komputerze.

---

## Krok 2: Wklej gotowy snippet

1. ⌨️ Wklej do pliku `ksiega.md` **gotowy snippet HTML/JS**, który dostarczył nauczyciel (formularz Księgi Gości).
2. Znajdź w nim linijkę ze zmienną `API_URL`. Wygląda mniej więcej tak:

   ```javascript
   const API_URL = '__API_URL__';
   ```
3. ⌨️ Podmień wartość na swój **pełny endpoint** z ćwiczenia 2 (Invoke URL **wraz z** `/entries`):

   ```javascript
   const API_URL = 'https://abc123xyz.execute-api.eu-north-1.amazonaws.com/entries';
   ```

⚠️ **To jedyna linijka, którą zmieniasz.** Reszta snippetu ma działać bez modyfikacji. Uważaj na:
- pełny adres z `https://` na początku,
- `/entries` na końcu,
- apostrofy `'...'` wokół adresu.

---

## Krok 3: Zapisz zmiany przez Pull Request

1. 🖱️ Pod edytorem wybierz **Create a new branch for this commit** (nowa gałąź, np. `dodaj-ksiege-gosci`).
2. ⌨️ Wpisz krótki opis commita, np. `Podpiecie formularza Ksiegi Gosci pod API`.
3. 🖱️ Kliknij **Propose changes**, a następnie **Create pull request**.
4. 🖱️ (Jeśli pracujesz w parze) Poproś kolegę/koleżankę o **code review**. Jeśli robisz to sam w domu — przejrzyj zmiany samodzielnie w zakładce **Files changed**.
5. 🖱️ Kliknij **Merge pull request → Confirm merge**.

---

## Krok 4: Poczekaj na automatyczne wdrożenie (CI/CD)

1. 🖱️ Przejdź do zakładki **Actions** w repozytorium.
2. Zobaczysz uruchomiony workflow (GitHub Actions buduje stronę i wysyła ją na S3 — to Pipeline z Projektu 1).
3. Poczekaj, aż przy przebiegu pojawi się zielony ✔. Zwykle 1-3 minuty.

---

## ✅ Punkt kontrolny — test na żywo

1. 🖱️ Wejdź na swoją stronę (subdomenę, np. `https://zespolX.szkola.pl`) i otwórz podstronę **Księga Gości**.
2. Lista wpisów powinna się załadować (zobaczysz wpis `Jan Testowy` z poprzednich ćwiczeń albo informację o braku wpisów).
3. ⌨️ Wpisz swoje imię i wiadomość, 🖱️ kliknij **Wyślij wiadomość**.
4. Wpis powinien pojawić się na liście w ciągu chwili.

Jeśli wpis się zapisał i wyświetlił — **cała infrastruktura jest spięta: przeglądarka → API Gateway → Lambda → DynamoDB**. 🎉

## ⚠️ Najczęstsze błędy

- **„Nie udało się pobrać wpisów" / błąd CORS w konsoli (F12)** → w API Gateway (ćwiczenie 2, krok 4) adres w **Allow-Origin** nie zgadza się z adresem strony. Sprawdź `https` i brak końcowego slasha.
- **Formularz wysyła, ale nic się nie zapisuje** → sprawdź, czy `API_URL` ma na końcu `/entries`. Bez tego trafiasz w nieistniejącą trasę (błąd `404`).
- **Zmiany nie widać na stronie** → CloudFront buforuje stronę. Odśwież ją z pominięciem cache (Ctrl+Shift+R / Cmd+Shift+R) albo poczekaj chwilę. Sprawdź też, czy workflow w **Actions** zakończył się zielonym ✔.
- **Stara wartość `__API_URL__`** → nie podmieniłeś adresu w kroku 2.
