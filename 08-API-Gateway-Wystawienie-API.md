# Drzwi frontowe dla Waszego kodu (Amazon API Gateway i CORS)

## Problem: Do Lambdy nie da się zapukać z Internetu

Macie już działającą funkcję Lambda, ale jest ona ukryta głęboko w AWS i chroniona twardymi regułami bezpieczeństwa (IAM). Zwykła przeglądarka ani formularz na Waszej stronie nie mają prawa wywołać jej bezpośrednio. Lambda nie ma własnego publicznego adresu URL, pod który mógłby trafić ruch z Internetu.

Potrzebujemy czegoś, co stanie "na drzwiach frontowych": przyjmie żądanie z Internetu, sprawdzi je i bezpiecznie przekaże do Lambdy. Tym kimś jest cyfrowy recepcjonista.

## Rozwiązanie: Amazon API Gateway (recepcjonista aplikacji)

**Amazon API Gateway** tworzy publiczny adres URL (**Endpoint**), pod który świat może wysyłać żądania. Gdy ktoś wywoła ten adres, API Gateway przyjmuje żądanie i przekazuje je dalej do Waszej Lambdy, a jej odpowiedź odsyła z powrotem do nadawcy. Sama Lambda pozostaje ukryta – świat widzi tylko recepcjonistę.

W naszym projekcie użyjemy wariantu **HTTP API** – nowszego, szybszego i tańszego niż klasyczne REST API, idealnego do prostych aplikacji takich jak Księga Gości.

## Trasy i metody (routing na poziomie API)

API Gateway nie przepuszcza żądań na ślepo. Reaguje tylko na konkretne, zdefiniowane **trasy (routes)** – połączenie metody HTTP i ścieżki:

* `GET /entries` → pobranie listy wszystkich wpisów,
* `POST /entries` → dodanie nowego wpisu.

Obie trasy prowadzą do tej samej Lambdy. To już kod w Pythonie decyduje, co zrobić, patrząc na metodę żądania (routing z poprzedniej lekcji). Nowy wpis wysyłamy jako JSON w formacie `{"author": "...", "message": "..."}`.

## Dlaczego formularz nie zadziała bez CORS

Tutaj czeka na Was najczęstsza pułapka całego projektu. Przeglądarki mają wbudowane zabezpieczenie o nazwie **CORS (Cross-Origin Resource Sharing)**. Domyślnie blokuje ono stronie WWW wysyłanie żądań do API, które znajduje się pod innym adresem (inną domeną) niż sama strona.

Wasza strona działa pod adresem typu `https://zespol1.szkola.pl`, a API Gateway ma zupełnie inny adres (`...execute-api...amazonaws.com`). Dla przeglądarki to dwie różne domeny, więc bez wyraźnej zgody zablokuje ona połączenie, a w konsoli przeglądarki zobaczycie błąd CORS. Formularz "nie zadziała", mimo że Lambda i API są poprawne.

Rozwiązaniem jest skonfigurowanie CORS w API Gateway: wskazujemy, że żądania z domeny Waszej strony są dozwolone, oraz jakie metody (`GET`, `POST`, `OPTIONS`) i nagłówki (`Content-Type`) wolno przysyłać. Dopiero wtedy przeglądarka przepuści ruch z formularza.

## Jak API Gateway "ma prawo" wołać Lambdę

Samo wskazanie Lambdy to nie wszystko. Lambda jest domyślnie zamknięta, więc trzeba jej jawnie powiedzieć: "pozwalam, aby to konkretne API Gateway Cię wywoływało". To uprawnienie (tzw. Lambda permission / resource policy) nadaje się raz podczas konfiguracji integracji. Bez niego API Gateway dostanie od Lambdy odmowę, a użytkownik zobaczy błąd.

## Słowniczek pojęć (do zapamiętania przed zajęciami)

* **API Gateway:** Usługa AWS tworząca publiczny punkt wejścia do Waszej aplikacji i przekazująca żądania do Lambdy.
* **HTTP API:** Lekki, szybki i tani wariant API Gateway, którego używamy w projekcie.
* **Endpoint / Invoke URL:** Publiczny adres URL wystawiony przez API Gateway, na który wysyłamy żądania. To ten adres przekazujecie innym zespołom.
* **Trasa (Route):** Para metoda + ścieżka (np. `POST /entries`), na którą reaguje API Gateway.
* **Integracja:** Połączenie trasy w API Gateway z konkretnym "celem" – u nas z funkcją Lambda.
* **CORS:** Zabezpieczenie przeglądarki, które blokuje żądania między różnymi domenami, dopóki serwer wyraźnie na nie nie zezwoli.
* **Deploy / Stage:** Opublikowanie API, dzięki czemu Endpoint staje się aktywny i dostępny publicznie.

## Co będziemy z tym robić na zajęciach?

W konsoli AWS utworzycie API Gateway w wariancie **HTTP API** i podepniecie do niego funkcję Lambda z poprzedniej lekcji (integracja). Zdefiniujecie dwie trasy: `GET /entries` oraz `POST /entries`.

Następnie skonfigurujecie **CORS**, zezwalając na żądania z adresu Waszej strony i na metody `GET`, `POST`, `OPTIONS` oraz nagłówek `Content-Type`. Opublikujecie API (Deploy) i skopiujecie **Invoke URL**. To Wasz publiczny adres – zapiszcie go starannie w notatniku, bo w kolejnej lekcji wkleicie go na stronę, a później wymienicie się nim z innymi zespołami.
