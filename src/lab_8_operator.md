# Operator ?

W funkcjach zwracająych `Result` można użyć operatora `?` aby natychmiast wrócić wartość `Err`.

Poniższy kod

```rust
use std::{
    fs::File,
    io::{self, Read},
};

fn read_from_file(filename: &str) -> Result<String, io::Error> {
    let mut file = match File::open(filename) {
        Ok(file) => file,
        Err(err) => return Err(err),
    };
    let mut string_buffer = String::new();
    match file.read_to_string(&mut string_buffer) {
        Ok(_) => Ok(string_buffer),
        Err(err) => Err(err),
    }
}
```

można skrócić do:

```rust
use std::{
    fs::File,
    io::{self, Read},
};

fn read_from_file(filename: &str) -> Result<String, io::Error> {
    let mut file = File::open(filename)?;
    let mut string_buffer = String::new();
    file.read_to_string(&mut string_buffer)?;

    Ok(string_buffer)
}
```

Należy zwrócić uwagę na to, aby zwracany typ `Err` (w powyżyszm przykładzie – `std::io::Error`)
zgadzał się z `Err` użytym z operatorem `?`. Kolejny przykład nie skompiluje się:

```rust,compile_fail
use std::{
    collections::TryReserveError,
    fs::File,
    io::{self, Read},
};

fn read_from_file_wrong(filename: &str) -> Result<String, TryReserveError> {
    let mut file = File::open(filename)?;
    let mut string_buffer = String::new();
    string_buffer.try_reserve(16)?;
    file.read_to_string(&mut string_buffer)?;

    Ok(string_buffer)
}
```

```text
error[E0277]: `?` couldn't convert the error to `TryReserveError`
  --> src/main.rs:12:40
   |
11 | fn read_from_file_wrong(filename: &str) -> Result<String, TryReserveError> {
   |                                            ------------------------------- expected `TryReserveError` because of this
12 |     let mut file = File::open(filename)?;
   |                    --------------------^ the trait `From<std::io::Error>` is not implemented for `TryReserveError`
   |                    |
   |                    this can't be annotated with `?` because it has type `Result<_, std::io::Error>`
   |
   = note: the question mark operation (`?`) implicitly performs a conversion on the error value using the `From` trait
   = help: the trait `From<std::io::Error>` is not implemented for `TryReserveError`
           but trait `From<TryReserveErrorKind>` is implemented for it
   = help: for that trait implementation, expected `TryReserveErrorKind`, found `std::io::Error`
```
