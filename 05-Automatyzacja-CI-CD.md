# Kto wdraża kod, gdy programista śpi (Automatyzacja, CI/CD i GitHub Actions)

## Problem: Ręczne wgrywanie plików to proszenie się o kłopoty

Dawniej, kiedy programista skończył pisać kod, musiał ręcznie połączyć się z serwerem (np. przez protokół FTP) i skopiować nowe pliki ze swojego komputera na serwer produkcyjny.

Ten proces był powolny i generował mnóstwo błędów. Wystarczyło, że inżynier zapomniał skopiować jeden plik, a cała aplikacja przestawała działać. Dodatkowo, jeśli w zespole pracowało 10 osób, pojawiał się chaos – kto, kiedy i co dokładnie wgrał na produkcję? Takie środowisko pracy generowało słynne tłumaczenie: "Dziwne, u mnie działa". Ręczne wgrywanie, czyli *manual deployment*, w nowoczesnym IT jest absolutnie niedopuszczalne.

## Rozwiązanie: CI/CD, czyli fabryka oprogramowania

Inżynierowie doszli do wniosku, że maszyny są znacznie lepsze w powtarzalnych zadaniach niż ludzie. Tak powstała koncepcja **CI/CD (Continuous Integration / Continuous Deployment)**, co w branży oznacza ciągłą integrację i ciągłe wdrażanie.

W tym modelu to nie człowiek loguje się do AWS, by wgrać nową wersję strony na bucket S3. Człowiek wysyła kod na GitHub (robi push lub zamyka Pull Request), a GitHub automatycznie uruchamia zaprogramowane wirtualne środowisko. To automatyczne środowisko pobiera najnowszy kod, sprawdza go, buduje gotową stronę, bezpiecznie autoryzuje się w AWS i samo umieszcza pliki w docelowym buckecie.

Do budowy tego mechanizmu wykorzystamy **GitHub Actions** – narzędzie wbudowane w GitHuba, które pozwala pisać instrukcje dla procesów automatyzacji za pomocą ustrukturyzowanych plików tekstowych.

## Bezpieczeństwo przede wszystkim (OIDC)

Skoro automatyczny proces z GitHuba ma wgrać pliki na Twój bucket w AWS, musi się tam najpierw zautoryzować. Kiedyś używano do tego statycznych kluczy (Access Keys). Niestety, programiści często przez pomyłkę publikowali je w otwartym kodzie, co szybko kończyło się przejęciem zasobów chmurowych przez złośliwe boty.

Wdrożymy to zgodnie z dzisiejszym standardem bezpieczeństwa: użyjemy mechanizmu **OIDC (OpenID Connect)**. Dzięki niemu GitHub i AWS komunikują się za pomocą krótkotrwałych, wirtualnych tokenów opartych na zaufaniu (Trust Relationship). W naszym kodzie nie będzie zaszytego ani jednego statycznego poświadczenia.

## Słowniczek pojęć (do zapamiętania przed zajęciami)

* **Pipeline:** Zautomatyzowana ścieżka (rurociąg), przez którą przechodzi kod – od momentu napisania przez inżyniera, aż do uruchomienia na infrastrukturze docelowej w chmurze.
* **Workflow:** Pojedynczy proces automatyzacji w GitHub Actions. Definiujemy w nim, *kiedy* ma się uruchomić (np. gdy ktoś zrobi merge do gałęzi `main`) i *co* ma dokładnie wykonać krok po kroku.
* **YAML:** Format zapisu danych, w którym będziemy pisać nasze pliki konfiguracyjne dla CI/CD. Opiera się na wcięciach (spacjach) i jest bardzo czytelny dla człowieka. Zamiast nawiasów klamrowych używa precyzyjnego formatowania wierszy.
* **OIDC (OpenID Connect):** Bezpieczny standard uwierzytelniania, pozwalający jednemu systemowi (GitHub) udowodnić innemu (AWS), że ma prawo wykonać określoną akcję (np. wgranie plików na bucket), bez użycia stałych haseł.

## Co będziemy z tym robić na zajęciach?

Na kolejnej lekcji utworzycie w swoim repozytorium folder `.github/workflows/`, a w nim plik YAML, który dostarczę. Będziecie musieli edytować w nim tylko dwie linijki: podać numer (ARN) specjalnej roli IAM z Waszego konta AWS oraz dokładną nazwę Waszego bucketu S3.

Po zatwierdzeniu tych zmian, GitHub Actions przy każdym nowym commicie zainstaluje program MkDocs, wygeneruje z Waszych plików Markdown kod HTML, użyje OIDC do autoryzacji w koncie AWS i automatycznie zsynchronizuje pliki na buckecie S3. W ten sposób zbudujecie i uruchomicie swój pierwszy w pełni funkcjonalny Pipeline CI/CD.

## Ważne: nowy format identyfikatora repozytorium (immutable subject claims)

Rola IAM w AWS ufa GitHubowi na podstawie tzw. `sub` (subject) — identyfikatora, który GitHub wysyła w tokenie OIDC. Repozytoria utworzone po **15 lipca 2026** używają nowego, „niezmiennego" formatu, który zawiera **numeryczne ID organizacji i repozytorium**:

```
repo:<organizacja>@<ID_ORG>/<repozytorium>@<ID_REPO>:environment:<środowisko>
```

Przykład (prawdziwe demo):

```
repo:chmurownia@331737546/demo@1391044409:environment:demo
```

Jeśli w polityce zaufania roli użyjecie starego formatu (`repo:organizacja/repo:*`), autoryzacja **nie zadziała** — zobaczycie błąd:

```
Not authorized to perform sts:AssumeRoleWithWebIdentity
```

### Jak znaleźć swoje numeryczne ID

Najprościej odczytać cały `sub` bezpośrednio z GitHuba. Wejdźcie w repozytorium:
**Settings → Actions → General** (sekcja dot. OIDC) — GitHub pokazuje tam dokładny identyfikator `sub`, który wysyła. Skopiujcie go i wklejcie do polityki zaufania roli IAM (pole `token.actions.githubusercontent.com:sub`).

Alternatywnie ID repozytorium można pobrać z API:

```bash
gh api repos/CHMUROWNIA/NAZWA_REPO --jq '{owner_id: .owner.id, repo_id: .id}'
```

### Dlaczego używamy `environment`

W workflow ustawiamy `environment: <nazwa>` (np. `demo`). Dzięki temu token GitHuba zawiera `:environment:<nazwa>` w `sub`, a my możemy ograniczyć zaufanie roli tylko do wdrożeń z tego konkretnego środowiska — to bezpieczniejsze niż zaufanie całemu repozytorium.
