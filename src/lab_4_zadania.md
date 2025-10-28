# Zadania do samodzielnego wykonania

1. W dokumentacji [standardowej biblioteki](https://doc.rust-lang.org/std/) Rust, sprawdź które
   podstawowe typy danych implementują cechę `Copy`, a które tej cechy nie implementują.

2. Sprawdź czy podstawowe typy danych implementują klonowanie.

3. W przypadku funkcji definiujących argument jako mutowalną zmienną, ale tej zmiennej w żaden
   sposób nie zmieniają, kompilator wypisuje ostrzeżenie. Sprawdź jakie.

4. Sprawdź jak zachowa się program, w którym został przekroczony zakres elementów zbioru, np.

   ```rust,should_panic
   let zeros = [0u8; 16];
   let slice = &zeros[..20];
   ```

5. Zaimplementuj trzy funkcje:

   ```rust,ignore
   fn eat_string(s: String)
   fn borrow_string(s: &String)
   fn borrow_mut_string(s: &mut String)
   ```

   Każda ma wypisać komunikat, ale w inny sposób:

   - `eat_string` — przejmuje własność i wypisuje tekst wielkimi literami.
   - `borrow_string` — pożycza tylko do odczytu, wypisuje tekst bez zmian.
   - `borrow_mut_string` — pożycza mutowalnie, dopisuje na końcu `HURRA!`, po czym wypisuje wynik. W
     `main():` utwórz `String`, wywołaj po kolei wszystkie trzy funkcje tak, by program się
      kompilował, wypisz końcowy stan `String`. Zastanów się nad kolejnością wywołań.

6. Napisz funkcję, która cenzoruje podany tekst w taki sposób, że:

   - jest stała lista słów do ocenzurowania (np. jako `const`)
   - zakazane słowa zastępowane są `***` w tekście
   - słowa zawierające zakazane słowa nie są cenzurowane, np. zakazane słowo _luj_ nie spowoduje, że
     funkcja ocenzuruje słowo _malujący_
   - ocenzurowany tekst zwracany jest jako `String`

   Pomoc:

   - przejrzyj dokumentację `str` oraz `std::string::String`
   - podany tekst rozdziel po białych znakach i przerób słowo po słowie
   - nie trzeba zachowywać pierwotnuych białych znaków – wystarczy przerobione słowa połączyć jedną
    spacją
