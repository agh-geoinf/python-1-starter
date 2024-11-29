## Wstęp

Funkcje pozwalają na organizowanie i strukturyzowanie kodu w logiczne bloki, które można wielokrotnie wywoływać. Dzięki funkcjom możemy **uprościć programy**, **zmniejszyć ilość powtarzającego się kodu**, a także sprawić, że nasze rozwiązania staną się bardziej **modularne** i **łatwiejsze do utrzymania**.

Zalety używania funkcji:

- Modularność: Dzielisz duży problem na mniejsze części, które są łatwiejsze do zarządzania.

- Ponowne wykorzystanie: Funkcję można wywoływać wielokrotnie w różnych miejscach programu.

- Łatwiejsze utrzymanie: Zmiana logiki w jednym miejscu (w funkcji) automatycznie wprowadza zmiany w całym programie.

- Czytelność: Funkcje pomagają tworzyć bardziej zrozumiały i uporządkowany kod.

```python
import random
def generuj_losowa(seed=None):
    # Ustawienie ziarna (seed) generatora liczb losowych
    if seed is not None:
        random.seed(seed)
    # Generowanie losowej liczby z zakresu od 0 do 100
    return random.randint(0, 100)

liczba = generuj_losowa(seed=42)
print(f"Wygenerowana liczba: {liczba}")
```

Trzy kluczowe elementy każdej funkcji:

1. Słowo kluczowe służace definiowaniu funkcji - `def`

2. Argumenty: definiowanie i podawane wewnątrz `()`

3. Zwracane wartości - słowo kluczowe `return`

### Zadania

1. Napisz funkcję `zmien_wartosc(arg)`, która przyjmuje jeden argument i próbuje zmodyfikować ten argument w różny sposób w zależności od tego, czy jest on niemutowalny (w tym przypadku integerem) czy mutowalny (w tym przypadku listą).

    - Jeśli jest listą, wykonaj `arg[0] = 'kalafior'`.

    - Jeśli jest integerem, wykonaj `arg = 65482652`.

Wypisz przykłady dla obu przypadków, wypisz wartości przed i po wykonaniu funkcji. Jak się zachowują te obiekty?

!!! tip
    Warto skorzystać z funkcji `isinstance()`.

??? - note "Teoria: mutowalne i niemutowalne obiekty w funkcjach"
    Kiedy zmienne są przekazywane do funkcji jako argumenty, Python nie tworzy ich kopii, lecz przekazuje referencję do oryginalnego obiektu. W związku z tym sposób, w jaki te obiekty zachowują się wewnątrz funkcji, zależy od ich typu – mutowalne lub niemutowalne.

    **Obiekty mutowalne** (np. listy, słowniki):

    Zmienne tego typu mogą być modyfikowane w miejscu. Jeśli zostaną przekazane jako argumenty do funkcji i ich zawartość zostanie zmieniona, zmiana ta wpłynie na oryginalny obiekt, który istnieje poza funkcją.

    **Obiekty niemutowalne** (np. liczby, napisy, krotki):

    Zmienne niemutowalne nie mogą być modyfikowane w miejscu. Każda próba modyfikacji powoduje utworzenie nowego obiektu. Z tego powodu, zmiany wprowadzone wewnątrz funkcji nie wpływają na oryginalny obiekt poza funkcją.

## Dopasowywanie argumentów

Funkcje mogą przyjmować argumenty na różne sposoby, co umożliwia elastyczne przekazywanie danych. Kluczowe elementy to: **argumenty pozycyjne**, **argumenty nazwane**, **wartości domyślne**, oraz specjalne operatory **`*args`** i **`**kwargs`**, które pozwalają na przekazywanie zmiennej liczby argumentów.

```python
# dodanie wartości domyślnych
def dodaj(a = 0, b = 0):
    return a + b

# argumenty pozycyjne przekazywane są w podanej kolejności
print(dodaj(3, 5))

# argumenty nazwane można mieszać
print(b=5, a=3)
```

**`*args` - zmienna liczba argumentów pozycyjnych**

