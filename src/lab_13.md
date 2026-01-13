# Współbieżność (wątki, synchronizacja)

# Wątki

W ramach działającego programu, czyli _procesu_, mogą być uruchamiane niezależne części działające
współbieżnie, zwane _wątkami_.

## Uruchamianie

Uruchomienie osobnego wątku odbywa się za pomocą `std::thread::spawn`.

```rust
use std::{thread, time::Duration};

const DELAY: Duration = Duration::from_millis(1);

fn main() {
    thread::spawn(|| {
        for i in 1..10 {
            println!("Wątek liczy {i}");
            thread::sleep(DELAY);
        }
    });

    for i in 1..5 {
        println!("Główny wątek liczy {i}");
        thread::sleep(DELAY);
    }
}
```

Funkcja `spawn` zwraca uchwyt (typu `std::thread::JoinHandle`), którego można użyć do czekania na
uruchomiony wątek.

```rust
use std::{thread, time::Duration};

const DELAY: Duration = Duration::from_millis(1);

fn main() {
    let handle = thread::spawn(|| {
        for i in 1..10 {
            println!("Wątek liczy {i}");
            thread::sleep(DELAY);
        }
    });

    for i in 1..5 {
        println!("Główny wątek liczy {i}");
        thread::sleep(DELAY);
    }

    handle.join().expect("Błąd złączenia z wątkiem");
}
```

> Co się stanie jeśli przenieść `join()` przed `for`?

## Przenoszenie zmiennych

Poniższy kod nie zostanie skompilowany.

```rust,compile_fail
use std::{thread, time::Duration};

const DELAY: Duration = Duration::from_millis(1);

fn main() {
    let fib = [1, 2, 3, 5, 8, 11];

    let handle = thread::spawn(|| {
        for i in fib {
            println!("Wątek liczy {i}");
            thread::sleep(DELAY);
        }
    });

    handle.join().expect("Błąd złączenia z wątkiem");
}
```

```text
error[E0373]: closure may outlive the current function, but it borrows `fib`, which is owned by the current function
  --> src/main.rs:8:32
   |
 8 |     let handle = thread::spawn(|| {
   |                                ^^ may outlive borrowed value `fib`
```

Poprawnie jest użyć `move` dla domknięcia, ponieważ domknięcie może potencjalnie żyć dłużej niż
tworząca go funkcja.

```rust
use std::{thread, time::Duration};

const DELAY: Duration = Duration::from_millis(1);

fn main() {
    let fib = [1, 2, 3, 5, 8, 11];

    let handle = thread::spawn(move || {
        for i in fib {
            println!("Wątek liczy {i}");
            thread::sleep(DELAY);
        }
    });

    handle.join().expect("Błąd złączenia z wątkiem");
}
```

# Komunikacja między wątkami

Kolejka typu _wielu producentów, jeden konsument_ (multi-producer, single-consumer; MPSC).

```rust
use std::{sync::mpsc::channel, thread};

fn main() {
    let fib = [1, 2, 3, 5, 8, 11];
    let (tx, rx) = channel();

    let handle = thread::spawn(move || {
        for i in fib {
            tx.send(i).expect("Błąd wysyłania");
            println!("Wątek wysłał {i}");
        }
    });

    while let Ok(i) = rx.recv() {
        println!("Główny wątek odebrał {i}");
    }

    handle.join().expect("Błąd złączenia z wątkiem");
}
```

Funkcja `channel` tworzy parę nadawca, odbiorca: `(Sender, Receiver)`. Nadawca wspiera klonowanie
(`Clone`), więc można go użyć np. w wielu wątkach.

```rust
use std::{sync::mpsc::channel, thread};

fn main() {
    let fib = [1, 2, 3, 5, 8, 11];
    let (tx, rx) = channel();
    let tx_clone = tx.clone();

    let handle1 = thread::spawn(move || {
        for i in fib {
            tx.send(i).expect("Błąd wysyłania");
            println!("Wątek 1 wysłał {i}");
        }
    });

    let handle2 = thread::spawn(move || {
        for i in 20..24 {
            tx_clone.send(i).expect("Błąd wysyłania");
            println!("Wątek 2 wysłał {i}");
        }
    });

    while let Ok(i) = rx.recv() {
        println!("Główny wątek odebrał {i}");
    }

    handle1.join().expect("Błąd złączenia z wątkiem 1");
    handle2.join().expect("Błąd złączenia z wątkiem 2");
}
```

Kanał utworzony przez `channel` posiada (teoretycznie) „nieskończony bufor” na wiadomości. Limit
bufora implementuje `sync_channel`, który jako argumenty przyjmuje taki limit. Przy wartości 0,
każda nadana wiadomość musi być odebrana zanim możliwe będzie nadanie kolejnej wiadomości.

```rust
use std::{sync::mpsc::sync_channel, thread};

fn main() {
    let fib = [1, 2, 3, 5, 8, 11];
    let (tx, rx) = sync_channel(2);
    let tx_clone = tx.clone();

    let handle1 = thread::spawn(move || {
        for i in fib {
            tx.send(i).expect("Błąd wysyłania");
            println!("Wątek 1 wysłał {i}");
        }
    });

    let handle2 = thread::spawn(move || {
        for i in 20..24 {
            tx_clone.send(i).expect("Błąd wysyłania");
            println!("Wątek 2 wysłał {i}");
        }
    });

    while let Ok(i) = rx.recv() {
        println!("Główny wątek odebrał {i}");
    }

    handle1.join().expect("Błąd złączenia z wątkiem 1");
    handle2.join().expect("Błąd złączenia z wątkiem 2");
}
```
