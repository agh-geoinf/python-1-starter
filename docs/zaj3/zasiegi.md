Zasięg (ang. scope) określa, gdzie dana zmienna lub funkcja jest widoczna i dostępna. Zasady te pomagają w organizacji kodu oraz uniknięciu błędów wynikających z konfliktów nazw.

Główne zasady zarządzania zasięgiem opierają się na regule LEGB oraz specjalnych instrukcjach `global` i `nonlocal`.

## Przypisane nazwy są lokalne

Kiedy przypisujemy wartość zmiennej w funkcji, staje się ona lokalna dla tej funkcji. Oznacza to, że poza tą funkcją nie będzie widoczna.

```python
def funkcja():
    x = 10  # Zmienna lokalna, widoczna tylko w funkcji
    print(x)

funkcja()  # Wyświetli 10

# Natomiast taka operacja nie ma już sensu, zmienna x
# istnieje tylko wewnatrz funkcji
print(x)
```

## Reguła LEGB

Reguła ta określa w jakiej kolejności Python wyszukuje zmienne i funkcje, gdy napotka na nie w kodzie. Jest to ważne, ponieważ pozwala programiście zrozumieć, skąd Python pobiera wartość zmiennej, zwłaszcza gdy zmienne o tej samej nazwie występują w różnych zakresach (np. wewnątrz funkcji, w funkcjach zagnieżdżonych, w skrypcie globalnym).

LEGB to akronim od 4 typów zasięgów, przez które Python przechodzi w następującej kolejności:

1. **Local** - zmienne zadeklarowane w bieżącej funkcji lub wewnętrznej funkcji (najbardziej wewnętrzny zasięg).
2. **Enclosing** - zmienne zadeklarowane w zewnętrznej funkcji, jeśli funkcje są zagnieżdżone.
3. **Global** - zmienne deklarowane w skrypcie głównym, poza funkcjami, czyli na poziomie modułu.
4. **Built-in** - zmienne i funkcje wbudowane, dostępne globalnie (np. print, len).

```python
# W danym skrypcie / w danym module x jest
# zmienną globalną
x = "global"

def zewnetrzna():
    x = "enclosing"

    # Tak, funkcje mogą być zagnieżdżone
    def wewnetrzna():
        x = "local"
        print(x)  # Wydrukuje "local", bo najpierw przeszukuje lokalne zmienne
    wewnetrzna()
    print(x)  # Wydrukuje "enclosing"

zewnetrzna()
print(x)  # Wydrukuje "global"
```

## Instrukcje `global` i `nonlocal`

- `global` - pozwala zadeklarować zmienną globalną w funkcji, zmieniając jej wartość globalnie.
- `nonlocal` - umożliwia przypisanie wartości do zmiennej z otaczającego zakresu (nie globalnego), co przydaje się w funkcjach zagnieżdżonych.

Przykład użycia `global`:

```python
y = 5

def zmien_globalne():
    global y
    y = 10

zmien_globalne()
print(y)  # Wydrukuje 10
# Gdyby nie zastosowanie instrukcji global to y byłoby
# dalej równe 5, bo równe 10 byłoby jedynie lokalnie w funkcji
```

Przykład użycia `nonlocal`:

```python
def zewnetrzna():
    x = "original"

    def wewnetrzna():
        nonlocal x
        x = "modified"

    wewnetrzna()
    print(x)  # Wydrukuje "modified"
    # Pomimo tego, że x zostało zmodyfikowane jedynie
    # wewnątrz funkcji zagnieżdżonej

zewnetrzna()
```

## Funkcje zagnieżdżone, funkcje fabrykujące

Funkcje zagnieżdżone (ang. *nested functions*) to po prostu funkcje **zdefiniowanie wewnątrz innych funkcji**. Mogą być także wywoływane wewnątrz, ale nie tylko, bo mogą być również zwracane, używane jako argument czy być prywatnymi funkcjami pomocniczymi.

