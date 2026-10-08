# Co robić, gdy system "leży" (Amazon CloudWatch, logi i wielki finał)

## Problem: Aplikacja działa w ukryciu, a błędów nie widać

Wasz system jest już w pełni spięty, ale ma pewną niewygodną cechę: całość dzieje się niewidocznie, gdzieś w chmurze. Kiedy użytkownik zobaczy komunikat "coś poszło nie tak", a formularz zwróci błąd `500`, nie macie żadnego ekranu, na który można by zajrzeć. Lambda uruchomiła się, coś poszło źle i uśpiła się z powrotem. Gdzie szukać przyczyny?

W tradycyjnym programowaniu, gdy coś się psuło, programista widział błąd od razu na ekranie. W świecie Serverless kod wykonuje się na maszynie, do której nie macie dostępu i która znika zaraz po zakończeniu pracy. Potrzebujemy "czarnej skrzynki", która zapisze, co dokładnie działo się w środku funkcji.

## Rozwiązanie: Amazon CloudWatch (czarna skrzynka aplikacji)

**Amazon CloudWatch** to usługa, która automatycznie zbiera **logi** – zapisy tego, co robiła Wasza Lambda przy każdym wywołaniu. Za każdym razem, gdy funkcja się uruchamia, cokolwiek wypisze (również komunikaty o błędach), trafia do CloudWatch i jest tam przechowywane. To Wasz dziennik pokładowy.

Logi są pogrupowane w tzw. **grupy logów (Log Groups)**. Logi Waszej funkcji znajdziecie w grupie o nazwie zaczynającej się od `/aws/lambda/` i dalej nazwa Waszej funkcji. Wewnątrz grupy znajdują się **strumienie (Log Streams)** – kolejne "kartki" dziennika, uporządkowane w czasie.

Co ważne, dobrze napisany kod sam dba o te zapisy. Nasza funkcja używa modułu `logging` – przy każdym zapisanym wpisie odnotowuje, kto go dodał, a w razie problemu z bazą zapisuje pełny opis błędu Pythona. Dzięki temu w logach widać nie tylko *że* coś się zepsuło, ale *dokładnie co i gdzie*.

## Jak czytać log błędu

Gdy do Lambdy trafi zniekształcone żądanie, w logach pojawi się tzw. **traceback** – ślad błędu Pythona pokazujący, w której linii kodu coś się posypało. Każdy wpis w CloudWatch ma też precyzyjny **znacznik czasu (timestamp)** – co do sekundy wiadomo, kiedy błąd wystąpił. To bezcenne, gdy trzeba połączyć skargę użytkownika ("przestało działać koło 14:30") z konkretnym zdarzeniem w logach.

## Słowniczek pojęć (do zapamiętania przed zajęciami)

* **CloudWatch:** Usługa AWS do zbierania logów, metryk i monitorowania działania aplikacji.
* **Log (dziennik):** Zapis zdarzeń i komunikatów generowanych przez działający kod.
* **Log Group:** Grupa logów jednej usługi, np. `/aws/lambda/nazwa-funkcji`.
* **Log Stream:** Pojedynczy strumień wpisów wewnątrz grupy, uporządkowany w czasie.
* **Timestamp (znacznik czasu):** Dokładny moment (data i godzina), w którym zapisano dany wpis w logach.
* **Traceback:** Ślad błędu w Pythonie wskazujący, gdzie w kodzie wystąpił problem.
* **Błąd 500:** Kod odpowiedzi oznaczający błąd po stronie serwera (coś poszło nie tak w Lambdzie).

## Co będziemy z tym robić na zajęciach?

W pierwszej części otworzycie w konsoli AWS logi swojej Lambdy w usłudze CloudWatch i odnajdziecie grupę logów Waszej funkcji. Następnie ja – jako prowadzący – celowo wyślę do Waszego API zniekształcony JSON, aby wywołać błąd. Waszym zadaniem detektywistycznym będzie odnaleźć w logach CloudWatch ślad tego błędu i podać mi dokładną godzinę jego wystąpienia.

Na finał nastąpi wzajemna prezentacja projektów na rzutniku. Każdy zespół pokaże swoją działającą Księgę Gości i opowie, co dzieje się "pod maską" – od formularza, przez API Gateway i Lambdę, aż po DynamoDB. Wspólnie omówimy, czym architektura chmurowa, którą zbudowaliście, różni się od tradycyjnych szkolnych zadań z programowania.
