## Python Enhancement Proposals

PEP (Python Enhancement Proposals) to oficjalne dokumenty opisujące nowe funkcje, standardy, ulepszenia i procesy w języku Python. PEP-y stanowią podstawowy sposób proponowania zmian w języku i ekosystemie, a także są kluczowym źródłem informacji o standardach kodowania i najlepszych praktykach.

Tworzone są przez członków społeczności, a każdy PEP przechodzi przez proces akceptacji, który obejmuje recenzję techniczną, dyskusję w społeczności i finalne zatwierdzenie przez komitet kierujący językiem Python.

**Rodzaje PEPów**:

- **Standardowe** - Proponują zmiany w implementacji Pythona, np. wprowadzenie nowych funkcji, konstrukcji języka czy bibliotek.
- **Informacyjne** - Opisują zasady, dobre praktyki lub procesy bez wymuszania ich implementacji.
- **Procesowe** - Określają procesy zarządzania projektem Python, takie jak rozwój języka czy standardy pracy.

**Wybrane przykłady PEPów**:

- PEP 8 - styl kodowania

Definiuje podstawy formatowania kodu w Pythonie. Kluczowe zasady to wcięcia - używanie 4 spacji, nazewnictwo - klasy w stylu `CamelCase`, funkcje i zmienne w stylu `snake_case` czy białe znaki - brak spacji wokół nawiasów, np. `foo(a, b)` zamiast `foo( a, b )`.

- PEP 257 - docstringi

Definiuje zasady dokumentowania modułów, klas i funkcji. Każdy moduł, klasa i funkcja powinny mieć docstring. Docstringi powinny być zwięzłe, ale informatywne. Multiliniowe docstringi powinny zaczynać się od podsumowania na pierwszej linii.

- PEP 484 - typowanie

Wprowadza adnotacje typów, które pozwalają na precyzyjne określenie typów danych.

**Po co nam PEPy?**

1. Zapewniają jednolite standardy, dzięki czemu kod w Pythonie jest łatwiejszy do czytania i utrzymania.
2. Służą jako przewodnik dla początkujących programistów, wprowadzając ich w dobre praktyki.
3. Dzięki nim Python może rozwijać się w sposób kontrolowany i przemyślany, odpowiadając na potrzeby społeczności.


##  Narzędzia do analizy i formatowania kodu

**Linting** to kluczowy element nowoczesnego programowania, który pomaga utrzymać wysoki standard jakości kodu. Proces ten automatycznie analizuje kod źródłowy i identyfikuje błędy, problemy stylistyczne oraz potencjalne problemy logiczne. Jego główne cele to zwiększenie czytelności, spójności i niezawodności kodu.

Same zalety 😁

2. Otrzymujemy lepszej jakości kod.
2. Mamy możliwość wczesnego wykrywania błędów.
3. Zwiększamy produktywność naszego zespołu.
4. W dużych projektach ujednolica styl, co zwiększa łatwość utrzymywania kodu.
5. Eliminujemy problemy stylistyczne.

Na szczęście mamy pod ręką bogaty ekosystem narzędzi do analizy i formatowania, możliwe jest:

1. Automatyczne wykrywanie błędów i niezgodności ze standardami - narzędzia takie jak `flake8`, `pylint`, czy `mypy` pozwalają na szybkie wychwycenie problemów w kodzie.
2. Utrzymywanie spójnego stylu kodowania - narzędzia takie jak `black` i `isort` zapewniają jednolity styl kodu w całym projekcie.
3. Optymalizacja i czystość kodu - analizatory kodu pomagają usuwać redundancje, identyfikować problemy z wydajnością oraz utrzymywać dobre praktyki.

Na zajęciach skupimy się na narzędziu `ruff`, które integruje funkcjonalności różnych narzędzi:

| **Funkcja**                  | **`ruff`**              | **Odpowiadające narzędzie**      | **Opis**                                                                        |
|------------------------------|-------------------------|-----------------------------------|---------------------------------------------------------------------------------|
| **Sprawdzanie stylu kodu**   | ✅ Tak                  | `flake8`                         | Sprawdza zgodność ze standardami stylu, takimi jak PEP 8.                       |
| **Sortowanie importów**      | ✅ Tak                  | `isort`                          | Sortuje i grupuje importy według ustalonych zasad.                              |
| **Analiza typów statycznych**| ✅ Częściowo            | `mypy`                           | Ostrzega o niezgodnych typach w adnotacjach, ale nie zastępuje pełnej analizy.  |
| **Sprawdzanie błędów logicznych**| ✅ Tak               | `flake8-bugbear`, `pylint`       | Identyfikuje potencjalne błędy w logice programu, np. użycie `==` zamiast `is`. |
| **Formatowanie kodu**        | ✅ Ostrzeżenia          | `black`                          | Wskazuje problemy z formatowaniem zgodnie z PEP 8, ale nie formatuje kodu.      |
| **Analiza jakości kodu**     | ✅ Tak                  | `pylint`, `radon`                | Identyfikuje redundancje, długi kod, złożoność funkcji.                         |
| **Wsparcie dla docstringów** | ✅ Tak                  | `pydocstyle`                     | Sprawdza poprawność dokumentacji zgodnie z PEP 257.                             |
| **Wydajność**                | ✅ Ekstremalnie szybkie |                                  | Napisane w Rust, jest wielokrotnie szybsze niż `flake8` czy `pylint`.           |
| **Integracja w CI/CD**       | ✅ Tak                  | Wszystkie                        | Łatwo integruje się z procesami `CI/CD`.                                        |
| **Obsługa konfiguracji**     | ✅ Tak                  | Wszystkie                        | Obsługuje konfigurację w plikach `pyproject.toml`, `ruff.toml`, lub CLI.        |

Dodatkowe zalety tego narzędzia to:

1. Wydajność - napisane w Rust, jest wielokrotnie szybsze niż `flake8` czy `pylint`.
2. Łatwa integracja z CI/CD.
3. Obsługuje konfigurację w plikach `pyproject.toml`, `ruff.toml`, lub CLI.