??? - "Przykłady dla funkcji zagnieżdżonych"
    Wywołanie funkcji wewnątrz:

    ```python
    def zewnetrzna(x):
        def wewnetrzna(y):
            return x + y
        # Funkcja wewnetrzna jest dostępna tylko w zewnetrzna
        return wewnetrzna(x)

    print(zewnetrzna(5))  # Wydrukuje 10
    print(wewnetrzna(3))  # Spowoduje błąd, bo wewnetrzna nie jest dostępna poza zewnetrzna
    ```

    Użycie funkcji zagnieżdżonej jako funkcji fabrykującej:

    ```python
    def funkcja_fabrykujaca(wiadomosc):
        def funkcja_zagniezdzona():
            return f"Twoja wiadomość to: {wiadomosc}"
        return funkcja_zagniezdzona

    # Tworzymy nową funkcję, która pamięta "wiadomosc"
    nowa_funkcja = funkcja_fabrykujaca("Witaj, świecie!")
    print(nowa_funkcja())  # Wydrukuje "Twoja wiadomość to: Witaj, świecie!"
    ```

    Przekazanie funkcji zagnieżdżonej jako argument:

    ```python
    def obliczenia(operator):
        def dodaj(x, y):
            return x + y

        def mnoz(x, y):
            return x * y

        # Zwracamy funkcję zagnieżdżoną w zależności od operatora
        if operator == "dodaj":
            return dodaj
        elif operator == "mnoz":
            return mnoz
        else:
            raise ValueError("Nieznany operator")

    # Teraz mamy funkcję dodaj i mnoz dostępną spoza obliczenia
    wybrana_funkcja = obliczenia("dodaj")
    print(wybrana_funkcja(2, 3))  # Wydrukuje 5

    wybrana_funkcja = obliczenia("mnoz")
    print(wybrana_funkcja(2, 3))  # Wydrukuje 6
    ```

    Tworzenie prywatnych funkcji pomocniczych:

    ```python
    def przetwarzaj_dane(lista):
        def filtruj(x):
            return x % 2 == 0

        def kwadrat(x):
            return x ** 2

        przefiltrowane = filter(filtruj, lista)
        return [kwadrat(x) for x in przefiltrowane]

    print(przetwarzaj_dane([1, 2, 3, 4, 5, 6]))  # Wydrukuje [4, 16, 36]
    ```

Funkcje fabrykujące (ang. *factory functions*) są więc jednym z typów funkcji zagnieżdżonych, gdzie **zwracana jest funkcja wewnętrzna**. Funkcje fabrykujące wykorzystują zamknięcia, co oznacza, że **funkcje, które zwracają, pamiętają kontekst, w którym zostały stworzone** (np. wartości zmiennych z zakresu otaczającego). To pozwala im zachować stan, co czyni je bardzo przydatnymi do tworzenia funkcji z predefiniowanymi ustawieniami lub konfiguracjami.

??? - "Przykłady dla funkcji zagnieżdżonych"
    Predefiniowanie parametrów:

    ```python
    def mnoznik(factor):
        def pomnoz(x):
            return x * factor
        return pomnoz  # Zwracamy nową funkcję

    # Tworzymy nową funkcję z zapamiętanym "factor"
    pomnoz_2 = mnoznik(2)
    pomnoz_3 = mnoznik(3)

    print(pomnoz_2(5))  # Wydrukuje 10 (5 * 2)
    print(pomnoz_3(5))  # Wydrukuje 15 (5 * 3)
    ```

    Liczniki:

    ```python
    def licznik():
        count = 0

        def zwieksz():
            nonlocal count
            count += 1
            return count

        return zwieksz

    licznik1 = licznik()
    licznik2 = licznik()

    print(licznik1())  # Wydrukuje 1
    print(licznik1())  # Wydrukuje 2
    print(licznik2())  # Wydrukuje 1 (nowy licznik)
    ```

    Konfiguracje w ramach wzorca fabrykującego:

    ```python
    def formatuj_text(style):
        def formatuj(wiadomosc):
            if style == "uppercase":
                return wiadomosc.upper()
            elif style == "lowercase":
                return wiadomosc.lower()
            elif style == "capitalize":
                return wiadomosc.capitalize()
            else:
                return wiadomosc
        return formatuj

    uppercase_formatter = formatuj_text("uppercase")
    lowercase_formatter = formatuj_text("lowercase")

    print(uppercase_formatter("hello world"))  # Wydrukuje "HELLO WORLD"
    print(lowercase_formatter("HELLO WORLD"))  # Wydrukuje "hello world"
    ```

    Walidatory:

    ```python
    def walidator_dlugosci(min_dlugosc, max_dlugosc):
        def waliduj(text):
            return min_dlugosc <= len(text) <= max_dlugosc
        return waliduj

    maly_tekst = walidator_dlugosci(1, 5)
    duzy_tekst = walidator_dlugosci(5, 10)

    print(maly_tekst("Hi"))       # Wydrukuje True
    print(maly_tekst("HelloWorld"))  # Wydrukuje False
    print(duzy_tekst("Python"))    # Wydrukuje True
    ```

    Funkcje złożonych obliczeń:

    ```python
    def procent_calculator(procent):
        def oblicz(calkowita_kwota):
            return calkowita_kwota * (procent / 100)
        return oblicz

    podatek_vat = procent_calculator(23)
    zysk_kapitalowy = procent_calculator(19)

    print(podatek_vat(1000))  # Wydrukuje 230.0
    print(zysk_kapitalowy(1000))  # Wydrukuje 190.0
    ```

    Złożone filtry i sortowania:

    ```python
    def filtruj_po_min_wartosci(min_wartosc):
        def filtruj(lista):
            return [x for x in lista if x >= min_wartosc]
        return filtruj

    filtruj_od_10 = filtruj_po_min_wartosci(10)

    print(filtruj_od_10([5, 10, 15, 20]))  # Wydrukuje [10, 15, 20]
    ```

