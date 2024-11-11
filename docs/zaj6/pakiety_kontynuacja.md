Na jednych z poprzednich zajęć omawialiśmy, jak tworzyć i organizować pakiety lokalne w Pythonie, aby kod był przejrzysty i łatwy do ponownego wykorzystania.

Istnieją jednak mechanizmy, które pozwalają na dalsze ulepszanie procesu zarządzania pakietami – umożliwiają one **instalowanie pakietów w trybie edytowalnym**, dzięki czemu wprowadzane zmiany są natychmiast dostępne bez potrzeby reinstalacji. Możemy również zarządzać zależnościami w sposób zautomatyzowany, budować gotowe do instalacji paczki, a nawet publikować nasze pakiety (np. w Python Package Index (PyPI), aby inne osoby mogły je zainstalować za pomocą `pip`).

## Definiowanie i rozwijanie pakietów lokalnie

Pracując nad pakietami, często chcemy zainstalować je w lokalnym środowisku, aby mieć możliwość testowania jego funkcji w miarę ich rozwoju. W tym celu możemy zdefiniować konfigurację pakietów w pliku`pyproject.toml`.

???- note "Czy można używać czegoś innego niż `pyproject.toml`?"

    Tradycyjnie używano pliku `setup.py`, a następnie dodano `setup.cfg` jako opcję konfiguracji bezpośredniej w formacie `.ini`. Obecnie, dzięki wprowadzeniu PEP 518 i PEP 621, możliwa jest konfiguracja pakietów wyłącznie za pomocą `pyproject.toml`, który upraszcza zarządzanie projektami w całym ekosystemie.
    
    W najnowszych wersjach `setuptools` wystarczy plik `pyproject.toml`, w poprzednich mogą być wymagane inne / pozostałe.    

    Więcej szczegółów w dokumentacji [setuptools](https://setuptools.pypa.io/en/latest/index.html).

### Przykład konfiguracji

Plik `pyproject.toml` powinien się znaleźć w głównym katalogu naszego projektu.

**Zawartość `pyproject.toml`** - przechowuje szczegółowe informacje o projekcie, takie jak nazwa, wersja, autor, oraz wymagania dla systemu budowania pakietu.

```python
[build-system]
requires = ["setuptools", "setuptools-scm"]
build-backend = "setuptools.build_meta"

[project]
name = "moje-pakiety"
version = "0.0.1"
requires-python = '>=3.8.0'
description = "Poszczegolne pakiety odpowiadajace przeprowadzonym zajeciom"
authors = [
    { name = "Jakub Staszel", email = "jstaszel@agh.edu.pl" }
]

[tool.setuptools.packages.find]
where = ["src"]
namespaces = false
```

### Popularne układy projektów

Aktualnie `setuptools` wspiera automatyczne przeszukiwanie 2 typów struktur projektów, na tych zajęciach stworzyliśmy `src-layout`, więcej szczegółów [tutaj](https://setuptools.pypa.io/en/latest/userguide/package_discovery.html#automatic-discovery).

### Instalacja w trybie edytowalnym

Żeby mieć dostęp do lokalnych pakietów w trakcie rozwijania całego projektu, wystarczy zainstalować pakiety w trybie edytowalnym za pomocą `pip install -e .`.