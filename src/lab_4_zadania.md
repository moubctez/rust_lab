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
