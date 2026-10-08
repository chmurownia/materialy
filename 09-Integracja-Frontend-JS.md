# Gdy strona rozmawia z backendem (Integracja Frontend–API w JavaScript)

## Problem: Dwa oddzielne światy, które się nie znają

Macie już dwie gotowe części układanki, ale wciąż nie są one połączone. Z jednej strony stoi **frontend** – to, co widzi użytkownik: strona WWW z formularzem Księgi Gości, zbudowana w MkDocs i serwowana przez CloudFront. Z drugiej strony stoi **backend** – ukryty mechanizm z API Gateway, Lambdą i DynamoDB, który potrafi zapisywać i oddawać wpisy.

Problem w tym, że strona nic nie wie o istnieniu Waszego API. Formularz jest tylko ładnie wyglądającym polem tekstowym – po kliknięciu "Wyślij" nic się nie dzieje, bo nikt nie powiedział przeglądarce, gdzie ma zanieść dane.

## Rozwiązanie: JavaScript jako posłaniec

Łącznikiem między tymi światami jest **JavaScript** – język, który wykonuje się bezpośrednio w przeglądarce użytkownika. To on reaguje na kliknięcie przycisku, zbiera dane z formularza (imię i wiadomość), pakuje je w format **JSON** i wysyła na Wasz **Invoke URL** z API Gateway.

Do wysłania danych JavaScript używa wbudowanego mechanizmu **fetch**. Działa to jak wysłanie listu: skrypt tworzy żądanie `POST`, w kopercie umieszcza JSON `{"author": "...", "message": "..."}`, podaje adres (Wasz Endpoint) i wysyła. Po chwili wraca odpowiedź z backendu, a skrypt może odświeżyć listę wpisów, pobierając ją żądaniem `GET`.

## Jedna linijka, którą musicie podmienić

Dostarczony przeze mnie fragment HTML/JS jest gotowy do działania, ale ma jedno puste pole: zmienną `API_URL`. To tam wklejacie swój własny **Invoke URL** z poprzedniej lekcji. Jeśli zostawicie cudzy adres albo zrobicie literówkę, Wasz formularz będzie wysyłał dane do niewłaściwego (albo nieistniejącego) API.

```javascript
// Podmieńcie TYLKO tę linijkę na swój Invoke URL z API Gateway:
const API_URL = "https://wasze-api.execute-api.eu-north-1.amazonaws.com";
```

## Najczęstsze pułapki

* **Błąd CORS w konsoli przeglądarki.** Jeśli po wysłaniu wpisu w konsoli (klawisz F12) pojawia się komunikat o blokadzie CORS, oznacza to, że API Gateway nie zezwala na żądania z adresu Waszej strony. Trzeba wrócić do konfiguracji CORS z lekcji o API Gateway.
* **Zła metoda lub ścieżka.** Skrypt musi wysyłać `POST` na ścieżkę `/entries`. Literówka w adresie kończy się błędem `404` (nie znaleziono trasy).
* **Niezaktualizowany cache.** Pamiętajcie, że CloudFront buforuje stronę. Po wdrożeniu nowej wersji może być potrzebne odświeżenie lub chwila cierpliwości, zanim zobaczycie zmiany.

## Słowniczek pojęć (do zapamiętania przed zajęciami)

* **Frontend:** Warstwa aplikacji widoczna dla użytkownika – strona WWW wyświetlana w przeglądarce.
* **Backend:** Ukryta warstwa logiki i danych (API Gateway, Lambda, DynamoDB), z którą rozmawia frontend.
* **JavaScript (JS):** Język programowania działający w przeglądarce, który ożywia stronę i wysyła dane do API.
* **fetch:** Wbudowana w przeglądarkę funkcja JavaScript do wysyłania żądań HTTP do API.
* **Snippet:** Gotowy, krótki fragment kodu do wklejenia (u nas: HTML + JavaScript obsługujący formularz).
* **Konsola przeglądarki:** Narzędzie deweloperskie (klawisz F12), w którym widać błędy JavaScript i komunikaty CORS.

## Co będziemy z tym robić na zajęciach?

Na GitHubie wyedytujecie plik `ksiega.md` i wkleicie do niego gotowy snippet HTML/JS z formularzem Księgi Gości. Podmienicie w nim jedną zmienną – `API_URL` – na swój Invoke URL z API Gateway.

Zmianę zgłosicie jako **Pull Request**, zrobicie wzajemne code review w parze, zmergujecie do `main` i poczekacie, aż GitHub Actions automatycznie wdroży nową wersję strony (Pipeline z Projektu 1). Na końcu wejdziecie na swoją subdomenę i spróbujecie dodać wpis przez formularz. Jeśli wpis się zapisze i pojawi na liście – cała infrastruktura, od przeglądarki aż po bazę danych, jest poprawnie spięta.
