# Zadania do samodzielnego wykonania

1. Napisz funkcje `rev` odwracajaca kolejnosc elementow w tablicy `&mut [u32]`. Następnie:   
  * Dodaj test jednostkowy `test_reverse` podobny do tego w opisie. 
  * Dodaj drugi test jednostkowy, który sprawdza, że po wywolaniu `rev` długość tablicy nie ulega
    zmianie.
  * Stwórz test z makrem `assert_ne!`, które upewnia się, że pierwszy element po `rev` nie jest
    równy 0 dla podanej tablicy.
  * Dodaj test, który używa `assert!` i `contains` do sprawdzenia, że tablica po odwracaniu wciąż
    zawiera wskazaną wartość.
  * Zmień test w taki sposób, aby w przypadku błędu wyświetlał własny komunikat (dodatkowy argument
    makra `assert_eq!`).

2. Dodaj test oznaczony `#[ignore]` i sprawdź, że jest pomijany przy zwykłym wywołaniu `cargo test`.

3. Napisz test oznaczony `#[should_panic]`, który panikuje przez `unwrap()` na `File::open`.

4. Wypisz listę wszystkich testów poleceniem `cargo test -- --list` i opisz, jak rozpoznać nazwę
   testu w module (zapisz to w komentarzu na końcu kodu razem z listą testów).

5. Dodaj komentarz dokumentacyjny `///` do funkcji i umieść w nim przyklad z blokiem kodu Rust
   (` ```rust `).

6. Upewnij się, że przykład z dokumentacji przechodzi jako test dokumentacji przy `cargo test`.

7. Praca nad własnym projektem: aktualne źródła projektu dołączyć (tylko pliki `*.rs` i`Cargo.toml`)
   do wysyłanych zadań.
