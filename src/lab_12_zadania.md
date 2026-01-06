# Zadania do samodzielnego wykonania

1. Napisz funkcje `rev` odwracajaca kolejnosc elementow w tablicy `&mut [u32]`. Następnie:   
  * Dodaj test jednostkowy `test_reverse` podobny do tego w opisie. 
  * Dodaj drugi test jednostkowy, ktory sprawdza, ze po wywolaniu `rev` dlugosc tablicy nie ulega zmianie.
  * Stworz test z makrem `assert_ne!`, ktory upewnia sie, ze pierwszy element po `rev` nie jest rowny 0 dla podanej tablicy.
  * Dodaj test, ktory uzywa `assert!` i `contains` do sprawdzenia, ze tablica po odwracaniu wciaz zawiera wskazana wartosc.
  * Zmien test tak, aby w przypadku bledu wyswietlal wlasny komunikat (dodatkowy argument makra `assert_eq!`).

2. Dodaj test oznaczony `#[ignore]` i sprawdz, ze jest pomijany przy zwyklym `cargo test`.

3. Napisz test oznaczony `#[should_panic]`, ktory panikuje przez `unwrap()` na `File::open`.

4. Wypisz liste wszystkich testow poleceniem `cargo test -- --list` i opisz, jak rozpoznac nazwe testu w module (zapisz to w komentarzu na końcu kodu razem z listą testów).

5. Dodaj komentarz dokumentacyjny `///` do funkcji i umiesc w nim przyklad z blokiem ` ```rust `.

6. Upewnij sie, ze przyklad z dokumentacji przechodzi jako test dokumentacji przy `cargo test`.

7. Praca nad własnym projektem: aktualne źródła projektu dołączyć (tylko pliki `*.rs` i`Cargo.toml`)
   do wysyłanych zadań.
