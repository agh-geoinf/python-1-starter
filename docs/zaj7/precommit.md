Jest kolejnym narzędziem, które ułatwia nam pracę poprzez agregację różnych funkcjonalności, a przy okazji blokuje nam możliwość przesyłania naszych zmian, jeśli nie spełniają one narzuconych wymagań.

W zależności od tego jak zostanie skonfigurowany, może name pomagać z:

1. Wyszukiwaniem błędów przed zatwierdzeniem kodu (`commit`) - np. zatwierdzanie kodu, który nie spełnia standardów stylu, importowanie nieużywanych bibliotek, niedokończone fragmenty kodu czy konflikty w merge'ach.
2. Powtarzalnymi procesami - np. manualnym sprawdzaniem stylu, uruchamianiem testów czy usuwaniem plików tymczasowych.
3. Zapewnieniem spójności w kodzie od różnych deweloperów - np. różni członkowie zespołu mogą korzystać z różnych standardów.
4. Wymaganiami projektowymi - wymusza stosowanie linterów, testów czy innych narzędzi.

Konfiguracja `pre-commit` odbywa się poprzez plik `.pre-commit-config.yaml` (zwykle w głównym katalogu repozytorium). Ten plik definiuje, jakie zadania (`hooks`) mają być uruchamiane przed commitowaniem.

### Zadania
1. Zainicjuj `pre-commit` wykorzystując komendę `make setup-pre-commit`
2. Uruchom `pre-commit hooks` za pomocą kolejnej komendy `make pc`
3. Ponownie popraw wszystkie wylistowane błędy.