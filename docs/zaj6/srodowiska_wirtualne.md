Kiedy pracujemy nad wieloma projektami, często napotykamy **problem zarządzania różnymi wersjami bibliotek i ich zależnościami**. Jeśli zainstalujemy wszystkie biblioteki globalnie, może dojść do konfliktów wersji, które będą powodować błędy w działaniu poszczególnych projektów. Rozwiązaniem są środowiska wirtualne, które umożliwiają utrzymanie **czystego głównego Pythona i instalowanie specyficznych dla projektu zależności w osobnych, izolowanych środowiskach**.

Upraszczają także **współpracę między developerami** oraz p**roces wdrażania (deploymentu) projektu**. Dzięki środowiskom wirtualnym każdy członek zespołu może łatwo zainstalować dokładnie te same wersje bibliotek, co eliminuje problemy z kompatybilnością między różnymi konfiguracjami lokalnymi. Dodatkowo pliki konfiguracyjne zawierają wszystkie niezbędne zależności z ich wersjami, co umożliwia szybkie odtworzenie środowiska na nowych maszynach – zarówno u nowych członków zespołu, jak i na serwerach produkcyjnych. Dzięki temu wdrożenie projektu staje się szybsze i bardziej bezawaryjne, ponieważ na etapie produkcji odtworzone jest dokładnie to samo środowisko, na którym projekt był rozwijany i testowany.

??? - tip "Dlaczego warto używać środowisk wirtualnych?"
    - Izolacja zależności – każde środowisko działa niezależnie od innych, co pozwala na instalowanie specyficznych wersji bibliotek dla każdego projektu bez konfliktów między nimi.
    - Ochrona globalnego Python – eliminuje potrzebę instalowania bibliotek globalnie, co utrzymuje główną instalację Pythona w czystości i zmniejsza ryzyko konfliktów.
    - Łatwe zarządzanie zależnościami – środowiska umożliwiają tworzenie plików konfiguracyjnych, które zawierają wszystkie zależności, co ułatwia ich zarządzanie i aktualizację.
    - Uproszczony deployment – dzięki plikom konfiguracyjnym można szybko odtworzyć środowisko na serwerach produkcyjnych, co przyspiesza wdrażanie aplikacji i zmniejsza ryzyko problemów wynikających z różnic w konfiguracjach.
    - Łatwość współpracy między developerami – pliki z zależnościami zapewniają, że każdy członek zespołu pracuje w identycznym środowisku, co eliminuje problemy z kompatybilnością na różnych maszynach.
    - Większa kontrola nad wersjami bibliotek – środowiska pozwalają na precyzyjne określenie wersji bibliotek, co jest kluczowe w projektach, gdzie stabilność zależy od konkretnych wersji pakietów.
    - Bezpieczeństwo – ograniczając instalacje do środowiska wirtualnego, zmniejszamy ryzyko wpływu potencjalnie niebezpiecznych bibliotek na inne projekty czy na system operacyjny.
    - Możliwość równoczesnej pracy nad projektami z różnymi wersjami Pythona – można tworzyć środowiska z różnymi wersjami interpretera Python, co jest przydatne w projektach wymagających specyficznej wersji.
    - Łatwość testowania nowych wersji bibliotek – można bezpiecznie przetestować aktualizacje bibliotek w osobnym środowisku bez ryzyka wpływu na inne projekty.
    - Prostota czyszczenia zależności – w razie potrzeby całe środowisko można usunąć wraz ze wszystkimi bibliotekami bez wpływu na resztę systemu.
    - Przejrzystość – środowiska umożliwiają łatwe śledzenie, jakie biblioteki są używane w danym projekcie, co zwiększa przejrzystość i ułatwia utrzymanie kodu.

## Narzędzia

W Pythonie dostępnych jest wiele narzędzi do **zarządzania środowiskami wirtualnymi i zależnościami**. Wybór odpowiedniego narzędzia zależy od specyficznych potrzeb projektu oraz od tego, jak precyzyjnie chcemy kontrolować zależności.

