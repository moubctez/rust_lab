# Typy generyczne, zaawansowane dziedziczenie i czas życia obiektów

# **TYPY GENERYCZNE**

## **1. Funkcja generyczna `identity`**

```rust
fn identity<T>(x: T) -> T {
    x
}

fn main() {
    let a = identity(10);
    let b = identity("tekst");
    let c = identity(3.14);

    println!("{a}, {b}, {c}");
}
```

## **2. Generyczna struktura `Stack<T>`**

```rust
struct Stack<T> {
    items: Vec<T>,
}

impl<T> Stack<T> {
    fn new() -> Self {
        Stack { items: Vec::new() }
    }

    fn push(&mut self, value: T) {
        self.items.push(value);
    }

    fn len(&self) -> usize {
        self.items.len()
    }
}

fn main() {
    let mut s1 = Stack::new();
    s1.push(10);
    s1.push(20);

    let mut s2 = Stack::new();
    s2.push("A");
    s2.push("B");

    println!("s1 len = {}", s1.len());
    println!("s2 len = {}", s2.len());
}
```

## **3. Generyczna funkcja z trait bound – `max`**

```rust
fn max<T: PartialOrd>(a: T, b: T) -> T {
    if a >= b { a } else { b }
}

fn main() {
    let m1 = max(3, 7);
    let m2 = max(1.5, 0.5);
    let m3 = max("abc", "abd");

    println!("{m1}, {m2}, {m3}");
}
```

## **4. Monomorfizacja – funkcja `square`**

```rust
fn square<T: std::ops::Mul<Output = T> + Copy>(x: T) -> T {
    x * x
}

fn main() {
    let a = square(3);
    let b = square(2.0);
    let c = square(1.5_f32);

    println!("{a}, {b}, {c}");
}
```

## **5. Const generics – `Buffer<T, N>`**

```rust
struct Buffer<T, const N: usize> {
    data: [T; N],
}

impl<T: Default + Copy, const N: usize> Buffer<T, N> {
    fn new() -> Self {
        Buffer { data: [T::default(); N] }
    }
}

fn main() {
    let b1: Buffer<i32, 4> = Buffer::new();
    let b2: Buffer<bool, 3> = Buffer::new();

    println!("Rozmiar b1: {}", b1.data.len());
    println!("Rozmiar b2: {}", b2.data.len());
}
```

# **ZAAWANSOWANE DZIEDZICZENIE (Cechy (TRAITY))**

## **6. Podstawowa cecha `Drawable`**

```rust
trait Drawable {
    fn draw(&self);
}

struct Button;
struct Label;

impl Drawable for Button {
    fn draw(&self) { println!("Rysuję przycisk"); }
}

impl Drawable for Label {
    fn draw(&self) { println!("Rysuję etykietę"); }
}

fn main() {
    let b = Button;
    let l = Label;

    b.draw();
    l.draw();

    draw_twice(&b);
    draw_twice(&l);
}

fn draw_twice(item: &impl Drawable) {
    item.draw();
    item.draw();
}
```

## **7. Polimorfizm statyczny i dynamiczny**

```rust
trait Drawable {
    fn draw(&self);
}

struct A; struct B;

impl Drawable for A { fn draw(&self) { println!("A"); } }
impl Drawable for B { fn draw(&self) { println!("B"); } }

fn draw_one(x: impl Drawable) {
    x.draw();
}

fn draw_all(xs: &[&dyn Drawable]) {
    for d in xs {
        d.draw();
    }
}

fn main() {
    let a = A;
    let b = B;

    draw_one(a);
    draw_one(b);

    let items: [&dyn Drawable; 2] = [&A, &B];
    draw_all(&items);
}
```

## **8. Cecha rozszerzająca inną cechę**

