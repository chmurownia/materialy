# Ćwiczenie 2: Publiczne API (Amazon API Gateway + CORS)

> **Teoria:** przeczytaj najpierw `../08-API-Gateway-Wystawienie-API.md`.
>
> **Wymagania wstępne:** ukończone ćwiczenie `01` (Lambda działa i zwraca `201` w teście).
>
> **Czas:** ok. 25 minut.

## Cel

Wystawisz swoją Lambdę na świat przez publiczny adres URL, utworzysz trasy `GET /entries` i `POST /entries`, skonfigurujesz CORS i skopiujesz **Invoke URL**.

---

## Krok 1: Utwórz HTTP API

1. 🖱️ Sprawdź region: **`eu-north-1`**. ⌨️ W wyszukiwarce wpisz `API Gateway`, 🖱️ wejdź w usługę.
2. 🖱️ Znajdź kafelek **HTTP API** i kliknij **Build**.

   ⚠️ Wybierz **HTTP API**, a nie "REST API". To dwie różne usługi — my używamy HTTP API (nowsza, tańsza).
3. 🖱️ W sekcji **Integrations** kliknij **Add integration**:
   - Typ: **Lambda**
   - Region: `eu-north-1`
   - **Lambda function:** wybierz z listy `ksiega-gosci-zespolX`
4. ⌨️ **API name:** `ksiega-gosci-zespolX`. 🖱️ Kliknij **Next**.

---

## Krok 2: Zdefiniuj trasy (routes)

Na ekranie **Configure routes** utworzysz dwie trasy prowadzące do tej samej Lambdy.

1. Pierwsza trasa (zwykle już wstępnie wypełniona):
   - **Method:** `GET`
   - **Resource path:** `/entries`
   - **Integration target:** Twoja Lambda
2. 🖱️ Kliknij **Add route** i dodaj drugą:
   - **Method:** `POST`
   - **Resource path:** `/entries`
   - **Integration target:** ta sama Lambda
3. 🖱️ Kliknij **Next**.

⚠️ Ścieżka musi być dokładnie `/entries` (ze slashem na początku). Kod i strona oczekują tej nazwy.

---

## Krok 3: Stage (publikacja)

1. Na ekranie **Define stages** zostaw domyślny stage o nazwie **`$default`** z włączonym **Auto-deploy**.
2. 🖱️ Kliknij **Next**, przejrzyj podsumowanie i kliknij **Create**.

💡 `$default` + Auto-deploy oznacza, że API jest aktywne od razu i każda zmiana publikuje się automatycznie.

---

## Krok 4: Skonfiguruj CORS

To najważniejszy krok — bez niego formularz na stronie dostanie błąd w przeglądarce.

1. 🖱️ W menu po lewej (w widoku Twojego API) kliknij **CORS**.
2. 🖱️ Kliknij **Configure** i uzupełnij:
   - **Access-Control-Allow-Origin:** adres Twojej strony, np. `https://zespolX.szkola.pl`
     - 💡 Na czas testów w domu możesz tymczasowo wpisać `*` (gwiazdka = każda domena), ale **pamiętaj, by przed oddaniem projektu zawęzić to do adresu swojej strony** — to kwestia bezpieczeństwa.
   - **Access-Control-Allow-Methods:** zaznacz `GET`, `POST`, `OPTIONS`
   - **Access-Control-Allow-Headers:** wpisz `Content-Type`
3. 🖱️ Kliknij **Save**.

---

## Krok 5: Skopiuj Invoke URL

1. 🖱️ Wejdź w zakładkę **Stages** (lub główny ekran API) i znajdź **Invoke URL**. Wygląda tak:

   ```
   https://abc123xyz.execute-api.eu-north-1.amazonaws.com
   ```
2. 📝 **Zapisz ten adres w notatniku.** Twój pełny endpoint Księgi Gości to Invoke URL + `/entries`:

   ```
   https://abc123xyz.execute-api.eu-north-1.amazonaws.com/entries
   ```

---

## ✅ Punkt kontrolny — test w przeglądarce

1. 🖱️ Wklej w pasek adresu przeglądarki swój endpoint z `/entries` na końcu (to żądanie `GET`).
2. Powinieneś zobaczyć listę wpisów w formacie JSON — np. `[{"author":"Jan Testowy","message":"Pierwszy wpis z domu!","timestamp":...}]` albo pustą listę `[]`.

Jeśli widzisz JSON — API działa! 🎉

## ⚠️ Najczęstsze błędy

- **`{"message":"Not Found"}`** → zła ścieżka. Endpoint musi kończyć się na `/entries`.
- **`{"message":"Internal Server Error"}`** → problem w Lambdzie (wróć do ćwiczenia 1, sprawdź uprawnienia i zmienną środowiskową).
- **Formularz na stronie nie działa, w konsoli przeglądarki (F12) błąd „CORS"** → wróć do kroku 4. Najczęściej adres w **Allow-Origin** nie zgadza się dokładnie z adresem strony (uwaga na `http` vs `https` i końcowy slash).

📝 Masz działający publiczny endpoint. W następnym ćwiczeniu podepniesz go pod formularz na stronie.
