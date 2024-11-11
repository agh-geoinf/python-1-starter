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

Żeby mieć dostęp do lokalnych pakietów w trakcie rozwijania całego projektu, wystarczy zainstalować pakiety w trybie edytowalnym za pomocą `pip install -e .` będąc w katalogu głównym projektu.

## Zarządzanie zależnościami pakietu

Tutaj sprawdzić jak się to ma do tworzenia środowisk wirtualnych i kolejnej sekcji.

## Budowanie pakietu

Budowanie pakietu to proces tworzenia dystrybucji kodu, która może być zainstalowana przez innych użytkowników lub na różnych maszynach. W Pythonie używa się narzędzi takich jak `setuptools` i `wheel`, aby stworzyć gotową paczkę w formatach `.whl` (wheel) i `.tar.gz` (source distribution).

Aby zbudować pakiet wystarczy mieć zainstalowany pakiet `build` ([dokumentacja](https://build.pypa.io/en/stable/)), a następnie uruchomić go poprzez `python -m build`. Pliki powinny zostać stworzone w folderze `dist`.

## Publikacja pakietu

Publikacja pakietu to kluczowy krok w procesie udostępnienia oprogramowania innym użytkownikom, zespołom lub całej społeczności. Udostępniając pakiet, umożliwiamy jego łatwą **instalację, aktualizację i wykorzystanie w innych projektach**. Proces publikacji zależy od repozytorium, na którym chcemy opublikować pakiet, a także od tego, czy nasz pakiet jest przeznaczony do użytku prywatnego, publicznego, czy specjalistycznego.

**Platformy do dystrybucji pakietów:**

### [PyPI](https://pypi.org/)
Jest domyślną i najczęściej używaną platformą. Pozwala na łatwą instalację pakietów za pomocą `pip` oraz ich aktualizację.

Instalacja dostępnego tam pakietu: `pip install <nazwa_pakietu>`.

### [conda-forge](https://conda-forge.org/)
Platforma do dystrybucji pakietów oparta na systemie Conda, który wspiera pakiety Python i nie tylko. Conda-Forge to społecznościowe repozytorium, które umożliwia tworzenie i publikowanie pakietów w szerokiej gamie języków programowania.

Instalacja dostępnego tam pakietu: `conda install -c conda-forge <nazwa_pakietu>`.

Wymaga Conda do zarządzania.

### [GitHub Packages](https://docs.github.com/en/packages/learn-github-packages/introduction-to-github-packages)
Platforma dystrybucji zintegrowana bezpośrednio z GitHub, co umożliwia publikację pakietów obok kodu źródłowego i integrację z GitHub Actions.

Instalacja dostępnego tam pakietu: w zależności od konfiguracji `pip install` lub `conda install`.

### Inne

- Anaconda Cloud
- Artifactory
- PyPI Pro
- Docker Hub