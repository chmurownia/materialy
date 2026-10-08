# Ćwiczenie 5: Czytanie logów i diagnozowanie błędów (Amazon CloudWatch)

> **Teoria:** przeczytaj najpierw `../11-CloudWatch-Logi-Finał.md`.
>
> **Wymagania wstępne:** ukończone ćwiczenie `02` (masz działające API i Lambdę).
>
> **Czas:** ok. 20 minut.

## Cel

Nauczysz się odnajdywać logi swojej Lambdy w CloudWatch, celowo wywołasz błąd `500` i odszukasz jego ślad (traceback) wraz z dokładną godziną wystąpienia.

---

## Krok 1: Wywołaj błąd celowo

Żeby było co diagnozować, najpierw „zepsujemy" jedno żądanie — wyślemy zniekształcone dane.

1. 🖱️ Otwórz terminal (lub Postman).
2. ⌨️ Wyślij żądanie `POST` z **nieprawidłowym** JSON (brak domykającego nawiasu — celowy błąd składni):

   ```bash
   curl -X POST "https://TWOJE-API.execute-api.eu-north-1.amazonaws.com/entries" \
     -H "Content-Type: application/json" \
     -d '{"author": "Test"'
   ```
3. 📝 **Zapisz dokładną godzinę** (co do minuty), o której wysłałeś to żądanie — przyda się do odnalezienia wpisu w logach.

> 💡 Zależnie od danych możesz dostać błąd `400` (sam kod to wychwyci) albo `500` (błąd po stronie serwera). Oba zostawiają ślad w logach — i o to chodzi w tym ćwiczeniu.

---

## Krok 2: Otwórz logi Lambdy w CloudWatch

Są dwie drogi — wybierz wygodniejszą.

**Droga A (z poziomu Lambdy — najszybsza):**
1. 🖱️ Wejdź w swoją funkcję w usłudze **Lambda**.
2. 🖱️ Zakładka **Monitor → View CloudWatch logs**. Otworzy się CloudWatch z grupą logów Twojej funkcji.

**Droga B (bezpośrednio w CloudWatch):**
1. ⌨️ W wyszukiwarce AWS wpisz `CloudWatch`, 🖱️ wejdź w usługę.
2. 🖱️ Menu po lewej: **Logs → Log groups**.
3. 🖱️ Znajdź grupę o nazwie `/aws/lambda/ksiega-gosci-zespolX`.

---

## Krok 3: Znajdź log błędu

1. 🖱️ Wejdź w grupę logów. Zobaczysz listę **Log streams** — każdy to osobny „strumień" wpisów, posortowane po czasie. Kliknij najnowszy (na górze).
2. Przeglądaj wpisy od najnowszych. Szukaj linii oznaczonych `ERROR` albo słowa `Traceback`.
3. **Traceback** to ślad błędu Pythona — pokazuje, w której linii kodu coś poszło nie tak i jaki to był typ błędu (np. `JSONDecodeError`).
4. 🖱️ Przy każdym wpisie po lewej widnieje **timestamp** (dokładny czas). Odszukaj wpis z godziny zbliżonej do tej, którą zapisałeś w kroku 1.

> 💡 Jeśli wpisów jest dużo, użyj pola **Filter events** i wpisz `ERROR`, żeby odfiltrować tylko błędy.

---

## ✅ Punkt kontrolny

Potrafisz wskazać w logach CloudWatch:

- grupę logów swojej Lambdy (`/aws/lambda/...`),
- konkretny wpis z błędem (`ERROR` / `Traceback`),
- **dokładny timestamp** (datę i godzinę co do sekundy), kiedy błąd się pojawił.

To jest dokładnie zadanie detektywistyczne z lekcji 10/11: na zajęciach nauczyciel wyśle zniekształcone żądanie do Twojego API, a Twoim zadaniem będzie podać mu godzinę błędu z logów.

## ⚠️ Najczęstsze błędy / wskazówki

- **Brak grupy logów** → Lambda musi zostać choć raz wywołana, żeby grupa powstała. Jeśli jej nie ma, uruchom najpierw test z ćwiczenia 1.
- **Nie widzę świeżego wpisu** → logi pojawiają się z kilkusekundowym opóźnieniem. Odśwież stronę (ikona 🔄) i sprawdź najnowszy log stream.
- **Zły region** → logi są per region. Upewnij się, że CloudWatch jest w `eu-north-1`.

---

## 🎓 Wielki finał

Gratulacje — przeszedłeś cały Projekt 2 samodzielnie! Masz działającą, bezserwerową aplikację:

**Przeglądarka → API Gateway → Lambda (Python) → DynamoDB**, z logami w CloudWatch.

Przygotuj krótką prezentację swojej Księgi Gości: pokaż działający formularz i opowiedz, co dzieje się „pod maską" na każdym etapie. Zastanów się, czym ta architektura chmurowa różni się od tradycyjnego serwera, który trzeba by utrzymywać 24/7.
