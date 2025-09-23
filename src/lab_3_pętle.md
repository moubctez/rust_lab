# Pętle

Pętle pozwalają na wielokrotne wykonywanie tego samego bloku kodu.

## Pętla `loop`

Pętla `loop` powtarza blok kodu dopóki nie zostanie przerwana przez `break` lub `return`.

Poniższy kawłek kodu będzie wykonywał się w nieskończoność, to jest dopóki działający program nie
zostanie przerwany (np. przez wciśnięcie Ctrl-C).

```rust,ignore
loop {
    println!("Jestem w pralce!");
}
```

Poniżej znajduje się przykładowy program zliczający ustawione bity w licznie 32-bitowej. Pętla
`loop` zostanie przerwana kiedy liczba będzie równa zero.

```rust
let mut liczba = 0xdead_f00du32;
let mut jedynki = 0;
loop {
    if liczba == 0 {
        break;
    }
    if liczba & 1 == 1 {
        jedynki += 1;
    }
    liczba >>= 1;
}
println!("{jedynki}");
```

Pętla jest również wyrażaniem, więc może zwracać wartość.

```rust
let mut liczba = 0xdead_f00du32;
let mut jedynki = 0;
let wynik = loop {
    if liczba == 0 {
        break jedynki;
    }
    jedynki += liczba & 1;
    liczba >>= 1;
};
println!("{wynik}");
```

Pętle mogą mieć etykiety, które są pomocne w określaniu, którą pętlę ma przerwać `break`.

```rust
'pętla_zewn: loop {
    loop {
        break 'pętla_zewn;
    }
}
```

## Pętla `while`

Pętla `while` powtarza blok kodu tak długi jak spełniony jest podany warunek. Warunek jest obliczany
przy każdym powtórzeniu pętli.

```rust
let mut liczba = 0xdead_f000u32;
let mut jedynki = 0;
while liczba != 0 {
    jedynki += liczba & 1;
    liczba >>= 1;
}
println!("{jedynki}");
```

## Pętla `for`

Pętla `for` służy do wykonywania bloku kodu dla każdego elementu w zbiorze. Zbiorem może być
tablica.

```rust
let a = [10, 20, 30, 40, 50];

for element in a {
    println!("wartość: {element}");
}
```

> Pętlę `for` nazwyamy _iterującą_. Wyrażenie do iterowania musi implementować
> `std::iter::IntoInterator`.

Innym elementem gerenrującym jest `Range` (ang. _zakres_) ze standardowej biblioteki Rust, który
generuje liczby z zakresu `początek..koniec`. Zakres będzie zawierał liczby `początek <= n <
koniec`. Poniższy kod wypisze liczby od 1 do 7.

```rust
for i in 1..8 {
    println!("{i}");
}
```
