# Zadania do samodzielnego wykonania

1. Zdefiniuj proste domknięcia dodające 3 i 5 do argumentu.
2. Utwórz tablicę wskaźników do funkcji i wykonaj je sekwencyjnie na tej samej wartości.
3. Kalkulator oparty o tablicę wskaźników do funkcji
 Zbuduj program konsolowy, który działa jak prosty **kalkulator „strategii”**: użytkownik wybiera operację, a program wykonuje ją na podanej liczbie. Operacje mają być zrealizowane jako **wskaźniki do funkcji** (`fn(i64) -> i64`) przechowywane w **tablicy** (lub wektorze) i wywoływane przez indeks / nazwę.
	 a. Zaimplementuj co najmniej **6 operacji** jako zwykłe funkcje o sygnaturze:
	   ```rust
	   fn op(x: i64) -> i64
	   ```
	   Przykładowe operacje (możesz dobrać własne):

	   * `inc`: `x + 1`
	   * `dec`: `x - 1`
	   * `double`: `2 * x`
	   * `square`: `x * x`
	   * `neg`: `-x`
	   * `abs`: `x.abs()`
	 b. Zbuduj strukturę danych opisującą operacje jako pary:
	   * nazwa (np. `&'static str`)
	   * wskaźnik do funkcji `fn(i64) -> i64`
	   Wymaganie: przechowuj je w **tablicy**:

	   ```rust
	   const OPS: [(&str, fn(i64) -> i64); N] = [ ... ];
	   ```
	 c. Program ma działać w pętli:
	   * wypisz listę dostępnych operacji (z numerami i nazwami),
	   * wczytaj od użytkownika:
		 * wartość początkową `x` (i64),
		 * sekwencję operacji, które mają być wykonane po kolei.

	 d. Sekwencja operacji ma być wykonywana „łańcuchowo”:
	   * wynik poprzedniej operacji jest wejściem następnej,
	   * po każdej operacji wypisz krok, np.:
		 ```
		 start: 10
		 [double] 10 -> 20
		 [inc]    20 -> 21
		 [square] 21 -> 441
		 wynik: 441
		 ```
	 Użytkownik podaje np.:
	 * wartość: `10`
	 * lista: `double inc square`
	 Program wyszukuje nazwę w tablicy `OPS`.
	* Jeśli użytkownik poda niepoprawną liczbę → wypisz komunikat i poproś ponownie.
	* Jeśli poda nieistniejącą operację (zła nazwa) → wypisz błąd i:
	  * albo przerwij obliczenia,
	  * albo pomiń błędny krok (wybierz i opisz w komentarzu).
	* Nie używaj domknięć ani `Box<dyn Fn>` w tym zadaniu — wyłącznie `fn(...)`.
	* Oddziel logikę od I/O:
	  * funkcja `apply_sequence(x, seq, ops) -> i64`,
	  * osobno wczytywanie i drukowanie.
	## Dodatkowe rozszerzenia
	 a. Dodaj operacje zależne od parametrów jako osobne funkcje „wariantowe”, np. `add_10`, `mul_3` (nadal `fn(i64)->i64`).
	 
	 b. Dodaj komendę `help` wypisującą opis operacji.
	 
	 c. Dodaj tryb „single step” — użytkownik wybiera kolejną operację w pętli, aż wpisze `end`.
	 
	 d. oceń trudność zmiany na działanie na liczbach zmiennoprzecinkowych (opisz w komentarzu) 

4. Praca nad własnym projektem - aktualne źródła projektu dołączyć (tylko pliki `*.rs` i`Cargo.toml`) do wysyłanych zadań.