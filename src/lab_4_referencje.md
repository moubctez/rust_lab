# Referencje i pożyczanie

Nie ma konieczności oddawania zmiennych na zawsze. Można je pożyczyć. Pożyczone zmienne muszą zostać zwrócone. Analogicznie jak w życiu. W świecie języka Rust, pożyczanie polega na utworzeniu _referencji_ do zmiennej. Referencje uznaczamy znakiem `&`.

```rust
fn print(s: &String) {
    let len = s.len();
    println!("Długość {len} znaków: {s}");
}

fn main() {
    let text = String::from("studia");
    print(&text);
}
```

W powyższym przykładzie, składnia `&text` oznacza referencję do wartości `text`. Referencja **nie** staje się właścicielem, a wartość, do której się odnosi nie przestanie istnieć kiedy zniknie referencja. Tworzenie referencji nazywamy _pożyczaniem_.

Pożyczone wartości domyślnie nie są mutowalne, czyli nie można ich zmieniać.

```rust,compile_fail
fn modify(s: &String) {
    s.push_str(", świecie");
}

fn main() {
    let text = String::from("Witaj");
    modify(&text);

    println!("{text}");
}
```

Kompilacja zakończy się błędem:

```text
error[E0596]: cannot borrow `*s` as mutable, as it is behind a `&` reference
 --> src/main.rs:2:5
  |
2 |     s.push_str(", świecie");
  |     ^ `s` is a `&` reference, so the data it refers to cannot be borrowed as mutable
```

## Mutowalne referencje

Wcześniejszy kod można naprawić w taki oto sposób:

```rust
fn modify(s: &mut String) {
    s.push_str(", świecie");
}

fn main() {
    let mut text = String::from("Witaj");
    modify(&mut text);

    println!("{text}");
}
```

Została utworzona mutowalna referencja `&mut text`. Teraz widać wyraźnie, że funkcja zmieni zawartość tej zmiennej. Tak samo definicja funkcji jawnie wskazuje, że zawartość zmiennej podanej jako argument, ulegnie zmianie.

Mutowalne referencje mają ograniczenie: w danej chwili można mieć tylko **jedną** mutowalną referencję do zmiennej (i żadnej innej, nawet niemutowalnej).

```rust,compile_fail
let mut text = String::from("hello");

let ref1 = &mut text;
let ref2 = &mut text;

println!("{ref1}, {ref2}");
```

Kompilator od razy zakrzyczy błędem:

```text
error[E0499]: cannot borrow `text` as mutable more than once at a time
 --> src/main.rs:5:16
  |
4 |     let ref1 = &mut text;
  |                --------- first mutable borrow occurs here
5 |     let ref2 = &mut text;
  |                ^^^^^^^^^ second mutable borrow occurs here
6 |
7 |     println!("{ref1}, {ref2}");
  |                ---- first borrow later used here
```

Za to referencji niemutowalnych może być wiele. Poniższy kod jest poprawny.

```rust
let text = String::from("hello");

let ref1 = &text;
let ref2 = &text;

println!("{ref1}, {ref2}");
```

## Dyndające referencje

Kompilator języka Rust gwarantuje, że nie będą występować _dyndające_ referencje, to znaczy takie, które odnoszą się do nieistniejących danych.

```rust,compile_fail
fn dangle() -> &String {
    let s = String::from("hello");
    &s
}

fn main() {
    let reference_to_nothing = dangle();
}
```

Kompilator zwróci błąd. Na razie można pominąć komunikat o czasie życia (_lifetime_), bo o tym będzie później. Na razie istotne jest to, że funkcja zwraca pożyczoną wartość, ale nie ma skąd pożyczyć.

```text
error[E0106]: missing lifetime specifier
 --> src/main.rs:1:16
  |
1 | fn dangle() -> &String {
  |                ^ expected named lifetime parameter
  |
  = help: this function's return type contains a borrowed value, but there is no value for it to be borrowed from
```
