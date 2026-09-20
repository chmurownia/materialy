# Jak ożywić statyczną stronę: API, Serverless i Bazy NoSQL

## Problem: Strona, która tylko "wygląda"

W pierwszej części projektu zbudowaliście wizytówkę i wdrożyliście ją na S3. Problem polega na tym, że jest to tzw. strona statyczna – składa się z gotowych plików HTML wysyłanych do użytkownika. Nie potrafi ona przyjmować nowych danych, takich jak komentarze, ponieważ S3 to tylko magazyn plików, a nie "mózg" aplikacji.

Tradycyjnym rozwiązaniem byłoby wynajęcie całego serwera (np. wirtualnej maszyny EC2), zainstalowanie na nim systemu operacyjnego, bazy danych SQL i napisanie aplikacji (np. w PHP czy Java). Taki serwer musiałby działać i kosztować 24 godziny na dobę, nawet jeśli Księgę Gości odwiedzałyby 3 osoby w tygodniu.

## Rozwiązanie 1: Kod na żądanie (AWS Lambda)

Zamiast stawiać serwer, skorzystamy z architektury **Serverless**. AWS Lambda pozwala wgrać sam kod (np. napisany w języku Python) do chmury. Ten kod uruchamia się wyłącznie wtedy, gdy jest potrzebny, wykonuje zadanie w kilka milisekund, a potem "usypia". Płacisz tylko za ułamek sekundy, w którym kod faktycznie pracował. W naszym projekcie Lambda będzie sercem systemu – jej zadaniem będzie odebranie nowej wiadomości i zapisanie jej.

## Rozwiązanie 2: Drzwi frontowe dla danych (Amazon API Gateway)

Zwykły użytkownik Internetu nie ma prawa wywołać Waszej funkcji Lambda bezpośrednio – chronią ją twarde reguły bezpieczeństwa (IAM). Potrzebujemy "drzwi frontowych" i cyfrowego recepcjonisty. Tym recepcjonistą jest **API Gateway**. Tworzy on publiczny adres URL (Endpoint). Kiedy formularz na Waszej stronie wyśle na ten adres dane (np. imię i wiadomość), API Gateway przepuści je przez "drzwi" i bezpiecznie przekaże do Waszej Lambdy.

## Rozwiązanie 3: Baza na sterydach (Amazon DynamoDB)

Kod wykonał swoje zadanie, ale gdzieś musi trwale zapisać wiadomość. Zamiast budować klasyczną bazę relacyjną (SQL), w której trzeba tworzyć sztywne tabele z określonymi kolumnami i typami danych, użyjemy bazy **NoSQL** – Amazon DynamoDB. To ekstremalnie szybka, elastyczna i bezserwerowa baza danych. Zapisujemy w niej informacje w formie prostych "dokumentów" (par klucz-wartość).

## Słowniczek pojęć (do zapamiętania przed zajęciami)

* **API (Application Programming Interface):** Interfejs, przez który systemy komputerowe rozmawiają ze sobą, bez udziału człowieka.
* **Endpoint:** Dokładny adres internetowy (URL), wystawiony przez API Gateway, na który należy wysłać dane, aby wyzwolić konkretną akcję (np. `[https://wasze-api.amazonaws.com/KsiegaGosci](https://wasze-api.amazonaws.com/KsiegaGosci)`).
* **JSON (JavaScript Object Notation):** Lekki format tekstowy do przesyłania danych. Systemy używają go do wymiany informacji, ponieważ jest zrozumiały zarówno dla przeglądarek, Pythona (w Lambdzie), jak i baz danych (DynamoDB). Wygląda jak lista właściwości ujęta w klamry, np.: `{"author": "Janek", "message": "Hej!"}`.
* **CORS (Cross-Origin Resource Sharing):** Zabezpieczenie wbudowane w przeglądarki internetowe. Domyślnie blokuje ono stronie WWW możliwość wysyłania zapytań do API znajdującego się pod innym adresem niż sama strona. Będziemy musieli je poprawnie skonfigurować w API Gateway, inaczej formularz nie zadziała.

## Co będziemy z tym robić na zajęciach?

Za pomocą konsoli AWS wyklikacie własne API Gateway, Lambdę oraz tabelę w DynamoDB. Przypiszecie im odpowiednie uprawnienia, aby usługi mogły się ze sobą komunikować. Następnie wkleicie przygotowany skrypt w JavaScript na swoją stronę zbudowaną w MkDocs.

W ostatniej fazie zostaniecie hakerami w białych kapeluszach. Wymienicie się swoimi Endpointami (adresami API) z innymi zespołami w klasie i użyjecie narzędzi sieciowych, aby komunikować się bezpośrednio z ich bazami danych – wysyłając im wiadomości z ominięciem ich strony internetowej, wyłącznie za pomocą architektury API.