### Zadania

1. Napisz funkcję fabrykującą `stworz_funkcje_potegujaca(wykladnik)`, która przyjmuje jeden argument: wykładnik potęgi. Zwracana przez nią funkcja zagnieżdżona `poteguj(podstawa)` powinna również przyjmować jeden argument – podstawę potęgi – i zwracać wynik podniesienia tej podstawy do potęgi określonej przez wykładnik przekazany do funkcji zewnętrznej.

!!! tip
    Wywołanie takiej funkcji i sprawdzenie powinno wyglądać następująco:

    ```python
    potega_2 = stworz_funkcje_potegujaca(2)`  # Tworzy funkcję potęgującą do kwadratu
    print(potega_2(4))  # Wynik: 16
    ```

## Zachowanie stanu

Jest istotnym elementem programowania, szczególnie w przypadku potrzeby utrzymania danych pomiędzy kolejnymi wywołaniami funkcji lub fragmentami kodu.

Jest to też dobry przypadek użycia dla zagadnień poznanych na dzisiejszych zajęciach.

Za przykład posłuży name accumulator, który zbierać będzie liczby dodawane w kolejnych wywołaniach funkcji. Po każdym dodaniu chcemy mieć możliwość pobrania aktualnej sumy. Poniżej rożne przykłady implementacyjne:

??? - "Użycie zmiennej `nonlocal`"
    ```python
    def akumulator_nonlocal():
        suma = 0

        def dodaj_wartosc(wartosc):
            nonlocal suma
            suma += wartosc
            return suma

        return dodaj_wartosc

    # Przykład użycia
    akum = akumulator_nonlocal()
    print(akum(5))  # Wydrukuje 5
    print(akum(10))  # Wydrukuje 15
    ```

??? - "Użycie zmiennej `global`"
    Zmienne `global` pozwalają na dostęp i modyfikację zmiennych globalnych wewnątrz funkcji. To podejście jest rzadziej stosowane, gdyż zmienne globalne są widoczne w całym module, co może prowadzić do błędów, ale w niektórych sytuacjach może się przydać.

    ```python
    suma_global = 0

    def dodaj_do_akumulatora(wartosc):
        global suma_global
        suma_global += wartosc
        return suma_global

    # Przykład użycia
    print(dodaj_do_akumulatora(5))   # Wydrukuje 5
    print(dodaj_do_akumulatora(10))  # Wydrukuje 15
    ```

??? - "Użycie klasy"
    Co prawda klasy będą dopiero na przyszłych zajęciach, ale są jednym z najczęstszych sposobów zarządzania stanem. Instancje klas pozwalają na łatwe tworzenie i przechowywanie danych w atrybutach.

    ```python
    class Accumulator:
        def __init__(self):
            self.suma = 0

        def dodaj(self, wartosc):
            self.suma += wartosc
            return self.suma

    # Przykład użycia
    akum = Accumulator()
    print(akum.dodaj(5))   # Wydrukuje 5
    print(akum.dodaj(10))  # Wydrukuje 15
    ```

??? - "Użycie atrybutu funkcji"
    ```python
    def akumulator_funkcyjny(wartosc):
        akumulator_funkcyjny.suma += wartosc
        return akumulator_funkcyjny.suma

    # Inicjalizacja atrybutu funkcji
    akumulator_funkcyjny.suma = 0

    # Przykład użycia
    print(akumulator_funkcyjny(5))   # Wydrukuje 5
    print(akumulator_funkcyjny(10))  # Wydrukuje 15
    ```

### Zadania

1. Napisz funkcję `licznik()`, która za każdym razem, gdy jest wywoływana, zwiększa swoje wewnętrzne liczenie o jeden (licznik stanu). Zaimplementuj cztery wersje tej funkcji, wykorzystując:

    - Zmienną `nonlocal` w zagnieżdżonej funkcji

    - Zmienną `global`.

    - Klasę z atrybutem instancji - wskazówka: zaimplementowanie w klasie funkcji `__init__` oraz `__call__`

    - Atrybut funkcji - funkcja, jak każdy inny obiekt, może mieć swoje atrybuty