### [venv](https://docs.python.org/3/library/venv.html)

Wbudowany moduł Pythona, który pozwala tworzyć proste środowiska wirtualne. Nie ma wbudowanego zarządzania zależnościami.

### [pipenv](https://pipenv.pypa.io/en/latest/)

Łączy funkcje `pip` (do zarządzania zależnościami) i `venv` (do tworzenia środowisk wirtualnych). Jest to bardziej kompleksowe rozwiązanie, które automatycznie tworzy środowisko wirtualne i zarządza zależnościami na podstawie plików `Pipfile` i `Pipfile.lock`.

### [poetry](https://python-poetry.org/docs/)

Zaawansowane narzędzie, umożliwia łatwe definiowanie zależności, tworzenie środowisk wirtualnych i automatyczne tworzenie paczek z `pyproject.toml`. Oprócz tego tworzy także plik blokujący `poetry.lock`.

### [miniconda](https://docs.anaconda.com/miniconda/)

Menedżer pakietów i środowisk wirtualnych, sprawdza się w skomplikowanych środowiskach, szczególnie w data science i big data, gdyż pozwala także na instalację niskopoziomowych bibliotek (np. w C/C++).

[conda-lock](https://conda.github.io/conda-lock/) dodatkowo generuje pliki blokujące, co zapewnia spójność wersji.

Alternatywnie do [conda](https://docs.conda.io/en/latest/) można także używać [mamba](https://mamba.readthedocs.io/en/latest/) (która jest kompatybilna, ale lepiej zoptymalizowana pod kątem wydajności).

### [uv](https://docs.astral.sh/uv/)

Skupia się na pracy z `pyproject.toml` i jest zaprojektowane dla bardziej zaawansowanych deweloperów. Umożliwia definiowanie zależności i instalację pakietów bez dodatkowego narzutu. Jest stosunkowo nowym narzędziem.

## Zadania

1. Stwórz środowisko wirtualne wykorzystując `conda` i `conda-lock` na Pythonie 3.12 zakładając, że wymagane pakiety to `pytest` w wersji `8.*`, `pandas` w wersji `2.2.*` oraz `requests` w wersji wyższej niż `2.25`.

???+ warning "Dostęp do conda, mamba i conda-lock"
    Żeby zainstalować `conda` i `mamba`, najlepiej zaintalować je zgodnie z tymi [instrukcjami](https://github.com/conda-forge/miniforge#windows).

    Następnie powinny być one już dostępne w terminalu jako polecenia `conda` i `mamba`. Kolejnym krokiem jest zainstalowane `conda-lock` w środowisku `base` (lub w innym środowisku) za pomocą komend: `conda activate` oraz `mamba install -c conda-forge conda-lock` (lub z początkiem `conda`).

???+ warning "Uwaga"
    Przed zabraniem się do tworzenia środowiska wirtualnego, polecam najpierw zapoznać się z krokami w [dokumentacji](https://conda.github.io/conda-lock/basic_usage/). Najpierw tworzy się definicję, a później z niej środowisko wirtualne. Przykładów bardziej rozbudowanych `env.yml` można poszukać w internecie.

1. W środowisku tym zainstaluj lokalny pakiet w wersji edytowalnej.

??? - note "Dodanie do naszego wirtualnego środowiska obsługi notebooków i Jupyter Lab"
    Dopiero w tak stworzonym środowisku powinniśmy dodawać możliwość pracy w notebookach i interfejsie Jupyter Lab. Można to zrobić poprzez dodanie następujących bibliotek:

    ```python
    name: my-virtualn-env
    platforms:
      - win-64
    channels:
      - conda-forge
      - defaults
    dependencies:
      - jupyter
      - jupyterlab
      - notebook
      - pip:
          - mkdocs
    ```

    Widać tutaj, że w conda-lock można używać nie tylko dystrubucji pakietów z `conda-forge` czy `anaconda`, ale także z `pip`.
