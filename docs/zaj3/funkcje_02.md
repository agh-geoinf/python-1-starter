## Atrybuty

Każdy obiekt w Pythonie może mieć swoje atrybuty. Służą one przechowywaniu dodatkowych informacji na ich temat lub umożliwiają dostęp do ich stanów wewnętrznych.

Do atrybutów odwołujemy się za pomocą notacji kropkowej, np. `obiekt.atrybut`. Funkcje, tak samo jak inne obiekty, mogą mieć swoje atrybuty.

```python
def sample_function():
    return "Hello, world!"

sample_function.description = "To jest przykładowa funkcja."  # Dodanie niestandardowego atrybutu
print(sample_function.description)
```

## Adnotacje

Jest to sposób na dodawanie informacji o typach danych używanych w kodzie. Choć Python jest językiem dynamicznie typowanym i nie wymaga jawnego określania typów, adnotacje dają programiście możliwość wskazania, jakie typy danych powinny być używane, co poprawia czytelność i ułatwia pracę w zespołach.

??? - note "Idea adnotacji"
    Adnotacje stanowią coś w rodzaju "podpowiedzi" dla innych programistów oraz narzędzi analizujących kod (np. linterów, IDE), które mogą je wykorzystać do ułatwienia debugowania, uzupełniania kodu, czy znajdowania potencjalnych błędów.

W poniższym przykładzie argument `name` ma być typu `str`, tak samo zwracana przez funkcję wartość.

```python
def greet(name: str) -> str:
    return f"Hello, {name}!"
```

Zmienne także mogą posiadać swoje adnotacje.

```python
age: int = 25
name: str = "Alice"
```

Zaawansowane typy importuje się z modułu `typing`.

```python
from typing import List, Optional, Dict

def get_user_info(user_id: int) -> Optional[Dict[str, str]]:
    if user_id == 1:
        return {"name": "Hubert", "email": "hubert@example.com"}
    return None  # Funkcja może zwrócić słownik lub None
```

## Funkcje anonimowe – lambda

Są to **krótkie, anonimowe funkcje**, które można definiować bez nadawania im nazwy. Służą one głównie do wykonywania prostych operacji, zwłaszcza wtedy, gdy funkcja jest potrzebna tylko w jednym, konkretnym miejscu. Zamiast stosować pełną definicję funkcji z def, używamy słowa kluczowego lambda, aby szybko stworzyć funkcję w jednej linii.

```python
# Tak wygląda standardowo zdefiniowana funkcja
def add(x, y):
    return x + y
print(add(3, 5))

# A tak lambda
add = lambda x, y: x + y
print(add(3, 5))
```

### Zadania

1. Masz daną listę słowników reprezentujących informacje o książkach w bibliotece. Każdy słownik zawiera klucze: `tytul`, `author` oraz `rok_wydania`. Twoim zadaniem jest napisanie kodu, który wykonuje następujące operacje przy użyciu funkcji lambda:

    - Sortowanie książek według roku wydania: Posortuj listę książek w kolejności rosnącej według roku ich wydania.

    - Filtracja książek wydanych po 2000 roku: Utwórz nową listę zawierającą tylko te książki, które zostały wydane po roku 2000.

    - Transformacja listy do listy tytułów: Przekształć oryginalną listę książek w listę zawierającą tylko tytuły książek.

Wykorzystaj funkcje `sorted()`, `filter()` oraz `map()` w połączeniu z funkcjami lambda do realizacji zadania.

## Generatory

Generatory to specjalne obiekty, które generują wyniki na żądanie – jeden po drugim – zamiast tworzyć i przechowywać całą serię wyników od razu. Dzięki temu generatory są wydajniejsze w pracy z dużymi zestawami danych, ponieważ zużywają mniej pamięci.

Generatory działają na zasadzie *lazy evaluation*, co oznacza, że nie obliczają wszystkich wyników od razu, lecz tylko wtedy, gdy są potrzebne.

??? - notes "Czym generatory różnią się od iteratorów?"
    Różnią się w sposobie ich tworzenia:

    - **Iteratory** mogą być tworzone z dowolnej kolekcji iterowalnej (np. listy, krotki, słownika) przez wywołanie `iter()`, albo przez definiowanie klasy z metodą `__iter__()` i `__next__()`.

    - **Generatory** są tworzone przy użyciu funkcji z yield lub jako wyrażenia generatorów. Są to specjalne, uproszczone iteratory, które automatycznie obsługują stan i logikę __next__().

    Różnią się w sposobie przechowywania stanu:

    - **Iteratory** muszą ręcznie przechowywać stan między kolejnymi wywołaniami `__next__()`, co wymaga więcej kodu i zarządzania.

    - **Generatory** automatycznie zapamiętują stan wewnętrzny przy każdym wywołaniu `yield`, dzięki czemu są prostsze do implementacji.

    Różnią się w sposobie przechodzenia przez dane:

    - **Iteratory** zazwyczaj są jednorazowe, ale jeśli iterator działa na strukturze danych, jak lista, można go ponownie utworzyć przez `iter()`.

    - **Generatory** są jednorazowego użytku – po przeiterowaniu przez wszystkie wartości kończą się, i nie można ich wznowić od początku.

    Przykład:

    ```python
    # Niestandardowy iterator, który iteruje od 1 do podanej liczby:
    class CountUpTo:
        def __init__(self, max_value):
            self.max_value = max_value
            self.current = 1

        def __iter__(self):
            return self

        def __next__(self):
            if self.current > self.max_value:
                raise StopIteration
            else:
                self.current += 1
                return self.current - 1

    counter = CountUpTo(3)
    for number in counter:
        print(number)
    ```

    ```python
    # Analogiczny generator z yield
    def count_up_to(max_value):
        current = 1
        while current <= max_value:
            yield current
            current += 1

    for number in count_up_to(3):
        print(number)
    ```

### Funkcje generatorów

Tworzymy je tak samo jak zwykłe funkcje, ale zamiast `return` używamy `yield`, który zwraca wartość, a następnie "zawiesza" działanie funkcji. Gdy po raz kolejny wywołujemy `next()` na generatorze, funkcja kontynuuje od miejsca, w którym ostatnio się zatrzymała.

```python
def count_up_to(n):
    count = 1
    while count <= n:
        yield count  # Zwraca wartość, ale kontynuuje działanie po wywołaniu next()
        count += 1

# Tworzymy generator
counter = count_up_to(5)

print(next(counter))  # 1
print(next(counter))  # 2
# Możemy też przeiterować przez cały generator w pętli
for number in counter:
    print(number)  #  3, 4, 5
```

### Wyrażenia generatorów

To bardziej zwięzły sposób na tworzenie generatorów, podobny do list składanych. Różnią się jednak nawiasami: zamiast nawiasów kwadratowych `[]` (jak w listach składanych) używamy nawiasów okrągłych `()`.

```python
gen = (x ** 2 for x in range(5))

# Kolejne wartości możemy uzyskać wywołując next()
print(next(gen))  # 0
print(next(gen))  # 1

# Lub iterując po generatorze w pętli
for value in gen:
    print(value)  # 4, 9, 16
```

### Zadania

1. Napisz generator, który iteracyjnie zwraca nazwy dni tygodnia: od poniedziałku do niedzieli. Następnie, użyj tego generatora w pętli, aby wyświetlić każdy dzień tygodnia. Dodatkowo, zademonstruj, jak można użyć tego generatora do pobrania tylko pierwszych trzech dni tygodnia bez konieczności iterowania przez cały tydzień.

???+ tip
    To zadanie można wykonać zarówno funkcją jak i wyrażeniem.
