## Konwencja dot. nazewnictwa
- Nazwy rozpoczynające się od jednego `_` nie są importowane za pomocą `from moduł import *`.

Zmienne i funkcje zaczynające się od pojedynczego podkreślenia traktowane są jako "wewnętrzne", co oznacza, że powinny być stosowane tylko wewnątrz modułu i nigdzie poza nim.

```python
_internal_var = "To jest zmienna wewnętrzna"

def _internal_function():
    return "To jest funkcja wewnętrzna"

def public_function():
    return "To jest funkcja publiczna"
```

- Nazwy z dwoma `__` na początku i na końcu mają specjalne znaczenie dla interpretera.

Nazwy otoczone podwójnymi podkreśleniami, np. `__init__` lub `__name__`, są traktowane jako tzw. *magic methods*. Są one wykorzystywane przez interpreter do wykonywania specjalnych operacji.

```python
class ExampleClass:
    def __init__(self, value):
        self.value = value  # Funkcja __init__ jest wywoływana przy tworzeniu obiektu tej klasy

    def __str__(self):
        return f"ExampleClass with value: {self.value}"  # __str__ definiuje, jak obiekt będzie reprezentowany w formie tekstu

obj = ExampleClass(10)
print(obj)
```

- Zmienna będąca pojedynczym `_` przechowuje w sesji interaktywnej wynik ostatniego wyrażenia.
