# Gdzie trzymać stronę i jak serwować ją światu (Amazon S3 i CloudFront)

## Problem: Tradycyjny serwer to wąskie gardło

Gdy wpisujesz adres strony w przeglądarce, Twój komputer łączy się z innym komputerem (serwerem), aby pobrać z niego pliki (HTML, tekst, zdjęcia). Jeśli tradycyjny serwer stoi w Polsce, odwiedzający z Warszawy zobaczy stronę natychmiast. Ale jeśli na Twoją stronę wejdzie ktoś z Japonii czy Australii, dane muszą pokonać tysiące kilometrów przez światłowody na dnie oceanów. To trwa. Strona ładuje się wolno, a użytkownik się irytuje.

Co więcej, tradycyjne serwery to po prostu maszyny z systemem operacyjnym (np. Linux lub Windows). Trzeba o nie dbać, aktualizować je, resetować i chronić przed hakerami. Wynajmowanie serwera po to, aby wyświetlać prostą stronę-wizytówkę, to jak kupowanie 18-kołowej ciężarówki, żeby dowieźć jedną pizzę.

## Rozwiązanie 1: Amazon S3 (Magazyn bez dna)

Zamiast stawiać serwer, inżynierowie chmurowi używają **Object Storage** (magazynu obiektowego). Amazon S3 (Simple Storage Service) to usługa przypominająca nieskończenie pojemny dysk twardy, ale całkowicie pozbawiona systemu operacyjnego. Nie można na nim zainstalować programów ani gier. Służy wyłącznie do jednego: wrzucania i pobierania plików.

Dla naszej strony WWW to rozwiązanie idealne. Wrzucamy do S3 gotowe pliki z kodem, a infrastruktura AWS zajmuje się całą resztą. Nie martwimy się o awarie dysków, brak miejsca czy wirusy. S3 może przechować absolutnie wszystko – od małego pliku tekstowego po tysiące terabajtów filmów. (Ciekawostka: Netflix przechowuje wszystkie swoje seriale i filmy właśnie w Amazon S3!).

## Rozwiązanie 2: Amazon CloudFront (Globalny kurier)

Rozwiązaliśmy problem drogiego serwera, ale co z użytkownikami z Japonii? Tutaj wkracza Amazon CloudFront, czyli usługa typu **CDN (Content Delivery Network)**.

Wyobraź sobie, że CloudFront to globalna sieć błyskawicznie szybkich "kiosków", rozrzuconych w ponad 600 lokalizacjach na całym świecie. Gdy ktoś z Tokio wchodzi na Twoją stronę po raz pierwszy, CloudFront błyskawicznie pobiera ją z Twojego głównego magazynu S3 w Europie i robi jej wierną kopię w swoim japońskim kiosku. Kiedy kolejny Japończyk zechce zobaczyć Twoją stronę, otrzyma ją natychmiast z lokalnego kiosku w Tokio. Twoje S3 w Europie nawet nie zauważy tego ruchu. Ten sprytny mechanizm nazywamy **cache'owaniem** (buforowaniem).

## Słowniczek pojęć (do zapamiętania przed zajęciami)

* **Bucket:** Twój prywatny kontener w usłudze S3, do którego będziesz wrzucać pliki. Nazwa każdego bucketu w AWS musi być unikalna na skalę światową (jak adres e-mail).
* **Obiekt:** Każdy pojedynczy plik wrzucony do S3 (np. `cv.html` lub zdjęcie). W S3 nie ma klasycznych "plików", są tylko obiekty.
* **CDN (Content Delivery Network):** Sieć serwerów rozrzuconych po całym świecie, których zadaniem jest trzymanie kopii Twojej strony jak najbliżej użytkowników końcowych.
* **Cache (Pamięć podręczna):** Zapisana, tymczasowa kopia Twoich plików w usłudze CloudFront.

## Co będziemy z tym robić na zajęciach?

Na kolejnej lekcji zalogujecie się do konsoli AWS i stworzycie własny bucket w S3. Zgodnie z żelazną zasadą najwyższego bezpieczeństwa, **całkowicie zablokujemy do niego publiczny dostęp**. Nikt z zewnątrz nie będzie mógł wejść na Wasze S3 przez przeglądarkę. Następnie uruchomicie usługę CloudFront i wydacie jej specjalną "przepustkę" (OAC), aby tylko ona mogła czytać pliki z Waszego ukrytego magazynu i bezpiecznie serwować je na świat.
