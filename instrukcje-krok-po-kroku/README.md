# Instrukcje krok po kroku — Projekt 2 (nadrabianie w domu)

Te instrukcje są dla Ciebie, jeśli opuściłeś/opuściłaś zajęcia i chcesz wykonać ćwiczenie samodzielnie w domu. Każdy plik odpowiada jednej lekcji z Projektu 2 i prowadzi Cię "klik po kliku" przez konsolę AWS i GitHub.

## Zanim zaczniesz

Upewnij się, że masz:

- **Dostęp do konsoli AWS** — login i hasło do konta szkolnego (to samo, którego używałeś w Projekcie 1).
- **Region ustawiony na `eu-north-1` (Sztokholm)** — sprawdź prawy górny róg konsoli AWS. Jeśli jest inny region, wszystkie Twoje zasoby "znikną", bo tworzone są per region.
- **Dostęp do repozytorium GitHub Twojego zespołu** (z Projektu 1).
- **Tabelę DynamoDB z lekcji 5** — jeśli jej nie masz, zacznij od instrukcji `00`.
- **Materiały teoretyczne** — przeczytaj najpierw odpowiedni plik z katalogu nadrzędnego (np. `07-Lambda-Logika-Aplikacji.md`), żeby rozumieć, *co* robisz, a nie tylko klikać.

## Konwencje w instrukcjach

- 🖱️ = kliknij / wykonaj akcję w interfejsie
- ⌨️ = wpisz lub wklej tekst
- ✅ = punkt kontrolny: sprawdź, czy wszystko się zgadza, zanim przejdziesz dalej
- ⚠️ = najczęstszy błąd w tym miejscu

Nazwy, których używamy w przykładach (podmień na swoje, jeśli ustaliliście inne):

| Co | Przykładowa nazwa |
| --- | --- |
| Region | `eu-north-1` |
| Tabela DynamoDB | `ksiega-gosci-zespolX` |
| Funkcja Lambda | `ksiega-gosci-zespolX` |
| API Gateway | `ksiega-gosci-zespolX` |
| Zmienna środowiskowa z nazwą tabeli | `GUESTBOOK_TABLE` |

Zastąp `zespolX` numerem swojego zespołu (np. `zespol3`).

## Kolejność ćwiczeń

1. **`00-DynamoDB-tabela.md`** — tabela bazy danych (jeśli jeszcze jej nie masz; normalnie robione na lekcji 5).
2. **`01-Lambda.md`** — funkcja Lambda z gotowym kodem (lekcja 6/07).
3. **`02-API-Gateway.md`** — publiczne API i CORS (lekcja 7/08).
4. **`03-Integracja-strony.md`** — podpięcie formularza na stronie (lekcja 8/09).
5. **`04-API-Economy.md`** — wysyłanie żądań do cudzego API przez cURL/Postman (lekcja 9/10).
6. **`05-CloudWatch-logi.md`** — czytanie logów i diagnozowanie błędów (lekcja 10/11).

Rób je po kolei — każde ćwiczenie zakłada, że poprzednie jest ukończone.

## Utknąłeś?

Jeśli coś nie działa, nie usuwaj wszystkiego w panice. Przejdź do ostatniego ✅ punktu kontrolnego, który Ci zadziałał, i sprawdź sekcję ⚠️ w danej instrukcji. Zapisz treść błędu (zrzut ekranu) i przynieś ją na następne zajęcia — diagnozowanie błędów to normalna część pracy inżyniera chmurowego.
