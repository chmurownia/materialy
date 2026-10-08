# Serce aplikacji bez serwera (AWS Lambda i boto3)

## Problem: Statyczna strona nie ma "mózgu"

Wasza strona z Projektu 1 potrafi już wyświetlać treści, ale jest całkowicie bierna. Kiedy ktoś wpisze wiadomość w formularzu Księgi Gości, nie ma komu jej odebrać, przetworzić ani zapisać. S3 to magazyn plików – nie potrafi wykonywać kodu, podejmować decyzji ani rozmawiać z bazą danych.

Tradycyjnie potrzebowalibyśmy serwera aplikacyjnego, który działa non-stop i czeka na żądania. Taki serwer trzeba utrzymywać, aktualizować i opłacać przez 24 godziny na dobę – nawet w nocy, gdy nikt nie wchodzi na stronę. Dla Księgi Gości, która dostaje kilka wpisów tygodniowo, to kompletne marnotrawstwo.

## Rozwiązanie: Kod uruchamiany na żądanie (AWS Lambda)

**AWS Lambda** to usługa typu **Serverless** (bezserwerowa). Wgrywamy do niej sam kod funkcji (u nas: w języku Python), a AWS sam dba o maszynę, system operacyjny i jego aktualizacje. Funkcja "śpi", dopóki ktoś jej nie wywoła. Gdy nadchodzi żądanie, Lambda budzi się w ułamku sekundy, wykonuje zadanie i znów zasypia. Płacicie wyłącznie za milisekundy faktycznego działania kodu.

W naszym projekcie Lambda jest sercem systemu. Jej zadaniem jest:

* odebrać żądanie (dodanie wpisu albo prośbę o listę wpisów),
* sprawdzić poprawność danych (czy ktoś podał autora i treść),
* zapisać wpis w bazie DynamoDB lub odczytać z niej listę wpisów,
* zwrócić odpowiedź w formacie JSON.

## Jak Lambda rozmawia z bazą danych (boto3)

Kod w Pythonie nie łączy się z DynamoDB "ręcznie". Używa do tego oficjalnej biblioteki AWS o nazwie **boto3**. To zestaw gotowych narzędzi, dzięki którym jedną linijką kodu można zapisać wpis (`put_item`) albo pobrać listę wpisów (`query`). boto3 jest już zainstalowane w środowisku Lambdy – nie trzeba go dołączać samodzielnie.

Nasza funkcja odczytuje nazwę tabeli nie "na sztywno" z kodu, lecz ze **zmiennej środowiskowej** `GUESTBOOK_TABLE`. Dzięki temu ten sam kod może działać z różnymi tabelami – wystarczy zmienić ustawienie w konfiguracji Lambdy, bez dotykania samego kodu.

## Jak Lambda wie, co ma zrobić (event i routing)

Przy każdym wywołaniu Lambda dostaje tzw. **event** – obiekt JSON opisujący, co się stało. W naszym przypadku zawiera on między innymi metodę HTTP (`GET` lub `POST`) oraz treść żądania. Nasz kod zagląda do pola `requestContext.http.method` i na tej podstawie decyduje:

* `GET` → zwróć listę najnowszych wpisów,
* `POST` → zapisz nowy wpis,
* inna metoda → zwróć błąd "metoda niedozwolona".

Taki mechanizm wybierania właściwej akcji na podstawie żądania nazywamy **routingiem** (trasowaniem).

## Słowniczek pojęć (do zapamiętania przed zajęciami)

* **Serverless:** Model, w którym nie zarządzamy żadnym serwerem. Dostarczamy sam kod, a chmura uruchamia go na żądanie i rozlicza za czas wykonania.
* **Funkcja Lambda:** Pojedynczy fragment kodu wgrany do AWS, który uruchamia się w odpowiedzi na zdarzenie (np. żądanie z API Gateway).
* **Runtime:** Środowisko uruchomieniowe, w którym działa kod. U nas jest to `python3.12`.
* **boto3:** Oficjalna biblioteka Pythona do komunikacji z usługami AWS (np. DynamoDB, S3).
* **Zmienna środowiskowa:** Ustawienie przekazywane do funkcji z zewnątrz (np. `GUESTBOOK_TABLE`), dzięki któremu konfigurujemy kod bez jego edytowania.
* **Event:** Obiekt JSON opisujący zdarzenie, które wywołało funkcję. Zawiera dane żądania (metodę, treść, nagłówki).
* **Handler:** Funkcja startowa, od której AWS zaczyna wykonanie kodu. U nas nazywa się `lambda_handler`.
* **Kod statusu HTTP:** Liczba opisująca wynik operacji: `200`/`201` (sukces), `400` (błędne dane), `405` (zła metoda), `500` (błąd serwera).

## Co będziemy z tym robić na zajęciach?

Utworzycie w konsoli AWS funkcję Lambda w języku Python i wkleicie przygotowany przeze mnie kod obsługujący Księgę Gości. Następnie dodacie zmienną środowiskową `GUESTBOOK_TABLE`, wskazując na tabelę DynamoDB utworzoną w poprzednim tygodniu, oraz upewnicie się, że rola wykonawcza Lambdy ma uprawnienia `dynamodb:PutItem` i `dynamodb:Query`.

Na koniec przetestujecie funkcję bez udziału strony internetowej. W konsoli Lambdy utworzycie **testowy event** – fragment JSON udający żądanie `POST` z fikcyjną wiadomością – i uruchomicie go ręcznie. Jeśli funkcja odpowie kodem `201` i w tabeli DynamoDB pojawi się nowy wpis, oznacza to, że serce systemu bije poprawnie.
