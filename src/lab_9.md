# Elementy funkcyjne języka

### Domknięcia (closures)

Domknięcia to funkcje anonimowe, które mogą pobierać zmienne z otoczenia. Można je zapisywać
w zmiennych i przekazywać dalej. Kompilator sam wnioskuje ich typy.

```rust
fn main() {
    let factor = 5;
    let multiply = |x: i32| x * factor;
    let a = 7;
    let result = multiply(a);
    println!("Wynik: {result}");
}
```

### Funkcje wyższego rzędu

Funkcje wyższego rzędu przyjmują inne funkcje lub domknięcia jako argumenty lub zwracają funkcje.
Umożliwia to parametryzowanie zachowania.

```rust
fn apply_to_5<F>(f: F) -> i32
where
    F: Fn(i32) -> i32,
{
    f(5)
}

fn main() {
    let doubled = apply_to_5(|x| x * 2);
    let incremented = apply_to_5(|x| x + 1);

    println!("Podwojone: {doubled}");
    println!("Zwiększone: {incremented}");
}
```

### Predykaty

Predykat to funkcja lub domknięcie zwracające `bool`. Służy jako warunek w filtracji, walidacji i
sterowaniu przepływem. Nie jest osobnym typem językowym, tylko wzorcem użycia.

```rust
fn is_even(x: i32) -> bool {
    x % 2 == 0
}

fn main() {
    let value = 4;
    if is_even(value) {
        println!("{value} jest parzyste");
    } else {
        println!("{value} jest nieparzyste");
    }
}
```

### Adaptery iteratorów

Adaptery iteratorów tworzą nowe iteratory na podstawie istniejących, bez natychmiastowej realizacji
obliczeń. Pozwalają budować potoki przekształceń. Przykładem są `map`, `filter`, `take` itd.

```rust
fn main() {
    let numbers = [1, 2, 3, 4, 5];
    let doubled_iter = numbers.iter().map(|x| x * 2);
    let filtered_iter = doubled_iter.filter(|x| *x > 5);

    let collected: Vec<_> = filtered_iter.collect();
    println!("Wynik: {collected:?}");
}
```

### Konsumujące operatory iteratorów

Konsumujące operatory zużywają iterator i zwracają konkretny wynik. Przykłady to `sum`, `collect`,
`count`. Często kończą one łańcuch operacji funkcyjnych.

```rust
fn main() {
    let numbers = [1, 2, 3, 4, 5];
    let sum: i32 = numbers.iter().sum();
    let count = numbers.iter().count();
    let as_vec: Vec<_> = numbers.iter().map(|x| x * 2).collect();

    println!("Suma: {sum}");
    println!("Liczba elementów: {count}");
    println!("Podwojone: {as_vec:?}");
}
```

### Leniwa ewaluacja iteratorów

Iteratory w Rust są leniwe – obliczenia wykonują się dopiero przy konsumpcji. Dzięki temu można
definiować długie łańcuchy przekształceń bez kosztu pośredniego. Same adaptery nie robią nic dopóki
nie wywołamy operacji końcowej.

```rust
fn main() {
    let numbers = [1, 2, 3, 4, 5];

    let iter = numbers
        .iter()
        .map(|x| {
            println!("map: {x}");
            x * 2
        })
        .filter(|x| {
            println!("filter: {x}");
            *x > 5
        });

    println!("Przed collect");
    let collected: Vec<_> = iter.collect();
    println!("Po collect: {collected:?}");
}
```

---

### Kompozycja funkcji

Kompozycja polega na łączeniu funkcji w nowe funkcje. Umożliwia tworzenie złożonej logiki z
prostych kroków. W Rust robi się to ręcznie, przekazując jedną funkcję do drugiej.

```rust
fn add_one(x: i32) -> i32 {
    x + 1
}

fn double(x: i32) -> i32 {
    x * 2
}

fn compose<F, G>(f: F, g: G) -> impl Fn(i32) -> i32
where
    F: Fn(i32) -> i32,
    G: Fn(i32) -> i32,
{
    move |x| g(f(x))
}

fn main() {
    let add_then_double = compose(add_one, double);
    let result = add_then_double(5);
    println!("Wynik: {result}");
}

```
### Fabryki funkcji (funkcje generujące funkcje)

