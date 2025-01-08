## Zmienne środowiskowe - wstęp

Zmienna środowiskowa to para klucz-wartość przechowywana w systemie operacyjnym, która może być wykorzystywana przez programy w celu konfiguracji. Jako przykład można podać zmienną `PATH`, z której korzystaliśmy już wcześniej.

Wykorzystanie zmiennych środowiskowych (jako alternatywa do hardkodowania wartości w programie) ma szereg zalet i jest dobrą praktyką.

??? - note "Zalety korzystania ze zmiennych środowiskowych"
    - Bezpieczeństwo – unikanie przechowywania poufnych danych w kodzie – nie chcemy mieć tam naszych haseł czy poufnych danych;
    - Konfiguracja – możliwość zmiany parametru bez edytowania kodu – chociażby haseł, które powinny być zmieniane co jakiś czas czy przy przeniesieniu bazy danych na inny adres IP / URL;
    - Przenośność – łatwiejsze wdrażanie aplikacji na różnych środowiskach – tworzyć można wtedy bez problemu 3 środowiska, **deweloperskie**, gdzie pracują programiści i testują swoje zmiany, **testowe**, gdzie pracują testerzy i upewniają się, że wszystko działa poprawnie i nie ma bugów, **produkcyjne**, z którego korzystają użytkownicy;

**Dlaczego ten temat pojawia się przy bazach danych?**

A no dlatego, że nigdy nie chcemy danych dostępowych do bazy przechowywać w naszym kodzie i udostępniać ich do repozytorium.

### Sekrety w środowiskach wdrożeniowych

Można to robić na kilka sposobów w zależności od konfiguracji wdrożenia, tutaj przykłady:

**Bezpośrednie przekazywanie na serwerze lub w kontenerze**

Zmienne środowiskowe mogą być ustawione bezpośrednio na serwerze lub przekazywane do kontenera Docker. 

- Na serwerze, np. za pomocą komendy, w tym przypadku na Linuxa: 

`export DATABASE_URL="postgresql://user:password@prod-host:5432/prod-db"`

- Podczas uruchamiania kontenera Docker

`docker run -e DATABASE_URL="postgresql://user:passwrd@host:5432/db" my_app_image`

- Poprzez użycie pliku `.env` w `docker-compose`: 

Przykładowa treść pliku `docker-compose.yml`: 
```yaml
version: "3"
services:
  app:
    image: my_app_image
    env_file:
      - .env.prod
```

Tylko to wtedy my musimy zagwarantować dostępność tego pliku (szczegóły dot. pliku `.env` w praktycznej sekcji).

**Systemy do zarządzania sekretami**

Przykład: Azure Key Vault 

Azure Key Vault umożliwia przechowywanie poufnych informacji i zarządzanie nimi w bezpieczny sposób. Integruje się z usługami Azure oraz narzędziami CI/CD. 

Przykład pobierania sekretu w Pythonie (tylko musimy najpierw zagwarantować, że miejsce, gdzie uruchamiany jest Python ma dostęp do odpowiedniego zasobu w Azure):
```python
from azure.identity import DefaultAzureCredential 
from azure.keyvault.secrets import SecretClient 

vault_url = "https://myKeyVault.vault.azure.net/" 
credential = DefaultAzureCredential() 
client = SecretClient(vault_url=vault_url, credential=credential) 
secret = client.get_secret("DatabaseUrl") 

print(f"Database URL: {secret.value}") 
```

**Systemy CI/CD**

Przykład: Azure DevOps 

Podczas wdrożeń w Azure DevOps można korzystać z bibliotek zmiennych lub integrować potoki CI/CD z Azure Key Vault. 

Przykładowa treść potoku CI/CD w pliku `azure-pipelines.yml` (wykorzystująca Azure DevOps): 
```yaml
variables:
  - group: Prod-Secrets

steps:
  - task: UsePythonVersion@0
    inputs:
      versionSpec: "3.x"

  - script: |
      python -m venv venv
      source venv/bin/activate
      pip install -r requirements.txt
      python app.py
    env:
      DATABASE_URL: $(DatabaseUrl)
```

**Usługi zarządzania konfiguracją**

Przykład: Azure App Configuration 

Azure App Configuration pozwala na centralne zarządzanie konfiguracją aplikacji, w tym przechowywanie kluczy konfiguracyjnych i integrację z Key Vault. 

**Bezpieczne zmienne środowiskowe w Kubernetes** 

W przypadku aplikacji wdrażanych w Kubernetes można używać Secrets do przechowywania poufnych danych. 

