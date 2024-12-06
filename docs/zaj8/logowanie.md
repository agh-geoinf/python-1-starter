Logowanie to kluczowy element każdego programu, który pozwala na rejestrowanie zdarzeń zachodzących podczas jego działania. Może być używane do monitorowania aplikacji, diagnostyki błędów oraz zbierania informacji potrzebnych do analizy zachowania programu.

Korzystanie z funkcji `print` do debugowania jest całkiem wygodne, jednak szybko może stać się niewystarczające w bardziej zaawansowanych projektach. Python dostarcza name pakiet `logging`, który pozwala modyfikować podstawową konfigurację logowania, ale także budować własne `loggery`.

W logowaniu mamy poziomy (`DEBUG`, `INFO`, `WARNING`, `ERROR`, `CRITICAL`), które pozwalają określić priorytet i character komunikatu. W zależności od potrzeb, możliwe jest także określanie z jaką szczegółowością zapisywane są komunikaty.

Dzięki logowaniu możemy przekazywać komunikaty nie tylko do konsoli, ale także zapisywać je bezpośrednio do pliku.

## Podstawowa konfiguracja

Korzystanie z biblioteki `logging` jest bardzo proste, poniżej prosty skrypt:

```python
import logging

# Modyfikacja podstawowej konfiguracji
logging.basicConfig(level=logging.DEBUG,
                    format='%(asctime)s - %(levelname)s - %(message)s')

# Zgłaszanie komunikatów na różnych poziomach
logging.debug("Szczegółowy komunikat debug.")
logging.info("Informacyjny komunikat.")
logging.warning("Ostrzeżenie!")
logging.error("Wystąpił błąd.")
logging.critical("Błąd krytyczny!")
```

Skierowanie komunikatów do pliku:

```python
import logging

logging.basicConfig(filename='app.log',
                    filemode='w',
                    level=logging.WARNING,
                    format='%(asctime)s - %(levelname)s - %(message)s')

logging.info("To nie zostanie zapisane, bo poziom jest zbyt niski.")
logging.error("To zostanie zapisane w pliku.")
```

## Tworzenie własnych `loggerów`

Tworzenie własnych loggerów pozwala na większą kontrolę nad procesem logowania w aplikacjach wielomodułowych. Dzięki temu można:

- Oddzielić logi różnych modułów – każdy moduł może mieć swojego loggera, co ułatwia debugowanie.
- Zarządzać logowaniem niezależnie w każdym module – np. można ustawić inne poziomy logowania dla różnych części aplikacji.
- Zapewnić czytelność i porządek – logi z każdego modułu są łatwe do odróżnienia dzięki nazwie loggera.

Dobrą praktyką jest przypisywanie `loggera` do każdego modułu, np. za pomocą:

```python
from .utils import get_logger

_logger = get_logger(__name__)
```

Tylko oczywiście konieczna jest implementacja takiej funkcji, np. w module `utils`:

```python
import logging
from pathlib import Path

def get_logger(
    name: str,
    log_level: int | str = logging.INFO,
    log_file: Path = Path("application.log")
) -> logging.Logger:
    """Builds a `Logger` instance with provided name and log levels for stream and file.

    Args:
        name: The name for the logger.
        log_level: The default log level for the logger.
        log_file: Path to the file where logs should be saved.

    Returns:
        The logger.

    """
    logger = logging.getLogger(name)
    logger.setLevel(log_level)

    # Avoid adding multiple handlers if the logger already has them
    if not logger.handlers:
        # Formatter for both handlers
        formatter = logging.Formatter(
            fmt="%(asctime)s - %(name)s - %(levelname)s - %(message)s"
        )

        # Stream handler
        stream_handler = logging.StreamHandler()
        stream_handler.setLevel(logging.INFO)  # Set level for stream
        stream_handler.setFormatter(formatter)
        logger.addHandler(stream_handler)

        # File handler
        file_handler = logging.FileHandler(log_file, mode="a")
        file_handler.setLevel(logging.ERROR)  # Set level for file
        file_handler.setFormatter(formatter)
        logger.addHandler(file_handler)

    return logger
```

### Zadania

1. W pliku `./src/zajecia08/utils/logging.py` stwórz funkcję `get_logger` do tworzenia własnych loggerów. Zwracany logger ma posiadać zarówno logowanie strumieniowe jak i do pliku.
2. Zaprezentuj działanie tej funkcji wykorzystujać `_logger = get_logger(...)` w co najmniej 2 modułach logując informacje na różnych poziomach.
