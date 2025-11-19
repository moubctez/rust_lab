# Metody

Funkcje ściśle powiązane ze strukturami danych nazywamy **metodami**. Metody są podobne do funkcji:
definiujemy je za pomocą słowa kluczowego `fn`. Różnica jest taka, że pierwszym parametrem jest
`&self` albo `&mut self` oraz to, że znajdują się w bloku `impl`.

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!("Obszar prostokąta {}", rect1.area());
}
```

Zwróć uwagę, że `area(&self)` pożycza referecję do struktury, do której się odnosi.

Nazwy metod mogą być takie same jak nazwy pól struktury. Na przykład, można zdefiniować funkcję
zwracającą szerokość prostokąta. Może to być użyteczne w przypadku, kiedy pole jest prywatne (nie
chcemy aby inne części kodu miały do niego dostęp).

```rust
# struct Rectangle {
#     width: u32,
#     height: u32,
# }
#
impl Rectangle {
    fn width(&self) -> u32 {
        self.width
    }
}
```

Jeżeli metoda ma zmieniać strukturę z którą jest powiązana, to sygnatura funkcji powinna zawierać
`&mut self` jako pierwszy parametr. Na przykład: prosta funkcja zmieniająca wysokość prostokąta.

```rust
# struct Rectangle {
#     width: u32,
#     height: u32,
# }
#
impl Rectangle {
    fn set_height(&mut self, height: u32) {
        self.height = height
    }
}
```

Rzadziej używane, aczkolwiek czasami użyteczne, jest podanie samego `self` jako pierszego parametru.
Taka metoda „zje” powiązaną strukturę, czyli przejmie na własność, a struktura przestanie istnieć
na końcu zakresu metody. Niezbyt wyszukany, ale prosty przykład:

```rust
# struct Rectangle {
#     width: u32,
#     height: u32,
# }
#
impl Rectangle {
    fn to_area(self) -> u32 {
        self.height * self.width
    }
}

fn main() {
    let rect = Rectangle {
        width: 10,
        height: 3,
    };
    let area = rect.to_area();
    // Tu `rect` kończy istnieć.
    println!("{area}");
}
```

## Powiązane funkcje

Funkcje zdefiniowane w bloku `impl`, ale nie odnoszące się do `self`, nazwyamy **funkcjami
powiązanymi**. Nie potrzebują one instancji aby zostać wywołane. Można użyć takiej funkcji jako
konstruktora dla powiązanego typu.

W przykładzie poniżej `Self` jest synonimem struktury, której dotyczy implementacja. W miejsce
`Self` można byłoby użyć konkretnej nazwy – w tym przypadku `Rectangle`.

Powiązane funkcje wywołuje się używając składni `::`.

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn new(width: u32, height: u32) -> Self {
        Self {
            width,
            height,
        }
    }

    fn square(size: u32) -> Self {
        Self {
            width: size,
            height: size,
        }
    }
}

fn main() {
    let rect = Rectangle::new(10, 3);
    let square = Rectangle::square(8);
}
```

> Można definiować wiele bloków `impl` dla tego samego typu danych.
