# Ćwiczenie 0: Tabela bazy danych (Amazon DynamoDB)

> **Kiedy to robić:** tylko jeśli nie masz jeszcze tabeli DynamoDB z lekcji 5. Jeśli masz — przejdź od razu do `01-Lambda.md`.
>
> **Teoria:** przeczytaj najpierw sekcję o DynamoDB w pliku `../06-Ksiega-Gosci.md`.
>
> **Czas:** ok. 10 minut.

## Cel

Utworzysz pustą tabelę NoSQL, w której Lambda będzie zapisywać wpisy z Księgi Gości.

## Kroki

1. 🖱️ Zaloguj się do konsoli AWS i upewnij się, że w prawym górnym rogu masz region **Europe (Stockholm) `eu-north-1`**.
2. ⌨️ W górnym pasku wyszukiwania wpisz `DynamoDB` i 🖱️ wejdź w usługę **DynamoDB**.
3. 🖱️ W menu po lewej kliknij **Tables**, a potem przycisk **Create table**.
4. ⌨️ Wypełnij pola:
   - **Table name:** `ksiega-gosci-zespolX` (podmień `zespolX` na swój zespół).
   - **Partition key:** wpisz `pk`, typ pozostaw **String**.
   - 🖱️ Zaznacz opcję dodania **Sort key** i wpisz `sk`, typ **String**.

   ⚠️ Nazwy `pk` i `sk` muszą być dokładnie takie (małe litery). Gotowy kod Lambdy ich oczekuje — inna nazwa = błąd przy zapisie.
5. 🖱️ W sekcji **Table settings** zostaw **Default settings** (tryb rozliczania **On-demand** / PAY_PER_REQUEST — płacisz tylko za użycie).
6. 🖱️ Kliknij **Create table** na dole. Poczekaj, aż status zmieni się na **Active** (kilka-kilkadziesiąt sekund).

## ✅ Punkt kontrolny

- Na liście **Tables** widzisz swoją tabelę ze statusem **Active**.
- Klucz partycji to `pk` (String), klucz sortujący to `sk` (String).
- 📝 **Zapisz w notatniku dokładną nazwę tabeli** — będzie potrzebna w następnym ćwiczeniu.

## ⚠️ Najczęstsze błędy

- **Zły region.** Jeśli utworzysz tabelę w innym regionie niż Lambda, nie będą się "widzieć". Zawsze `eu-north-1`.
- **Literówka w kluczach.** `PK` to nie to samo co `pk`. Trzymaj się małych liter.
