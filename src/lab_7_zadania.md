# Zadania do samodzielnego wykonania

1. Utwórz iterator liczb od 1 do 10 i zastosuj `map`, by każdą wartość podnieść do kwadratu. Wypisz liczby i ich kwadraty w tabeli równo pod sobą, jak poniżej:
```text
|  1|  2|  3|  4|  5|  6|  7|  8|  9| 10|
-----------------------------------------
|  1|  4|  9| 16| 25| 36| 49| 64| 81|100|
````

2. Utwórz iterator, który przy pomocy `scan` generuje sumę bieżącą liczb od 1 do 10 włącznie (czyli `[1, 3, 6, 10, 15, 21, 28, 36, 45, 55]`)

3. Mając wektor napisów `["42", "x", "33", "z", "25"]`, przekształć go w liczby całkowite, pomijając błędne wpisy. Użyj `filter_map`.

4. Zaimplementuj iterator `ParityCounter`, który zadanego przedziału `[start, end]` generuje tylko liczby parzyste albo tylko liczby nieparzyste – w zależności od ustawienia.

Przykład użycia:

```rust,ignore
let evens: Vec<u32> = ParityCounter::evens(1, 10).collect();
let odds: Vec<u32> = ParityCounter::odds(1, 10).collect();
```

## Podpowiedź

```rust,ignore
#[derive(Debug)]
struct ParityCounter {
    current: u32,
    end: u32,
    is_even: bool,
}

impl ParityCounter {
    fn new(start: u32, end: u32, is_even: bool) -> Self {
        let mut current = start;

		//logika
		
        ParityCounter {
            current,
            end,
            is_even,
        }
    }

    pub fn evens(start: u32, end: u32) -> Self {
        ParityCounter::new(start, end, true)
    }

    pub fn odds(start: u32, end: u32) -> Self {
        ParityCounter::new(start, end, false)
    }
}

impl Iterator for ParityCounter {
    type Item = u32;

    fn next(&mut self) -> Option<Self::Item> {
		//logika
    }
}

fn main() {
    let evens: Vec<u32> = ParityCounter::evens(1, 10).collect();
    let odds: Vec<u32> = ParityCounter::odds(1, 10).collect();

    println!("Evens: {:?}", evens); 
    println!("Odds:  {:?}", odds); 
}
```


 