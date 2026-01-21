# Async/await
# Programowanie asynchroniczne

Zasada działania w skrócie: funkcje, które na coś czekają, mają możliwość zwolnienia zasobów dla
innych funkcji. Kiedy oczekiwanie zostanie zakończone, funkcja zostanie przywołana do dalszego
wykonuwania.

> Język Rust definiuje składnię programoania asynchronicznego, ale nie implementuje środowiska.
> Należy korzystać z zewnętrznych skrzynek, np. `tokio` lub `smol`, aby uruchonić takie środowisko.

## Tokio

Przykład:

```toml
[dependencies]
tokio = {version = "1", features = ["io-util", "macros", "net", "rt-multi-thread", "time"] }
```

```rust,ignore
# extern crate tokio;
use std::time::Duration;

const DELAY: Duration = Duration::from_millis(1);

async fn count(number: u32) {
    for i in 1..number {
        println!("Liczę {i}");
        tokio::time::sleep(DELAY).await;
    }
}

#[tokio::main]
async fn main() {
    count(10).await;
}
```

Makro `tokio::main` rozwija funkcję `main` mniej więcej do czegoś takiego:

```rust,ignore
# extern crate tokio;
# use std::time::Duration;
# use tokio::{runtime::Runtime, time::sleep};
#
# const DELAY: Duration = Duration::from_millis(1);
#
# async fn count(number: u32) {
#     for i in 1..number {
#         println!("Liczę {i}");
#         sleep(DELAY).await;
#     }
# }
#
fn main() {
    let rt = Runtime::new().expect("Failed building the Runtime");
    rt.block_on(async {
        count(10).await;
    })
}
```

### Połączenie z serwerem HTTP

```rust,ignore
use tokio::{
    io::{AsyncReadExt, AsyncWriteExt},
    net::TcpStream,
};

#[tokio::main]
async fn main() {
    let mut stream = TcpStream::connect("82.145.73.252:80").await.unwrap();
    let request = b"GET / HTTP/1.1\r\nHost: detox.wi.zut.edu.pl\r\nConnection: close\r\n\r\n";
    stream.write_all(request).await.unwrap();

    let mut response = String::new();
    stream.read_to_string(&mut response).await.unwrap();
    println!("{response}");
}
```

## Smol

Przykład:

```toml
[dependencies]
smol = "2"
```

```rust,ignore
use std::time::Duration;

const DELAY: Duration = Duration::from_millis(1);

async fn count(number: u32) {
    for i in 1..number {
        println!("Liczę {i}");
        smol::Timer::after(DELAY).await;
    }
}

fn main() {
    smol::block_on(async {
        count(10).await;
    })
}
```

> Funkcja `main` nie może być asynchronicza, ponieważ asynchroniczność wymaga środowiska, na którym
> musi być uruchamiana.

### Połączenie z serwerem HTTP

```rust,ignore
use smol::{
    block_on,
    io::{AsyncReadExt, AsyncWriteExt},
    net::TcpStream,
};

fn main() {
    block_on(async {
        let mut stream = TcpStream::connect("82.145.73.252:80").await.unwrap();
        let request = b"GET / HTTP/1.1\r\nHost: detox.wi.zut.edu.pl\r\nConnection: close\r\n\r\n";
        stream.write_all(request).await.unwrap();

        let mut response = String::new();
        stream.read_to_string(&mut response).await.unwrap();
        println!("{response}")
    });
}
```
# Powoływanie zadań

Asynchroniczne zadania powołujemy podobnie jak wątki. Zwracany uchwyt do zadania jest typu
`JoinHandle`.

```rust,ignore
# extern crate tokio;
use tokio::task::spawn;

#[tokio::main]
async fn main() {
    let handle = spawn(async { "Zadanie wykonane!" });

    let result = handle.await.unwrap();
    println!("{result}");
}
```

Wskazane jest aby zadania nie wykonywały długich obliczeń, bo w ten sposób zablokują środowisko
asynchroniczne i inne zadania nie będą się wykonywały. Takie zadania są **blokujące**. W skrócie,
funkcje `async` powinny często wywoływać `await`. Zadania blokujące można powołać do życia używając
`spawn_blocking` (nie są asynchroniczne).

```rust,ignore
# extern crate tokio;
use tokio::task::spawn_blocking;

#[tokio::main]
async fn main() {
    let handle = spawn_blocking(move || "Zadanie wykonane!");

    let result = handle.await.unwrap();
    println!("{result}");
}
```

Powoływanie wielu zadań używając `JoinSet`. Metoda `join_next` zwróci wynik pierszego zakończonego
zadania lub `None` kiedy wszystkie zadania zostaną zakończone.

```rust,ignore
use tokio::task::JoinSet;

#[tokio::main]
async fn main() {
    let mut set = JoinSet::new();

    for i in 0..10 {
        set.spawn(async move { format!("Liczę {i}") });
    }

    while let Some(result) = set.join_next().await {
        println!("{}", result.unwrap());
    }
}
```

Podobnie dla zadań blokujących.

```rust,ignore
use tokio::task::JoinSet;

#[tokio::main]
async fn main() {
    let mut set = JoinSet::new();

    for i in 0..10 {
        set.spawn_blocking(move || format!("Liczę {i}"));
    }

    while let Some(result) = set.join_next().await {
        println!("{}", result.unwrap());
    }
}
```
# Future

Funkcja asynchornicza jest odpowiednikiem do czegoś takiego:

```rust,ignore
fn count(number: u32) -> impl Future<Output = ()> {
    async move {
        for i in 1..number {
            println!("Liczę {i}");
            Timer::after(DELAY).await;
        }
    }
}
```

Cały blok `async move` jest wyrażaniem zwracanym przez funkcję. Wyrażanie implementuje cechę
`Future`, która posiada powiązany typ `Output`. `Output` definiuje typ danych zwracany przez
wyrażanie asynchroniczne (w powyższym przypadku zwracane jest _nic_, czyli `()`).
