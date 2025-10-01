# Referencje, własność obiektów i wypożyczanie

## Stos i sterta

Stos i sterta są dwoma sposobami zarządzania pamięcią dostępną dla działającego programu.

Stos ma z góry określoną wielkość, a wartości na nim odkładane są w odwrotnej kolejności niż
zdejmowane (ostatnia odłożona wartość będzie pierwszą do zdjęcia).

Sterta wymaga żądania od systemu operacyjnego przydzielenia obszaru pamięci o żądanej wielkości.
Kiedy taki kawałek pamięci przestaje być potrzebny, należy go zwolnić. Adres takiego kawałka pamięci
przeważnie odkładany jest na stosie.

Odkładanie na stosie jest szybsze od zapisywania na stercie ponieważ odpada krok żądania (alokacji)
pamięci.

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

`String` jest typem danych przechowującym ciąg znaków. Znaki przechowywane są w buforze na stercie.
Jest też ściśle związany z podstawowym typem danych `str`.

Nowy pusty `String` może zostać utworzony za pomocą metody `new'.

```rust
let mut text = String::new();
text.push_str("Witaj, świecie!");
```
