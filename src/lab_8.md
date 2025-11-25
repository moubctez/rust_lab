# Obsługa błędów

## panic

Makro `panic!` spowoduje nagłe przerwanie programu.

```rust,should_panic 
panic!("Krytyczny błąd programu!");
```

Dla zaawansowanych, panikę można przechwycić:

```rust,should_panic
std::panic::set_hook(Box::new(|panic_hook_info| {
    if let Some(payload) = panic_hook_info.payload_as_str() {
        println!("Program spanikował: {payload}");
    } else {
        println!("Program spanikował ale nic nie powiedział");
    }
}));

panic!("Krytyczny błąd programu");
```

## Result

Bibliotka standardowa Rust (std) definiuje `Result` jako `enum`:

```rust
pub enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

`Result` może przyjmować dwie wartości: `Ok` – dla pomyślnego wyniku, oraz `Err` – dla błędu; ten
typ jest używany do zwracania i propagacji błędów.

Na przykład, kiedy nie można zamienić ciągu znaków na liczbę całkowitą:

```rust
let text = "6e10";
match text.parse::<u32>() {
    Ok(number) => println!("Liczba: {number}"),
    Err(err) => println!("Nie można zamienić na liczbę: {err}"),
}
```

Powyższy program wypisze

```text
Nie można zamienić na liczbę: invalid digit found in string
```

Inny wynik da podobne rozwiązanie. Sprawdź jak zadziała taki program:

```rust
let text = "6e10";
match u32::from_str_radix(text, 16) {
    Ok(number) => println!("Liczba: {number}"),
    Err(err) => println!("Nie można zamienić na liczbę: {err}"),
}
```

### Metody

`Result` można zamienić na `Option` używając metody `ok()`. Tracimy wtedy informację o błędzie.

```rust
let text = "6e10";
match text.parse::<u32>().ok() {
    Some(number) => println!("Liczba: {number}"),
    None => println!("Nie można zamienić na liczbę"),
}
```

Metoda `unwrap_or` zwraca podaną domyślną wartość w przypadku gdy wystąpi `Err`:

```rust
let result = "6e10".parse::<u32>().unwrap_or(0);
```

Podobnie, metoda `unwrap_or_default` w przypadku wystąpianie `Err` zwróci domyślną wartość
zaimplementowaną jako cecha `Default`.

```rust
let result = "6e10".parse::<u32>().unwrap_or_default();
```

Kiedy chcemy zasiać panikę (`panic!`) kiedy wystąpi `Err`, można użyć metody `unwrap` lub `expect`.
Ta druga metoda wyświetli podany komunikat.

```rust,should_panic
let result = "6e10".parse::<u32>().unwrap();
```

```rust,should_panic
let result = "6e10".parse::<u32>().expect("Nie można zamienić na liczbę.");
```
