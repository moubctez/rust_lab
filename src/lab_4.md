# Referencje, własność obiektów i pożyczanie

## Stos i sterta

Stos i sterta są dwoma sposobami zarządzania pamięcią dostępną dla działającego programu.

Stos ma z góry określoną wielkość, a wartości na nim odkładane są w odwrotnej kolejności niż zdejmowane (ostatnia odłożona wartość będzie pierwszą do zdjęcia).

Sterta wymaga żądania od systemu operacyjnego przydzielenia obszaru pamięci o żądanej wielkości. Kiedy taki kawałek pamięci przestaje być potrzebny, należy go zwolnić. Adres takiego kawałka pamięci przeważnie odkładany jest na stosie.

Odkładanie na stosie jest szybsze od zapisywania na stercie ponieważ odpada krok żądania (alokacji) pamięci.

## Reguły własności

Rust zakłada, że:
* każda wartość posiada właściciela
* właściciel może być tylko jeden (w danej chwili)
* kiedy wartość wychodzi poza zakres, zostaje zrzucona (ang. _drop_)

Zakres przeważnie zaczyna się i kończy między nawiasami klamrowymi.

```rust
{
    let banner = "Palenie szkodzi";
    println!("{banner}");

    // Tutaj `banner` kończy żywot. 💀
}
```

## Typ `String`

`String` jest typem danych przechowującym ciąg znaków. Znaki przechowywane są w buforze na stercie. Jest też ściśle związany z podstawowym typem danych `str`.

Nowy pusty `String` może zostać utworzony za pomocą metody `new'.

```rust
let mut text = String::new();
text.push_str("Witaj, świecie!");
println!("{text}");
```

`String` składa się z:

- wskaźnika do bufora ze znakami
- wielkości bufora (w bajtach), czyli ile bufora jest zajętego przez znaki
- całkowitej pojemności bufora (również w bajtach), czyli ile znaków może on pomieścić

## Kopiowanie i przenoszenie

Weźmy na przykład taki kod:

```rust
let var1 = 1234;
let var2 = var1;

println!("{var1}");
```

Tutaj do zmiennej `var1` przypisana jest liczba całkowita, która jest typem danych przypisanym. Te typy są **kopiowane** w momencie przypisywania ich do kolejnych zmiennych – w tym przykładzie `var2`. Ponieważ `var2` zawiera **kopię** `var1`, zmienna `var1` nie kończy życia – można wyświetlić jej zawartość przez `println!`.

Inaczej sprawa wygląda w takim przypadku:

```rust,compile_fail
let text1 = String::from("studia");
let text2 = text1;

println!("{text1}");
```

Tu zawartość zmiennej `text1` zostaje **przeniesiona** do zmiennej `text2`, więc zmienna `text1` przestaje istnieć. Powyższy kod nie skompiluje się. Kompilator zgłosi błąd:

```text
error[E0382]: borrow of moved value: `text1`
 --> src/main.rs:5:16
  |
2 |     let text1 = String::from("studia");
  |         ----- move occurs because `text1` has type `String`, which does not implement the `Copy` trait
3 |     let text2 = text1;
  |                 ----- value moved here
4 |
5 |     println!("{text1}");
  |                ^^^^^ value borrowed here after move
```

Dlaczego tak się dzieje? W pierwszym przykładzie, liczba całkowita jest na tyle mało złożona, że można są skopiować z jednej zmiennej do drugiej. W drugim przykładzie rezerwowana jest pamięć na bufor znaków i kopiowanie nie jest takie proste, ponieważ trzeba pamiętać aby taki bufor zwolnić kiedy nie jest już potrzebny. Jeżeli nastąpiłoby kopiowanie, nie byłoby wiadomo która zmienna miałaby być odpowiedzialna za zwolenienie pamięci bufora.

W szczególności, jeżeli typ danych implementuje cechę (_trait_) `Copy`, to dane są **kopiowane**. Ale o tym będzie mowa później

## Klonowanie

W przypadku kiedy zachodzi potrzeba skopiowania zawartości zmiennej, na pomoc przychodzi **klonowanie**. W przypadku `String` klonowanie polega na zarezerwowaniu pamięci na nowy bufor i skopiowanie zawartości jedneo bufora do drugiego. Do klonowania służy metoda `clone` (z cechy
`Clone`). Poniższy kod zadziała:

```rust
let text1 = String::from("studia");
let text2 = text1.clone();

println!("{text1}");
```

## Kopiowanie i klonowanie w funkcjach

Reguły gry dla funkcji działają podobnie jak przy przypisywaniu wartości do zmiennych. Poniższy przykład to ilustruje:

```rust
// Ta funkcja przejmuje `String` na własność. Zmienna `s` przestaje istnieć po zakończeniu się
// funkcji.
fn eat_string(s: String) {
    println!("{s}");
}

// Ta funkcja otrzymuje kopię `i32`.
fn copied_number(n: i32) {
    println!("{n}");
}

fn main() {
    let var = 1234;
    let text = String::from("studia");
    eat_string(text);
    copied_number(var);
}
```

Również funkcje zwracające wartość przestrzegają reguł własności.

```rust
// Ta funkcja zwraca `String` jednocześnie oddając jego własność.
fn new_string() -> String {
    let mut s = String::new();
    s.push_str("studia");
    s
}
```
