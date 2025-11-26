# Zadania do samodzielnego wykonania

1. Napisz funkcję wyższego rzędu, która przyjmuje funkcję f: `fn(i32) -> i32` oraz liczbę x, a następnie zwraca `f(x)` powiększone o 1. Przygotuj kilka przykładowych wywołań

2. Napisz funkcję, która przyjmuje wektor `Vec<i32>` oraz predykat `Fn(i32) -> bool` (jako domknięcie i jako funkcję), i zwraca tylko te elementy, dla których predykat zwraca true. Użyj tej funkcji, aby wyfiltrować:
   * liczby parzyste z `vec![1, 2, 3, 4, 5, 6]`,
   * liczby większe od `3` z tego samego wektora.
   Wyświetl wynik obu filtracji.

3. Praca nad własnym projektem - aktualne źródła projektu dołączyć (tylko pliki `*.rs` i `Cargo.toml`) do wysyłanych zadań.