## Automatyzacja zadań w projektach programistycznych

`GNU Make` to narzędzie automatyzujące procesy budowania oprogramowania dla developerów. Zostało pierwotnie stworzone dla systemów Unix w latach 70, aby ułatwić kompilację kodu źródłowego w językach takich jak C. `Makefile` to plik konfiguracyjny, w którym definiuje się zasady, jak narzędzie `make` ma wykonywać zautomatyzowane zadania.

Działa na podstawie zależności między plikami. Dla każdego zadania (`target`) definiujemy:

1. Plik docelowy (target) - co chcemy osiągnąć (np. skompilowany program, przetestowana aplikacja).
2. Zależności (dependencies) - co jest wymagane, aby zrealizować zadanie.
3. Komendy (commands) - jak wykonać zadanie.

Generalnie korzysta się z niego prosto, a bardzo ułatwia życie.

Jego zalety:

1. **Automatyzowanie procesów** - pozwala zautomatyzować powtarzalne zadania, takie jak uruchamianie testów, kompilacja, instalacja zależności czy budowa projektu.
2. **Przenośność** - działa na różnych systemach operacyjnych (Unix, Linux, macOS, Windows z odpowiednimi narzędziami).
3. **Czytelność** - zamiast opisywać procesy w dokumentacji, wszystkie kroki są zdefiniowane w jednym pliku `Makefile`, co ułatwia nowym programistom zrozumienie projektu.
4. **Uniwersalność** - pomimo historycznego związku z `C` i `C++`, `Makefile` jest obecnie używany w wielu językach programowania, takich jak `Python`, `Go` czy `Rust`.

Żeby sprawdzić czy narzędzie jest dostępne, wystarczy uruchomić w konsoli `make --version`.
???- "Instalacja na **Linux**"

    Na większości dystrybucji GNU Make jest dostępny w standardowych repozytoriach.

    ```bash
    sudo apt update
    sudo apt install make
    ```

???- "Instalacja na **Windows**"
    
    Na Windows GNU Make nie jest instalowane domyślnie, ale można je zainstalować za pomocą różnych narzędzi.

    Tutaj przykład dla Chocolatey (które trzeba też najpierw [zainstalować](https://chocolatey.org/install)):

    ```cmd
    choco install make
    ```

???- "Instalacja na **MacOS**"
    
    W przypadku Homebrew GNU Make może być dostępny jako `gmake`, aby odróżnić go od wersji dostarczanej z systemem.

    ```
    brew install make
    ```

## Zadania

Nasz program, poza środowiskiem developerskim, nie potrzebuje w środowisku wirtualnym zainstalowanych bibliotek do lintingu czy testowania, dlatego zwykle stosuje się dwie osobne definicje środowiska (np. `env.yml` oraz `env-dev.yml`). 

1. Stwórz kopię pliku `env.yml`, zmień jego nazwę i zamień `dependencies` na 