Fabryki funkcji zwracają inne funkcje lub domknięcia. Umożliwiają tworzenie parametrów zachowania
konfigurujących logikę. To popularna technika przy predykatach i filtrach.

```rust
fn greater_than(limit: i32) -> impl Fn(i32) -> bool {
    move |x| x > limit
}

fn main() {
    let gt_10 = greater_than(10);
    let gt_3 = greater_than(3);

    println!("12 > 10? {}", gt_10(12));
    println!("2 > 3? {}", gt_3(2));
}
```

### Modele `Fn`, `FnMut`, `FnOnce`

Te trzy cechy opisują sposób w jaki domknięcie korzysta z przechwyconych danych. `Fn` nie mutuje ani
nie konsumuje danych, `FnMut` może je mutować, a `FnOnce` może je zużyć. Dobierany jest najwęższy
możliwy model.

```rust
fn call_fn<F>(f: F)
where
    F: Fn(),
{
    f();
}

fn call_fn_mut<F>(mut f: F)
where
    F: FnMut(),
{
    f();
}

fn call_fn_once<F>(f: F)
where
    F: FnOnce(),
{
    f();
}

fn main() {
    let x = 10;
    let f = || println!("Fn, x = {x}");
    call_fn(f);

    let mut counter = 0;
    let mut g = || {
        counter += 1;
        println!("FnMut, counter = {counter}");
    };
    call_fn_mut(&mut g);

    let s = String::from("Hello");
    let h = || {
        println!("FnOnce, s = {s}");
        // s jest konsumowane logicznie, choć tu tylko wypisywane
    };
    call_fn_once(h);
}
```

### Domyślna niemutowalność

Zmienne w Rust są domyślnie niemutowalne. Wspiera to styl funkcyjny, ograniczając efekty uboczne.
Mutowalność trzeba jawnie oznaczyć.

```rust
fn main() {
    let x = 5;
    println!("x = {x}");

    let mut y = 10;
    println!("y przed: {y}");
    y += 1;
    println!("y po: {y}");
}
```

### Algebraiczne typy danych (`enum`)

Enumy pozwalają definiować typy z wieloma wariantami, każdy z własną strukturą danych. To podstawa
modelowania stanów i przypadków. Naturalnie współpracują z `match`.

```rust
enum Shape {
    Circle(f64),
    Rectangle { width: f64, height: f64 },
}

fn area(shape: Shape) -> f64 {
    match shape {
        Shape::Circle(r) => 3.14 * r * r,
        Shape::Rectangle { width, height } => width * height,
    }
}

fn main() {
    let c = Shape::Circle(2.0);
    let r = Shape::Rectangle { width: 3.0, height: 4.0 };

    println!("Pole koła: {}", area(c));
    println!("Pole prostokąta: {}", area(r));
}
```

### Typ `Option`

`Option` reprezentuje wartość, która może istnieć (`Some`) lub nie (`None`). Zmusza do jawnego
obchodzenia się z brakiem wartości. Eliminuje potrzebę stosowania typu _null_.

```rust
fn find_even(numbers: &[i32]) -> Option<i32> {
    for &n in numbers {
        if n % 2 == 0 {
            return Some(n);
        }
    }
    None
}

fn main() {
    let data = [1, 3, 5, 8];
    match find_even(&data) {
        Some(n) => println!("Znaleziono parzystą: {n}"),
        None => println!("W zbiorze brak liczby parzystej"),
    }
}
```

### Typ `Result`

`Result` reprezentuje wynik, który może być poprawny (`Ok`) lub błędny (`Err`). Wymusza jawne
zarządzanie błędami.

```rust
fn parse_number(s: &str) -> Result<i32, String> {
    s.parse::<i32>().map_err(|e| format!("Błąd parsowania: {e}"))
}

fn main() {
    let inputs = ["42", "abc"];

    for s in &inputs {
        match parse_number(s) {
            Ok(n) => println!("Tekst '{s}' -> liczba {n}"),
            Err(e) => println!("Tekst '{s}' -> {e}"),
        }
    }
}

```
### Metoda `map` (dla `Option`/`Result`)