## Zmienne środowiskowe – w praktyce

### Sekrety w lokalnych środowiskach deweloperów

???+ danger "Nigdy, ale to nigdy nie wprowadzamy naszych poufnych danych do repozytorium!"

Jako deweloperzy, możemy korzystać z pliku .env, który tworzymy w głównym katalogu repozytorium i ignorujemy go dla GITa (chociażby przez dodanie odpowiedniego wpisu w .gitignore). Następnie taki plik można odtwarzać lub przekazywać go sobie inną drogą. 

Przykładowa treść pliku `.env`: 

```commandline
DATABASE_URL=postgresql://user:password@localhost:5432/mydatabase 
SECRET_KEY=my_super_secret_key 
DEBUG=True 
```

**Przykładowy kod Python do odczytu zmiennych środowiskowych**

???- tip "Treść pliku `settings.py`"

    ```python
    # Plik settings.py
    import os
    from pathlib import Path
    from dotenv import load_dotenv
    
    class AppConfig:
        """Klasa reprezentująca konfigurację aplikacji."""
    
        def __init__(self):
            # Załadowanie zmiennych środowiskowych
            self._load_env()
            
            # Inicjalizacja zmiennych konfiguracyjnych
            self.database_url = self._get_env_variable("DATABASE_URL", required=True)
            self.secret_key = self._get_env_variable("SECRET_KEY", "default_secret_key")
            self.debug = self._get_env_variable("DEBUG", "False").lower() == "true"
    
        def _load_env(self):
            """Ładuje plik .env z głównego katalogu."""
            base_dir = Path(__file__).resolve().parent
            env_path = base_dir / ".env"
            load_dotenv(env_path)
    
        def _get_env_variable(self, var_name, default=None, required=False):
            """Zwraca wartość zmiennej środowiskowej lub podaje wartość domyślną."""
            value = os.getenv(var_name, default)
            if required and value is None:
                raise ValueError(f"Environment variable '{var_name}' is required but not set.")
            return value
    
    ```
???- tip "Przykład użycia modułu, treść pliku `main.py`" 

    ```python
    from settings import AppConfig 
    
    def main(): 
        # Tworzymy obiekt konfiguracji 
        config = AppConfig() 
    
    if __name__ == "__main__": 
        main() 
    ```

## Łączenie z bazą danych i ORM-y - wstęp

ORM (ang. Object-Relational Mapping) to technika pozwalająca na mapowanie obiektów w językach programowania na rekordy w bazach danych relacyjnych. W Pythonie najpopularniejszym narzędziem ORM jest biblioteka `SQLAlchemy`, choć popularne są również `Django ORM` i `Peewee`. 

Dzięki ORM zamiast pisać ręcznie zapytania SQL, możemy posługiwać się obiektami klas Pythonowych. To upraszcza pracę z bazami danych, umożliwiając operowanie na danych z wykorzystaniem koncepcji znanych z programowania obiektowego.

???- "Zalety korzystania z ORM-ów"

    - Ułatwienie pracy z bazą danych – zamiast pisać skomplikowane zapytania SQL, możemy używać metod Pythonowych;
    - Bezpieczeństwo – ORM-y chronią przed atakami SQL Injection, gdyż automatycznie budują zapytania i "bezpiecznie" przekazują parametry;
    - Łatwiejsza zmiana bazy danych – ORM pozwala na łatwą zmianę silnika bazy danych (np. z SQLite na PostgreSQL) bez dużych zmian w kodzie;
    - Zwiększenie czytelności kodu – można działać na obiektach Pythonowych, a nie na wynikach zapytań SQL;
    - Praca z modelami – ORM umożliwia definiowanie "modeli" reprezentujących tabele i kolumny jako klasy i atrybuty, co pozwala pisać kod w sposób bardziej obiektowy;

ORM-y są wygodne, ale mają pewne ograniczenia, które sprawiają, że w niektórych przypadkach lepiej korzystać z "surowych" zapytań SQL (ang. raw SQL). 

???- "Wady korzystania z ORM-ów"

    - Wydajność – ORM-y generują bardziej ogólne zapytania SQL, które nie zawsze są zoptymalizowane, a do tego mogą wykonywać wiele zapytań, co dodatkowo może spowalniać aplikację;
    - Brak wsparcia dla zaawansowanych zapytań – niektóre operacje (np. WITH, WINDOW FUNCTIONS, HAVING, GROUP BY) mogą być trudne lub niewygodne do wyrażenia za pomocą ORM-a;
    - Ukrywanie szczegółów SQL – ORM-y mogą ukrywać to, jakie zapytania SQL są wykonywane, co może utrudnić debugowanie, przy bardziej zaawansowanych problemach bazy danych czasem potrzebujemy ręcznego śledzenia zapytań;
    - Zwiększony narzut pamięciowy – ORM-y wymagają dodatkowej pamięci do mapowania rekordów na obiekty i zarządzania sesjami; 

