# Ćwiczenie 4: Rozmowa z cudzym API (cURL / Postman)

> **Teoria:** przeczytaj najpierw `../10-API-Economy-Wspolpraca.md`.
>
> **Wymagania wstępne:** ukończone ćwiczenie `03` (Twoje API działa) oraz **adres API innego zespołu** (Invoke URL + `/entries`).
>
> **Czas:** ok. 25 minut.
>
> ℹ️ To ćwiczenie najlepiej robi się na żywo na zajęciach (wymiana adresów między zespołami). W domu możesz przećwiczyć je **na własnym API** — wszystkie komendy zadziałają też z Twoim endpointem. Gdy wrócisz na zajęcia, podmienisz adres na cudzy.

## Cel

Nauczysz się wysyłać żądania do API **bez używania strony internetowej** — bezpośrednio z terminala (cURL) lub z aplikacji Postman. Zobaczysz, że strona to tylko jedna z „twarzy" aplikacji, a prawdziwym interfejsem jest API.

---

## Wariant A: cURL (terminal)

cURL jest wbudowany w macOS, Linux i nowe wersje Windows (PowerShell). Otwórz terminal.

### A1. Pobierz listę wpisów (GET)

⌨️ Wpisz (podmień adres na swój lub cudzy):

```bash
curl "https://ADRES-API.execute-api.eu-north-1.amazonaws.com/entries"
```

Powinieneś zobaczyć listę wpisów w formacie JSON.

### A2. Dodaj wpis (POST)

⌨️ Wpisz:

```bash
curl -X POST "https://ADRES-API.execute-api.eu-north-1.amazonaws.com/entries" \
  -H "Content-Type: application/json" \
  -d '{"author": "Zespol X", "message": "Pozdrowienia wyslane przez cURL!"}'
```

Odpowiedź powinna brzmieć `{"message": "Entry saved"}` (status `201`).

> 💡 Na Windows w PowerShell użyj znaku backtick `` ` `` zamiast `\` do łamania linii, albo wpisz całą komendę w jednej linii.

---

## Wariant B: Postman (aplikacja z interfejsem)

1. 🖱️ Pobierz i zainstaluj **Postman** (lub użyj wersji w przeglądarce na postman.com).
2. 🖱️ Kliknij **New → HTTP Request**.

### B1. GET

1. 🖱️ Z listy metod wybierz **GET**.
2. ⌨️ W pole adresu wklej endpoint z `/entries`.
3. 🖱️ Kliknij **Send**. Na dole, w sekcji **Body**, zobaczysz listę wpisów (JSON).

### B2. POST

1. 🖱️ Zmień metodę na **POST**, adres zostaw ten sam.
2. 🖱️ Wejdź w zakładkę **Body → raw** i z listy po prawej wybierz **JSON**.
3. ⌨️ Wpisz treść:

   ```json
   { "author": "Zespol X", "message": "Pozdrowienia z Postmana!" }
   ```
4. 🖱️ Kliknij **Send**. Oczekiwany status: **201 Created**, treść `{"message": "Entry saved"}`.

---

## ✅ Punkt kontrolny

- Udało Ci się dodać wpis przez cURL **lub** Postman (status `201`).
- Po żądaniu `GET` widzisz swój świeżo dodany wpis na liście.
- Jeśli robisz to na zajęciach: wpis, który wysłałeś do **cudzego** API, pojawił się na **ich** stronie Księgi Gości.

💡 Zwróć uwagę: cURL i Postman **nie są przeglądarkami**, więc CORS ich nie dotyczy. Dlatego działają nawet wtedy, gdy formularz w przeglądarce blokowałby żądanie. To pokazuje, że CORS chroni użytkownika przeglądarki, a nie samo API.

---

## Część druga: współpraca na GitHubie (fork + PR)

Jeśli masz dostęp do repozytorium innego zespołu:

1. 🖱️ Wejdź na ich repozytorium i kliknij **Fork** (tworzysz własną kopię).
2. 🖱️ W swoim forku wyedytuj odpowiedni plik (np. stronę z listą zaprzyjaźnionych zespołów) i dodaj **link do swojej strony partnerskiej**.
3. 🖱️ Zapisz zmianę i otwórz **Pull Request** z powrotem do ich repozytorium.
4. 🖱️ Gdy inny zespół przyśle PR do Ciebie — przejrzyj go (**code review**) i zaakceptuj (**Merge**).

## ⚠️ Najczęstsze błędy

- **`{"error":"Both 'author' and 'message' are required"}`** → w treści JSON brakuje jednego z pól albo jest puste.
- **Wpis został odrzucony / zablokowany przez filtr treści** → to normalne. API ma wbudowany automatyczny filtr, który odrzuca wiadomości uznane za wulgarne lub obraźliwe. Jeśli trafisz na taki komunikat zamiast `201`, po prostu wyślij kulturalną wiadomość i spróbuj ponownie.
- **`{"message":"Not Found"}`** → adres bez `/entries` na końcu.
- **Błąd parsowania JSON w cURL na Windows** → zamień apostrofy/cudzysłowy zgodnie ze składnią PowerShell lub użyj Postmana (prościej na Windows).
