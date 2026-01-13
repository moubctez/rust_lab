# Zadania do samodzielnego wykonania
1. **Start wątku i przeplot wypisów**
   Przepisz program uruchamiający 1 wątek przez `thread::spawn`, gdzie oba wątki wypisują liczniki z `sleep`. Zaobserwuj przeplot wyjścia.

2. **Agregator: suma i statystyki w jednym konsumencie**
   Zbuduj program, w którym **dwa wątki–producenci** wysyłają liczby do **jednego konsumenta** przez kanał `std::sync::mpsc`. Konsument ma policzyć i wypisać raport: **liczność, suma, min, max, średnia**. 
   **Źródła danych (producenci)**
   * Producent A wysyła liczby z tablicy (np. `fib = [1, 2, 3, 5, 8, 11]`).
   * Producent B wysyła liczby z zakresu (np. `30..35`).
   * Każda wiadomość ma zawierać identyfikator producenta.
   
   **Protokół wiadomości**
   * Zdefiniuj:
     ```rust
     enum Msg {
         Data { producer: u8, value: i64 },
         Stop { producer: u8 },
     }
     ```
   * Każdy producent wysyła `Stop` na końcu.
   
   **Konsument (agregator)**
   * Odbiera wiadomości `recv()` w pętli.
   * Kończy dopiero po otrzymaniu `Stop` od obu producentów.
   * Liczy statystyki **globalnie** (ze wszystkich danych) oraz **po producencie** (osobno dla A i B).
   
   **Raport końcowy**
   * Dla całości i dla każdego producenta wypisz:
     * `count`, `sum`, `min`, `max`, `avg` (średnia jako `f64`).
   * Jeżeli producent nie wysłał żadnych danych (count=0), `min/max/avg` mają być „brak” (np. jako `None`).
   
   * Użyj `thread::spawn` + `move`, `tx.clone()` dla drugiego producenta.
   * Zadbaj o deterministyczne zakończenie (bez wiszenia): konsument kończy na podstawie `Stop`, a wątki są łączone przy pomocy `join()`.
   * Bez `unwrap()`/`expect()` w logice (dopuszczalne w `main` do uproszczenia *tylko* w miejscach, które uznajesz za „nie powinno się nigdy zdarzyć”, ale preferowane jest `Result`).

## Propozycje struktur i metod
* `Stats`:
  * pola: `count: u64`, `sum: i64`, `min: Option<i64>`, `max: Option<i64>`
  * metody:
    * `update(value: i64)`
    * `avg() -> Option<f64>`
* `run_producer(tx: Sender<Msg>, id: u8, data: impl IntoIterator<Item=i64>)`
* `run_aggregator(rx: Receiver<Msg>, producer_count: usize) -> (Stats, HashMap<u8, Stats>)`

**Raport czasu:** dodaj pomiar czasu przetwarzania (np. `Instant::now()`).

3. Praca nad własnym projektem: aktualne źródła projektu dołączyć (tylko pliki `*.rs` i`Cargo.toml`) do wysyłanych zadań.