Pythonowy przykład z raw SQL:

```python
from sqlalchemy.orm import sessionmaker 
from sqlalchemy import create_engine, text 
from settings import AppConfig 

# Konfiguracja połączenia z bazą danych 
config = AppConfig() 
engine = create_engine(config.database_url) 
Session = sessionmaker(bind=engine) 

def main():  
    session = Session() 

    # Zapytanie SQL: Pobierz użytkowników o wieku powyżej 30 lat 
    raw_sql = text("SELECT id, name, age FROM users WHERE age > :age") 
    result = session.execute(raw_sql, {"age": 30}) 

    # Parsowanie wyników 
    print("\nUsers older than 30 years:") 
    for row in result: 
        print(f"ID: {row.id}, Name: {row.name}, Age: {row.age}") 

if __name__ == "__main__": 
    main() 
```

## Połączenie z bazą danych i ORM-y – w praktyce

Przykład składa się z 3 plików, w tym 2 pierwsze są modułami pakietu, a ostatni to przykład użycia.

???- tip "Treść pliku `models.py`"
    ```python
    from sqlalchemy import Column, Integer, String
    from sqlalchemy.ext.declarative import declarative_base
    
    Base = declarative_base()
    
    class User(Base):
        __tablename__ = 'users'
        id = Column(Integer, primary_key=True)
        name = Column(String, nullable=False)
        age = Column(Integer, nullable=False)
    ```

???- tip "Treść pliku `db_manager.py`"
    ```python
    from sqlalchemy import create_engine, text
    from sqlalchemy.orm import sessionmaker
    from models import User
    from settings import AppConfig
    
    class DatabaseManager:
        """Klasa zarządzająca operacjami na bazie danych."""
    
        def __init__(self):
            # Załadowanie konfiguracji
            config = AppConfig()
            self.engine = create_engine(config.database_url, echo=config.debug)
            self.Session = sessionmaker(bind=self.engine)
    
        def add_user(self, name: str, age: int):
            """Dodaje nowego użytkownika do tabeli."""
            with self.Session() as session:
                new_user = User(name=name, age=age)
                session.add(new_user)
                session.commit()
                print(f"Added new user: {new_user.name}, age: {new_user.age}")
    
        def get_users_older_than(self, age: int):
            """Pobiera użytkowników starszych niż podany wiek."""
            with self.Session() as session:
                raw_sql = text("SELECT id, name, age FROM users WHERE age > :age")
                result = session.execute(raw_sql, {"age": age})
    
                users = [dict(row) for row in result]
                return users
    ```

???- tip "Treść pliku `main.py`"
    ```python
    from db_manager import DatabaseManager
    
    def main():
        db_manager = DatabaseManager()
    
        # Dodanie nowego użytkownika
        db_manager.add_user(name="Charlie", age=35)
    
        # Pobranie użytkowników starszych niż 30 lat
        users = db_manager.get_users_older_than(age=30)
    
        print("\nUsers older than 30 years:")
        for user in users:
            print(f"ID: {user['id']}, Name: {user['name']}, Age: {user['age']}")
    
    if __name__ == "__main__":
        main()
    ```

## Zadania

1. Skonfiguruj zmienne środowiskowe i moduł `settings.py`, które pozwolą Ci na połączenie się z bazą danych (najlepiej tą stworzoną w Azure). W zmiennych środowiskowych powinny być wszystkie sekrety, czyli chociażby URL, użytkownik, hasło i nazwa bazy danych.
2. Wykorzystaj kod SQL, który tworzy jakąś tabelę i wypełnia ją danymi. Może to być jakaś tabela z osobami, które tworzyliśmy na wcześniejszych zajęciach.
3. Napisz kod (odpowiednie moduły, zgodnie z przykładem wyżej), który pozwoli Ci w pliku `main.py` odczytać te osoby z tabeli, dodać kolejną osobę, a także usunąć konkretną osobę. 

Wynik tych zadań to pliki Pythonowe, proszę je dodać w zadaniu w Teams. Interesują mnie same skrypty, nie ma konieczności tworzenia pakietu czy definicji środowiska wirtualnego.