`map` przekształca zawartość struktury, pozostawiając kontekst `Some`/`None` lub `Ok`/`Err`.
Umożliwia czyste przekształcenia bez zmiany logiki błędów. Minimalizuje potrzebę ręcznego `match`.

```rust
fn main() {
    let maybe_number: Option<i32> = Some(5);
    let maybe_doubled = maybe_number.map(|x| x * 2);
    println!("Option po map: {maybe_doubled:?}");

    let result_number: Result<i32, &str> = Ok(10);
    let result_string = result_number.map(|x| format!("Liczba: {x}"));
    println!("Result po map: {result_string:?}");
}
```

---

### Metoda `and_then` (monadyczna płaskość)

`and_then` umożliwia łańcuchowe operacje, które same zwracają `Option` lub `Result`. Zapobiega
zagnieżdżeniom `Option<Option<T>>` i `Result<Result<T,E>,E>`. Ułatwia budowę potoków obliczeń
zależnych.

```rust
fn parse_and_double(s: &str) -> Result<i32, String> {
    s.parse::<i32>()
        .map_err(|e| format!("Błąd parsowania: {e}"))
        .and_then(|n| {
            if n % 2 == 0 {
                Ok(n * 2)
            } else {
                Err("Liczba nie jest parzysta".to_string())
            }
        })
}

fn main() {
    let inputs = ["10", "3", "xyz"];

    for s in &inputs {
        println!("Wejście: '{s}'");
        println!("Wynik: {:?}", parse_and_double(s));
    }
}
```

### Niekopiowalne domknięcia i `move`

Słowo kluczowe `move` przenosi wartości z otoczenia do domknięcia. Jest to potrzebne np. przy
przekazywaniu domknięcia do wątku. Taka closura często będzie `FnOnce`, bo konsumuje swoje dane.

```rust,editable
use std::thread;

fn main() {
    let data = String::from("Ważne dane");

    let handle = thread::spawn(move || {
        println!("W wątku: {data}");
    });

    handle.join().unwrap();
    // println!("To błąd: {data}");
}
```

### Funkcyjny styl w iteracjach

Zamiast klasycznych pętli można używać iteratorów i adapterów. Daje to deklaratywny, zwięzły,
czytelny i często bezpieczniejszy kod. Mutacja stanu jest zredukowana do minimum.

```rust
fn main() {
    let numbers = vec![1, 2, 3, 4, 5, 6];

    let result: Vec<_> = numbers
        .into_iter()
        .filter(|x| x % 2 == 0)
        .map(|x| x * x)
        .collect();

    println!("Kwadraty parzystych: {result:?}");
}
```

### Predykaty – przykład dodatkowy

Przykład funkcji fabrykującej predykaty — czyli funkcji wyższego rzędu, która zwraca domknięcie
będące predykatem.

```rust
// Zwraca predykat: |x| x > k
fn greater_than(k: i32) -> impl Fn(i32) -> bool {
    move |x| x > k
}
```

* użyty jest `move`, aby przenieść wartość `k` do domknięcia,
* funkcja zwraca domknięcie implementujące `Fn(i32) -> bool`,
* zwrócone domknięcie jest predykatem, bo zwraca `bool`.

```rust
fn greater_than(k: i32) -> impl Fn(i32) -> bool {
    move |x| x > k
}

fn main() {
    let is_gt_10 = greater_than(10); // predykat: x > 10
    let is_gt_3  = greater_than(3);  // predykat: x > 3

    println!("{}", is_gt_10(15)); // true
    println!("{}", is_gt_10(8));  // false

    println!("{}", is_gt_3(4));   // true
    println!("{}", is_gt_3(2));   // false
}
```

## Rozszerzony przykład filtracji wektora

```rust
fn greater_than(k: i32) -> impl Fn(i32) -> bool {
    move |x| x > k
}

fn filter_vec<F>(v: Vec<i32>, predicate: F) -> Vec<i32>
where
    F: Fn(i32) -> bool,
{
    v.into_iter().filter(|x| predicate(*x)).collect()
}

fn main() {
    let data = vec![1, 2, 3, 4, 5, 6];

    let gt4 = greater_than(4);

    let result = filter_vec(data, gt4);
    println!("{result:?}");
}
```