```rust
trait Printable {
    fn print(&self);
}

trait FancyPrintable: Printable {
    fn fancy_print(&self) {
        print!("*** ");
        self.print();
        println!(" ***");
    }
}

struct S(&'static str);

impl Printable for S {
    fn print(&self) {
        print!("{}", self.0);
    }
}

impl FancyPrintable for S {}

fn main() {
    let s = S("Hello");
    s.print();
    println!();
    s.fancy_print();
}
```

## **9. Konflikt metod – pełna ścieżka**

```rust
trait A { fn f(&self); }
trait B { fn f(&self); }

struct S;

impl A for S { fn f(&self) { println!("A::f"); } }
impl B for S { fn f(&self) { println!("B::f"); } }

fn main() {
    let s = S;

    A::f(&s);
    B::f(&s);

    // własny wybór
    (&s as &dyn A).f();
    (&s as &dyn B).f();
}
```

## **10. Supertrait – wymaganie `Read + Write`**

```rust
use std::io::{Read, Write};

trait ReadWrite: Read + Write {
    fn do_rw(&mut self) {
        println!("Wykonuję operacje Read + Write");
    }
}

impl<T: Read + Write> ReadWrite for T {}

fn main() {
    let mut cursor = std::io::Cursor::new(vec![1,2,3]);

    cursor.do_rw();
    cursor.write_all(&[9]).unwrap();
    cursor.set_position(0);

    let mut buf = [0; 4];
    cursor.read(&mut buf).unwrap();

    println!("{:?}", buf);
}
```

## **11. Trait „object safe”**

```rust
trait Animal {
    fn speak(&self);
}

struct Dog;
struct Cat;

impl Animal for Dog { fn speak(&self) { println!("Hau!"); } }
impl Animal for Cat { fn speak(&self) { println!("Miau!"); } }

fn make_speak(a: &dyn Animal) {
    a.speak();
}

fn main() {
    let d = Dog;
    let c = Cat;

    make_speak(&d);
    make_speak(&c);

    let animals: [&dyn Animal; 2] = [&d, &c];
    for a in animals {
        make_speak(a);
    }
}
```

# **CZASY ŻYCIA (LIFETIMES)**


## **12. Zależność lifetime – `choose_first`**

```rust
fn choose_first<'a>(x: &'a str, _y: &str) -> &'a str {
    x
}

fn main() {
    let a = "pierwszy";
    let b = "drugi";

    println!("{}", choose_first(a, b));
    println!("{}", choose_first("A", "B"));
}
```

## **13. Jawny lifetime w funkcji**

```rust
fn first<'a>(s: &'a str) -> &'a str {
    &s[0..1]
}

fn main() {
    println!("{}", first("test"));
    println!("{}", first("abc"));
}
```

## **14. Struktura z referencją – `RefHolder`**

```rust
struct RefHolder<'a, T> {
    value: &'a T,
}

impl<'a, T> RefHolder<'a, T> {
    fn get(&self) -> &T {
        self.value
    }
}

fn main() {
    let x = 10;
    let h = RefHolder { value: &x };

    println!("{}", h.get());
    println!("{}", *h.get());
}
```

## **15. Relacje `'a`, `'b` – funkcja `choose`**

```rust
fn choose<'a, 'b, T>(a: &'a T, b: &'b T) -> &T {
    // Zwrot według logiki (wybór typowy)
    if true { a } else { b }
}

fn main() {
    let x = 100;
    let y = 200;

    println!("{}", choose(&x, &y));
    println!("{}", choose(&y, &x));
}
```

## **16. Lifetimes + mutowalne pożyczanie**

```rust
fn push_and_get<'a>(v: &'a mut Vec<i32>, x: i32) -> &'a i32 {
    v.push(x);
    &v[v.len() - 1]
}

fn main() {
    let mut v = vec![1, 2, 3];

    let r1 = push_and_get(&mut v, 10);
    println!("{r1}");

    let r2 = push_and_get(&mut v, 20);
    println!("{r2}");
}
```

