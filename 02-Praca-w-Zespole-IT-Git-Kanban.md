# Praca w Zespole IT: Jak nie zepsuć sobie nawzajem kodu (Git i Kanban)

## Problem: Syndrom "projekt_ostateczny_v3_final.zip"

Wyobraź sobie, że piszesz z kolegą z pary ważny projekt zaliczeniowy. Ty odpowiadasz za stronę główną, on za zakładkę kontaktową. Przesyłacie sobie pliki przez Messengera lub e-mail: `projekt.zip`, potem `projekt_poprawki.zip`, aż w końcu `projekt_ostateczny_gotowy_v3.zip`.

Nagle okazuje się, że obaj w tym samym czasie edytowaliście ten sam plik ze stylami CSS. Kiedy próbujecie to połączyć, ktoś bezpowrotnie nadpisuje i kasuje pracę drugiego. Zaczynają się kłótnie, a projekt przestaje działać. W profesjonalnym programowaniu, gdzie nad jednym systemem potrafi pracować kilkuset inżynierów, takie ręczne przesyłanie plików doprowadziłoby do całkowitego paraliżu firmy.

## Rozwiązanie: Wehikuł czasu i cyfrowa tablica

Branża IT rozwiązała ten problem za pomocą dwóch potężnych narzędzi, z których będziecie korzystać od pierwszej minuty naszych warsztatów:

**1. System kontroli wersji (Git) i GitHub**
Git to oprogramowanie, które działa jak wehikuł czasu dla plików tekstowych (kodu). Zapisuje historię każdej, najdrobniejszej zmiany – kto ją wprowadził, kiedy i dlaczego. GitHub to z kolei platforma w chmurze (taki społecznościowy dysk Google dla programistów), która pozwala przechowywać historię Gita w Internecie. Dzięki temu programiści mogą bezpiecznie pracować nad tym samym projektem w tym samym czasie, a system sam dba o płynne łączenie (mergowanie) ich zmian.

**2. Tablica Kanban (GitHub Projects)**
Zanim ktokolwiek napisze linijkę kodu, zespół musi wiedzieć, co właściwie ma do zrobienia. Kanban to japońska metoda zarządzania pracą, wymyślona w fabrykach Toyoty. Polega na wizualizacji zadań za pomocą wirtualnych "karteczek" przypinanych do kolumn: *Do zrobienia (To Do)*, *W trakcie (In Progress)* i *Zrobione (Done)*. Dzięki tablicy każdy w zespole wie, za co odpowiada, i nikt nie dubluje pracy kolegi.

## Słowniczek pojęć (do zapamiętania przed zajęciami)

* **Repozytorium (Repo):** Wasz główny, wirtualny folder na projekt na GitHubie. Przechowuje wszystkie pliki oraz pełną historię ich modyfikacji.
* **Issue (Zadanie/Bilet):** Pojedyncze, konkretne zadanie do wykonania opisane w systemie (np. "Napisać tekst na stronę o mnie"). Z Issues buduje się tablicę Kanban.
* **Commit:** Pojedynczy "zapis gry". Paczka zmian w kodzie, którą wysyłacie do historii repozytorium opatrzona krótkim komentarzem (np. *Dodano zdjęcie do wizytówki*).
* **Gałąź (Branch):** Równoległa, bezpieczna kopia projektu. Zamiast psuć główną, działającą wersję strony (zwaną gałęzią `main`), tworzycie własną gałąź, by tam swobodnie eksperymentować.
* **Pull Request (PR):** Prośba o dołączenie Waszego kodu do głównego projektu. Zgłaszacie PR, gdy skończycie zadanie na swojej gałęzi. To najważniejszy moment współpracy – druga osoba z pary musi przejrzeć Wasz kod (Code Review) i go zatwierdzić (Approve), zanim zmiany trafią na produkcję.

## Co będziemy z tym robić na zajęciach?

Zapomnijcie o ręcznym przesyłaniu plików. Na najbliższej lekcji sklonujecie puste repozytorium. Skonfigurujecie wirtualną tablicę Kanban i podzielicie między siebie pierwsze zadania. Następnie każdy z Was napisze krótki tekst wizytówki w języku Markdown na osobnej gałęzi, zgłosi swój pierwszy Pull Request i poprosi drugą osobę o jego zatwierdzenie.
