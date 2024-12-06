Command Line Interface (CLI) to tekstowy interfejs, który pozwala użytkownikowi komunikować się z programem za pomocą poleceń wpisywanych w terminalu. CLI znajduje zastosowanie w szerokim spektrum projektów – od automatyzacji zadań, przez analitykę danych, po integrację w środowiskach konteneryzacji, takich jak Docker. W Pythonie tworzenie CLI jest stosunkowo proste, istnieją gotowe pakiety do ich tworzenia.

**Dlaczego akurat tworzenie CLI?**

- **Prosty i lekki sposób interakcji z aplikacją**

    - Minimalizm - CLI nie wymaga tworzenia graficznego interfejsu użytkownika (GUI), co zmniejsza złożoność projektu i czas jego realizacji. Wiele aplikacji, szczególnie w środowiskach technicznych i analitycznych, nie wymaga GUI – użytkownicy oczekują prostoty i funkcjonalności.
    - Elastyczność - użytkownicy mogą dostosować sposób działania aplikacji za pomocą opcji i argumentów.
    - Zgodność z DevOps i automatyzacją - CLI jest naturalnym wyborem w środowiskach, gdzie aplikacje są uruchamiane w sposób zautomatyzowany, np. w skryptach, zadaniach cron, czy pipeline'ach CI/CD.

- **Przydatność w analizie danych**

    - CLI dla analityków i naukowców - w analizie danych często nie potrzebujemy GUI. Interfejs tekstowy pozwala uruchamiać analizy z różnymi parametrami, bez konieczności klikania w aplikacji. Dzięki temu można z łatwością integrować narzędzie z istniejącymi pipeline'ami przetwarzania danych.
    - Powtarzalność - CLI pozwala na zapisanie dokładnej komendy użytej do analizy. To ułatwia odtworzenie wyników.

- **Uniwersalność i przenośność**

    - Współdziałanie w różnych środowiskach - CLI działa w każdym systemie operacyjnym, który ma dostęp do Pythona. Dzięki temu aplikacje są przenośne i łatwe do wdrożenia.
    - Brak zależności od GUI - w środowiskach serwerowych, gdzie nie ma dostępu do graficznego interfejsu, CLI jest jedynym praktycznym sposobem interakcji z aplikacją.

## `argparse`

Wbudowana biblioteka do tworzenia CLI. Jest prosta i funkcjonalna, ale wymaga większego wysiłku w porównaniu do nowoczesnych frameworków.

```python
import argparse

def main():
    parser = argparse.ArgumentParser(description="Prosty program CLI.")
    parser.add_argument("name", help="Imię użytkownika")
    parser.add_argument("--verbose", action="store_true", help="Wyświetla dodatkowe informacje")

    args = parser.parse_args()

    if args.verbose:
        print(f"Witaj, {args.name}! To jest szczegółowy komunikat.")
    else:
        print(f"Witaj, {args.name}!")

if __name__ == "__main__":
    main()

# Uruchamianie
# python script.py Jan --verbose
```

## `click`

Framework, który wykorzystuje dekoratory i jest bardziej czytelny niż `argparse`. Idealny dla średnich i dużych projektów. Obsługuje wielopoziomowe CLI i dynamiczne wartości domyślne.

```python
import click

@click.group()
def cli():
    """Program obsługujący różne komendy."""
    pass

@cli.command()
@click.argument("a", type=int)
@click.argument("b", type=int)
def add(a, b):
    """Dodawanie dwóch liczb."""
    click.echo(f"Wynik: {a + b}")

@cli.command()
@click.argument("a", type=int)
@click.argument("b", type=int)
def multiply(a, b):
    """Mnożenie dwóch liczb."""
    click.echo(f"Wynik: {a * b}")

if __name__ == "__main__":
    cli()

# Uruchamianie
# python script.py add 2 3
# python script.py multiply 4 5
```

## `typer`

Nowoczesny framework oparty na `click`, ale wykorzystujący type hints Pythona. Jest idealny do tworzenia intuicyjnych i zaawansowanych CLI z minimalnym wysiłkiem. Automatyczna walidacja argumentów i generowanie dokumentacji na podstawie adnotacji typów.

```python
import typer

app = typer.Typer()

@app.command()
def add(a: int, b: int):
    """Dodawanie dwóch liczb."""
    print(f"Wynik: {a + b}")

@app.command()
def multiply(a: int, b: int):
    """Mnożenie dwóch liczb."""
    print(f"Wynik: {a * b}")

if __name__ == "__main__":
    app()

# Uruchamianie
# python script.py add 2 3
# python script.py multiply 4 5
```

## Dodanie CLI do pakietu lokalnego

Żeby to osiągnąć, trzeba:

1. Zaimplementować funkcję CLI w pakiecie.
2. Dodać odpowiednią konfigurację w pliku `pyproject.toml`, aby CLI było dostępne jako polecenie.

```python
# Plik src/zajecia08/cli_entrypoint.py
import typer
from ... import add, multiply

app = typer.Typer()

@app.command()
def add_numbers(a: int, b: int):
    """Dodaje dwie liczby."""
    result = add(a, b)
    print(f"Wynik: {result}")

@app.command()
def multiply_numbers(a: int, b: int):
    """Mnoży dwie liczby."""
    result = multiply(a, b)
    print(f"Wynik: {result}")

if __name__ == "__main__":
    app()
```

```python
# Plik pyproject.toml - to trzeba dodać
[project.scripts]
mycli = "src/zajecia08/cli_entrypoint:app"

# Uruchamianie
# mycli add-numbers 10 20
```

### Zadania

1. Stwórz własne CLI i dodaj go do pakietu, do CLI dodaj jakąś jedną przykładową funkcję.