Wszystkie dodatkowe argumenty są zbierane w krotkę, dzięki czemu możemy obsłużyć więcej argumentów, niż zdefiniowano w sygnaturze funkcji.

```python
def suma(*liczby):
    return sum(liczby)

print(suma(1, 2, 3))  # 6
print(suma(10, 20))
```

**`**kwargs` – zmienna liczba argumentów nazwanych**

Argumenty są zbierane w słownik, co umożliwia przekazanie większej liczby argumentów nazwanych, niż przewidziano w sygnaturze funkcji.

```python
def przedstaw_sie(**dane):
    for klucz, wartosc in dane.items():
        print(f"{klucz}: {wartosc}")

przedstaw_sie(imie="Jan", wiek=30, miasto="Kraków")
```

??? - danger "Mieszane użycie argumentów nie zawsze jest możliwe"
    Ważne jest, aby przestrzegać kolejności: najpierw argumenty pozycyjne, potem domyślne, następnie `*args`, a na końcu `**kwargs`.

    ```python
    def funkcja_mieszana(a, b=10, *args, **kwargs):
        print(f"a: {a}, b: {b}")
        print(f"Argumenty dodatkowe (args): {args}")
        print(f"Argumenty nazwane (kwargs): {kwargs}")

    funkcja_mieszana(1, 2, 3, 4, imie="Ania", wiek=25)
    ```

    Oraz kilka niepoprawnych wywołań:

    ```python
    funkcja_mieszana()
    funkcja_mieszana(1, 2, 3, 4, 5, a=6)
    funkcja_mieszana(1, 2, 3, imie="Jan")
    funkcja_mieszana(a=1, 20)
    ```

    Sama definicja również może być niepoprawna:

    ```python
    def funkcja_mieszana(a=10, b):
        print(f"a: {a}, b: {b}")
    ```

### Zadania

1. Napisz funkcję `zamowienie_produktu`, która przyjmuje jeden obowiązkowy argument pozycyjny `nazwa_produktu` i dwa obowiązkowe argumenty nazwane: `cena` i `ilosc`. Funkcja powinna zwracać text podsumowujący zamówienie, zawierające nazwę produktu, łączną cenę (cena * ilość) oraz ilość zamówionego produktu.

    - Stwórz pustą listę, do której wstawisz wartości zwracane przez funkcję dla 3 różnych produktów.

    - Przeiteruj po wypełnionej liście, wyświetl teksty.

    - Zmodyfikuj funkcję tak, żeby oprócz tekstu podsumowującego zwracała także wartość zamówienia.

    - Na koniec wyświetl sumaryczną wartość zamówień (sumę z każdego zamówionego produktu).

    - Dodaj wartość domyślną dla argumentu `ilosc` równą 1.

!!! warning "Ważna informacja"
    Wykorzystaj poniższy początek definicji i go nie modyfikuj. Wymusi to podawanie argumentów po gwiazde jedynie w formie nazwanej.

    ```python
    def zamowienie_produktu(nazwa_produktu, *, cena, ilosc):
    ```

1. Napisz funkcję `stworz_raport`, która przyjmuje dowolną liczbę argumentów pozycyjnych (`*args`) i nazwanych (`**kwargs`). Argumenty pozycyjne powinny reprezentować numery ID produktów, a argumenty nazwane - informacje o tych produktach (np. nazwa, cena). Funkcja powinna tworzyć i wyświetlać raport, w którym dla każdego ID produktu podane są szczegółowe informacje na jego temat.

Wywołanie funkcji powinno wyglądać następująco:

```python
stworz_raport(101, 102, nazwa_101="Kubek termiczny", cena_101="45.99 zł", nazwa_102="Długopis", cena_102="4.99 zł")
```

## Funkcje - praktyczne porady

1. Funkcje powinny być niezależne od otoczenia - argumenty jako input, return jako output.
2. Unikamy zmiennych globalnych.
3. Nie modyfikujemy argumentów mutowalnych.
4. Funkcja ma być mała i mieć jeden cel.
5. Nie zmieniamy wartości zmiennych z innych modułów.
