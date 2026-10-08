# Gdy aplikacje rozmawiają bez ludzi (API Economy i przyjazny atak)

## Problem: Strona to tylko jedna z wielu "twarzy" aplikacji

Do tej pory każdy wpis trafiał do Waszej bazy przez formularz na stronie. Łatwo o złudzenie, że strona WWW *jest* aplikacją. To nieprawda. Strona to tylko jedna z możliwych "twarzy" (interfejsów) Waszego systemu. Prawdziwym sercem jest API – i każdy, kto zna jego adres i reguły, może z niego korzystać bez otwierania przeglądarki.

W nowoczesnym IT firmy wcale nie wchodzą na swoje strony nawzajem. Ich systemy rozmawiają ze sobą bezpośrednio – maszyna z maszyną, przez API. Zjawisko, w którym firmy udostępniają sobie usługi i dane poprzez API, nazywamy **API Economy** (gospodarką opartą na API). Tak działają płatności, mapy, logowanie przez konto Google czy prognozy pogody w aplikacjach.

## Rozwiązanie: Rozmowa bezpośrednio z API

Na tej lekcji zostaniecie "hakerami w białych kapeluszach". Zamiast klikać w cudzy formularz, wyślecie żądania bezpośrednio do API innego zespołu – z pominięciem jego strony internetowej. To nie jest włamanie: API Gateway jest *celowo* publiczne, a Wy używacie go dokładnie tak, jak zaprojektowano, tyle że bez przeglądarki.

Do takiej rozmowy używa się dwóch popularnych narzędzi:

* **cURL** – program uruchamiany w terminalu, który jedną komendą wysyła żądanie HTTP.
* **Postman** – aplikacja z graficznym interfejsem, w której wygodnie klika się metodę, adres i treść żądania.

Przykładowe żądanie `POST` w cURL, wysyłające pozdrowienia prosto do cudzej bazy:

```bash
curl -X POST "https://ICH-API.execute-api.eu-north-1.amazonaws.com/entries" \
  -H "Content-Type: application/json" \
  -d '{"author": "Zespol 1", "message": "Pozdrowienia z drugiej lawki!"}'
```

Aby odczytać wszystkie wpisy (również te dodane przez innych), wystarczy żądanie `GET`:

```bash
curl "https://ICH-API.execute-api.eu-north-1.amazonaws.com/entries"
```

## Dlaczego to działa bez strony (i bez CORS)

Zwróćcie uwagę na ważną rzecz: cURL i Postman **nie są przeglądarkami**, więc nie obowiązuje ich zabezpieczenie CORS. CORS to reguła wymuszana wyłącznie przez przeglądarki. Dlatego wpis, którego formularz nie chciał wysłać z powodu błędu CORS, może bez problemu trafić do bazy przez cURL. To świetnie pokazuje, że CORS chroni *użytkownika przeglądarki*, a nie samo API – API jest publiczne i broni się czym innym (walidacją danych w Lambdzie oraz uprawnieniami IAM).

## Druga część: współpraca na GitHubie

API Economy to nie tylko dane, ale też współpraca nad kodem. W drugiej części lekcji wejdziecie na repozytorium innego zespołu, zrobicie **fork** (własną kopię), dodacie link do ich strony partnerskiej i zgłosicie **Pull Request**. Zespoły będą na żywo akceptować PR-y od innych grup, robiąc wzajemne code review – dokładnie tak, jak wygląda praca w prawdziwym projekcie open source.

## Słowniczek pojęć (do zapamiętania przed zajęciami)

* **API Economy:** Model, w którym systemy i firmy udostępniają sobie usługi oraz dane poprzez API, bez udziału człowieka.
* **cURL:** Narzędzie wiersza poleceń do wysyłania żądań HTTP (GET, POST i innych).
* **Postman:** Aplikacja z graficznym interfejsem do testowania i wysyłania żądań do API.
* **Haker w białym kapeluszu:** Osoba, która testuje systemy w sposób etyczny i za zgodą właściciela, aby lepiej zrozumieć ich działanie i bezpieczeństwo.
* **Fork:** Własna kopia cudzego repozytorium na GitHubie, w której można wprowadzać zmiany bez naruszania oryginału.
* **Pull Request (PR):** Prośba o włączenie Waszych zmian do cudzego repozytorium, poddawana code review.

## Co będziemy z tym robić na zajęciach?

Na początek wymienicie się między zespołami swoimi adresami API (Invoke URL) oraz linkami do repozytoriów GitHub. Następnie, używając cURL lub Postmana, wyślecie żądania `POST` z pozdrowieniami prosto do baz danych innych grup – z całkowitym pominięciem ich stron internetowych – oraz żądania `GET`, aby zobaczyć, co pojawiło się w cudzej Księdze Gości.

W drugiej części wejdziecie na GitHuba innej grupy, zforkujecie ich repozytorium i otworzycie Pull Request dodający link do ich strony partnerskiej. Na koniec zespoły będą na żywo akceptować nawzajem swoje PR-y, przeprowadzając wspólne code review.
