## Moduły w Pythonie
Moduł to **pojedyńczy plik** zawierający kod źródłowy. Pozwalają one na lepsze strukturyzowanie projektów, dzięki czemu kod jest bardziej czytelny i łatwiejszy w zarządzaniu. Praktycznie, korzystając z modułów, można importować gotowe funkcje i klasy do innych plików, co zmniejsza ilość powtarzającego się kodu.

Różne elementy innych modułów można importować na wiele sposobów:
```python
import math  # standardowy import całego modułu
import math as m  # dodanie aliasu
from math import sqrt, pi  # importowanie wybranych elementów
from math import *  # importowanie wszystkich elementów
from math import sqrt as pierwiastek  # importowanie pojedyńczego elementu z aliasem
```
