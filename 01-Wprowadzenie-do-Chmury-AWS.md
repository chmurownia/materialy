# Wprowadzenie do Chmury AWS: Od blaszaka do globalnej sieci

## Problem: Jak to robiono kiedyś?

Wyobraź sobie, że masz świetny pomysł na aplikację. Jeszcze kilkanaście lat temu oznaczało to konieczność zakupu fizycznego serwera – drogiego, ciężkiego komputera, który musiał stać w klimatyzowanym pomieszczeniu z niezawodnym łączem internetowym.

Jeśli Twoja aplikacja zyskiwała nagłą popularność (np. w weekend), serwer zawieszał się z przeciążenia. Musiałeś kupić kolejny, czekać tygodnie na dostawę i samodzielnie go skonfigurować. A co, jeśli po miesiącu popularność spadała? Zostawałeś z drogim, bezużytecznym sprzętem pobierającym prąd. Ten model był wolny, drogi i bardzo ryzykowny dla nowych projektów.

## Rozwiązanie: Czym jest chmura AWS?

Chmura obliczeniowa (Cloud Computing) to w dużym uproszczeniu **wynajmowanie cudzych komputerów i usług przez Internet**. Zamiast kupować własny sprzęt, wypożyczasz moc obliczeniową, miejsce na dysku lub gotowe bazy danych od globalnego dostawcy, jakim jest Amazon Web Services (AWS).

Działa to dokładnie tak samo, jak prąd w gniazdku (model **Pay-as-you-go**). Nie musisz budować własnej elektrowni, by włączyć światło w pokoju. Po prostu wkładasz wtyczkę i płacisz rachunek tylko za to, co faktycznie zużyłeś. W AWS możesz uruchomić potężny serwer w 30 sekund, a po dwóch godzinach go usunąć, płacąc za to ułamki centów. Z rozwiązań AWS korzystają dziś giganci tacy jak Netflix, Spotify czy Epic Games (twórcy Fortnite). Podczas tych warsztatów będziecie pracować dokładnie na tych samych narzędziach.

## Słowniczek pojęć (do zapamiętania przed zajęciami)

* **Chmura publiczna:** Usługi IT dostarczane przez zewnętrznego dostawcę przez Internet, z których każdy może skorzystać i wynająć fragment dla siebie.
* **AWS Region:** Fizyczna, bezpieczna lokalizacja na świecie (np. Sztokholm, Frankfurt, Irlandia), składająca się z ogromnych budynków pełnych serwerów (Data Center). My w naszym projekcie będziemy pracować w regionie w Sztokholmie (oznaczonym w systemie jako `eu-north-1`).
* **Zasada najmniejszych uprawnień (Least Privilege):** Złota reguła bezpieczeństwa w chmurze. W AWS żaden program, usługa ani użytkownik nie ma prawa zrobić absolutnie niczego, dopóki wyraźnie mu na to nie zezwolimy (nie napiszemy odpowiedniej polityki uprawnień).
* **Serverless (Architektura bezserwerowa):** Najnowocześniejsze podejście do chmury. Nie wynajmujemy tu całych serwerów, którymi trzeba zarządzać, aktualizować na nich system czy dbać o dyski. Zlecamy chmurze tylko konkretne zadanie (np. przechowanie pliku lub wykonanie 10 linijek kodu). AWS dba o to, by system działał i nie zawiesił się, niezależnie od tego, czy stronę odwiedzi 10 osób, czy milion.

## Co będziemy z tym robić na zajęciach?

Nie będziemy instalować żadnych systemów operacyjnych. Skupimy się od razu na architekturze Serverless. Wykorzystamy chmurę jako inteligentny magazyn plików, który w ułamku sekundy zaserwuje naszą statyczną stronę WWW użytkownikom na całym świecie. Będziemy wyklikiwać usługi z poziomu konsoli przeglądarki, łącząc je ze sobą jak klocki Lego.
