# Ćwiczenie 1: Funkcja Lambda (serce aplikacji)

> **Teoria:** przeczytaj najpierw `../07-Lambda-Logika-Aplikacji.md`.
>
> **Wymagania wstępne:** ukończone ćwiczenie `00` (masz tabelę DynamoDB) i zapisana jej nazwa.
>
> **Czas:** ok. 25 minut.

## Cel

Utworzysz funkcję AWS Lambda w Pythonie, wkleisz gotowy kod obsługujący Księgę Gości, połączysz ją z tabelą DynamoDB i przetestujesz bez udziału strony internetowej.

---

## Krok 1: Utwórz funkcję Lambda

1. 🖱️ W konsoli AWS sprawdź region: **Europe (Stockholm) `eu-north-1`** (prawy górny róg).
2. ⌨️ W wyszukiwarce wpisz `Lambda` i 🖱️ wejdź w usługę **Lambda**.
3. 🖱️ Kliknij **Create function**.
4. 🖱️ Wybierz opcję **Author from scratch**.
5. ⌨️ Wypełnij:
   - **Function name:** `ksiega-gosci-zespolX`
   - **Runtime:** wybierz **Python 3.12**
   - **Architecture:** zostaw **x86_64**
6. 🖱️ Kliknij **Create function**. Po chwili zobaczysz ekran swojej nowej funkcji.

---

## Krok 2: Wklej kod funkcji

1. 🖱️ Na ekranie funkcji przewiń do sekcji **Code source**. Zobaczysz edytor z plikiem `lambda_function.py`.
2. 🖱️ Usuń całą zawartość tego pliku i ⌨️ wklej **gotowy kod**, który dostarczył nauczyciel (plik `handler.py` z Księgi Gości).
3. ⚠️ **Ważne — nazwa handlera.** Gotowy kod ma funkcję startową `lambda_handler` w pliku `handler.py`. Domyślnie Lambda szuka pliku `lambda_function.py`. Masz dwie opcje:
   - **Prościej:** wklej kod do istniejącego `lambda_function.py` (nazwa pliku bez znaczenia, ważne że funkcja nazywa się `lambda_handler`), i zostaw ustawienie **Handler** jako `lambda_function.lambda_handler`.
   - **Zgodnie z repo:** zmień nazwę pliku na `handler.py` i w zakładce **Runtime settings → Edit** ustaw **Handler** na `handler.lambda_handler`.
4. 🖱️ Kliknij **Deploy** (nad edytorem), aby zapisać kod. Poczekaj na komunikat "Changes deployed".

---

## Krok 3: Ustaw zmienną środowiskową (połączenie z tabelą)

Kod nie ma w sobie nazwy tabeli na sztywno — odczytuje ją ze zmiennej środowiskowej `GUESTBOOK_TABLE`.

1. 🖱️ Przejdź do zakładki **Configuration → Environment variables**.
2. 🖱️ Kliknij **Edit → Add environment variable**.
3. ⌨️ Wpisz:
   - **Key:** `GUESTBOOK_TABLE`
   - **Value:** dokładna nazwa Twojej tabeli z ćwiczenia 0 (np. `ksiega-gosci-zespolX`)
4. 🖱️ Kliknij **Save**.

⚠️ Literówka w nazwie tabeli = błąd `500` przy każdym żądaniu. Skopiuj nazwę, nie przepisuj z pamięci.

---

## Krok 4: Nadaj Lambdzie uprawnienia do bazy (IAM)

Domyślnie Lambda nie ma prawa pisać do DynamoDB. Trzeba jej na to pozwolić.

1. 🖱️ Przejdź do zakładki **Configuration → Permissions**.
2. 🖱️ W sekcji **Execution role** kliknij nazwę roli (link otworzy konsolę IAM w nowej karcie).
3. 🖱️ W IAM kliknij **Add permissions → Create inline policy**.
4. 🖱️ Przełącz się na zakładkę **JSON** i ⌨️ wklej (podmień `NAZWA_TABELI` i `ID_KONTA`):

   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "AllowDynamoDB",
         "Effect": "Allow",
         "Action": ["dynamodb:PutItem", "dynamodb:Query"],
         "Resource": "arn:aws:dynamodb:eu-north-1:ID_KONTA:table/NAZWA_TABELI"
       }
     ]
   }
   ```

   > ID konta znajdziesz w prawym górnym rogu konsoli AWS (12 cyfr). Nazwę tabeli masz z ćwiczenia 0.
5. 🖱️ Kliknij **Next**, nadaj polityce nazwę np. `ksiega-gosci-dynamodb` i kliknij **Create policy**.

💡 Nasz kod używa tylko `PutItem` (zapis) i `Query` (odczyt listy). Nadajemy dokładnie tyle, ile trzeba — to zasada **najmniejszych uprawnień**.

---

## Krok 5: Przetestuj funkcję (bez strony!)

1. 🖱️ Wróć do karty z Lambdą. Przejdź do zakładki **Test**.
2. 🖱️ Wybierz **Create new event**, nazwij go np. `test-post`.
3. ⌨️ W polu **Event JSON** wklej udawane żądanie `POST` (tak wygląda event z API Gateway HTTP API):

   ```json
   {
     "requestContext": { "http": { "method": "POST" } },
     "body": "{\"author\": \"Jan Testowy\", \"message\": \"Pierwszy wpis z domu!\"}"
   }
   ```
4. 🖱️ Kliknij **Test**.

## ✅ Punkt kontrolny

- Wynik testu pokazuje **statusCode `201`** oraz treść `{"message": "Entry saved"}`.
- 🖱️ Wejdź w konsolę **DynamoDB → Tables → Twoja tabela → Explore table items**. Powinien tam być nowy wiersz z autorem `Jan Testowy`.
- (Opcjonalnie) Utwórz drugi event `test-get` z treścią `{ "requestContext": { "http": { "method": "GET" } } }` i sprawdź, czy zwraca `200` z listą wpisów.

## ⚠️ Najczęstsze błędy

- **`statusCode 500` + błąd o DynamoDB w logach** → brak uprawnień (krok 4) albo zła nazwa tabeli (krok 3).
- **`KeyError: 'GUESTBOOK_TABLE'`** → nie dodałeś zmiennej środowiskowej (krok 3). Po dodaniu kliknij **Deploy** jeszcze raz.
- **`statusCode 400 "Invalid JSON body"`** → w polu `body` eventu JSON musi być **tekstem** (stringiem), dlatego cudzysłowy są poprzedzone ukośnikiem `\"`. Skopiuj dokładnie jak wyżej.

📝 Zapisz w notatniku: funkcja działa, zostaje ją tylko wystawić na świat — to następne ćwiczenie.
