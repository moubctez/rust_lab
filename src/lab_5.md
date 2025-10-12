# Struktury, enumeracje i dopasowywanie wzorców

Struktura łączy różne typy danych w jedną całość. Definiujemy ją za pomocą słowa kluczowego `struct`
oraz nazw pól i typów danych dla każdego pola. Na przykład definicja struktury może wyglądać tak:

```rust
struct User {
    id: u64,
    name: String,
    active: bool,
}
```

Mając definicję struktury, można utworzyć jej **instancję**, czyli strukturę z konkretnymi danymi.
Przy okazji, kolejność pól nie ma znaczenia. Do pól struktury można dobrać się używając kropki `.`.

```rust
# struct User {
#     id: u64,
#     name: String,
#     active: bool,
# }
#
let user1 = User {
    id: 1,
    name: String::from("Adam"),
    active: true,
};

let mut user2 = User {
    active: false,
    id: 2,
    name: String::from("Ewa"),
};

if user1.active {
    user2.active = true;
}
```

Zawartość jednej instancji można **przenieść** do drugiej instancji.

```rust
# struct User {
#     id: u64,
#     name: String,
#     active: bool,
# }
#
let user = User {
    id: 1,
    name: String::from("Adam"),
    active: true,
};

let duplicate_user = User {
    id: 2,
    name: user.name,
    active: user.active,
};
```

Jeśli chcemy zmienić tylko jakieś pola struktury, a resztę pozostawić bez zmian, można użyć notacji
**uaktualniania** – `..`.

```rust
# struct User {
#     id: u64,
#     name: String,
#     active: bool,
# }
#
let user = User {
    id: 1,
    name: String::from("Adam"),
    active: true,
};

let duplicate_user = User {
    id: 2,
    ..user
};
```

W powyższych przykładach zwróć uwagę, że zmienna `user` przestaje być użyteczna, ponieważ jej pole
`name` zostaje przeniesione, a `String` nie implementuje kopiowania. Gdyby tylko pola `id` oraz
`active` były przeniesione do `duplicate_user`, ich zawartość zostałaby skopiowana, a tym samym
zmienna `user` nadal byłaby do użytku. Najlepiej, drogi Czytelniku, sprawdź to sam.

Struktur można używać tak jak innych typów danych. Przykładowa funkcja tworząca i zwracająca
strukturę `User`:

```rust
# struct User {
#     id: u64,
#     name: String,
#     active: bool,
# }
#
fn new_user(id: u64, name: String) -> User {
    User {
        id: id,
        name: name,
        active: true,
    }
}
```

Jeżeli nazwy pól i zmiennych są takie same, można pominąć nazwę pola i napisać skrótowo:

```rust
# struct User {
#     id: u64,
#     name: String,
#     active: bool,
# }
#
fn new_user(id: u64, name: String) -> User {
    User {
        id,
        name,
        active: true,
    }
}
```

## Struktury krotkowe

Można również tworzyć strukty, które nie mają nazw pól, a jedynie typy danych. Do poszczególnych pól
można dobrać się używjąc kropki `.` i indeksu pola, albo rozbierając strukturę na poszczególne składowe.

```rust
struct Colour(i32, i32, i32);
struct Point(i32, i32, i32);

let black = Colour(0, 0, 0);
let origin = Point(0, 0, 0);

println!("x={}, y={}, z={}", origin.0, origin.1, origin.2);
let Colour(red, green, blue) = black;
println!("{red}, {green}, {blue}");
```

Należy zwrócić uwagę, że `Colour` i `Point` to dwie różne struktury, a co za tym idzie – dwa różne
typy danych. Mimo że ich zawartość składa się z takich samych typów.

## Struktury jednostkowe

Innym wariantem struktury jest struktura, która nie zawiera żadnych danych, czyli nie definiuje pól.
Takie struktury wykorzystywane są przy definiowaniu cech (o czym będzie mowa później).

```rust
struct NoNeedForAnyData;

let variant = NoNeedForAnyData;
```
