# Iteratory
Iterator to obiekt dostarczający sekwencję wartości na żądanie. Odbywa się to leniwie — element powstaje dopiero przy konsumpcji. Iteratory są zero-cost: kompilator agresywnie inline’uje i optymalizuje.
```rust
    let v = [4, 1, 3];
    let it = v.iter().map(|x| x * 2); 
    let out: Vec<_> = it.collect(); 
    assert_eq!(out, [8, 2, 6]);
```

# Iteratory - trait
Trait `Iterator` definiuje jedną wymaganą metodę: `fn next(&mut self) -> Option<Self::Item>`, oprócz tego duża liczba metod domyślnych (adaptery + konsumenci).

```rust
pub trait Iterator {
    type Item;
    fn next(&mut self) -> Option<Self::Item>;
    // ... domyślne implementacje metod
}
```
Przykładowa implementacja własnego iteratora:
```rust
struct Counter { cur: u32, end: u32 }

impl Counter {
	fn new(end: u32) -> Self { Self { cur: 0, end } }
}

impl Iterator for Counter {
	type Item = u32;
	fn next(&mut self) -> Option<Self::Item> {
		if self.cur < self.end {
			let v = self.cur;
			self.cur += 1;
			Some(v)
		} else { None }
	}
}

fn main() {
	let nums: Vec<_> = Counter::new(3).collect();
	for x in nums {
		println!("x: {x}");
	}
}
```

# Iteratory - iterowanie
Dostęp do elementów:
* `iter()` → elementy typu `&T` (pożyczone do odczytu)
* `iter_mut()` → `&mut T` (modyfikowalne referencje)
* `into_iter()` → przenosi kolekcję, elementy by‑value
```rust
let mut v = vec![3, 4, 8];
for r in v.iter_mut() { *r *= 3; }
assert_eq!(v, vec![9, 12, 24]);
for x in v.into_iter() {
	println!("x: {x}");
}
```

Pętle for używają `IntoIterator` na danym typie.
Przykładowo dla `Vec<T>`:
* `for x in &v` → `&T`
* `for x in &mut v` → `&mut T`
* `for x in v` → `T` (przenosi v)
# Iteratory - adaptery

## Konsumenci (consuming adapters)
Zjadają cały iterator i zwracają wynik:
`collect`, `count`, `sum`, `product`, `all`, `any`, `find`, `for_each`, `last`, `max_by`, `min_by`, itd.
```rust,editable
fn main() {
    let nums = 1..=10;

    let count = nums.clone().count();
    println!("Liczba elementów: {count}");

    //let sum: i32 = nums.sum();
    let sum: i32 = (1..=10).sum();
    println!("Suma: {sum}");

    let first_even = (1..=10).find(|x| x % 2 == 0);
    println!("Pierwsza liczba parzysta: {:?}", first_even);

    let all_positive = (1..=10).all(|x| x > 0);
    let any_multiple_of_3 = (1..=10).any(|x| x % 3 == 0);
    println!("Wszystkie dodatnie: {all_positive}", );
    println!("Jakaś wielokrotność 3: {any_multiple_of_3}");

    nums.for_each(|x| print!("{} ", x * x));
    println!();
}

```
## Adaptery transformujące
Zwracają nowy iterator: `map`, `filter`, `filter_map`, `scan`, `take_while`, `skip_while`, `inspect`, itd.
```rust
fn main() {
    let first_ten_doubled: Vec<_> = (1..).map(|x| x * 2).take(10).collect();
    println!("map + take => {:?}", first_ten_doubled);

    let evens: Vec<_> = (1..=20).filter(|x| x % 2 == 0).collect();
    println!("filter => {:?}", evens);

    let parsed: Vec<i32> = ["1", "x", "3", "x", "5"]
        .into_iter()
        .filter_map(|s| s.parse::<i32>().ok())
        .collect();
    println!("filter_map => {:?}", parsed); 

    let prefix_sums: Vec<i32> = (1..=5)
        .scan(0, |acc, x| { *acc += x; Some(*acc) })
        .collect();
    println!("scan (prefix sums) => {:?}", prefix_sums); // [1, 3, 6, 10, 15]

    let small_squares: Vec<_> = (1..)
        .map(|x| x * x)
        .take_while(|&s| s <= 50)
        .collect();
    println!("take_while (squares <= 50) => {:?}", small_squares); 

    let after_five: Vec<_> = (1..)
        .skip_while(|&x| x < 5)
        .take(3)
        .collect();
    println!("skip_while + take => {:?}", after_five); 

    let sum_even_squares: i32 = (1..=10)
        .map(|x| x * x)
        .inspect(|s| println!("inspect: square = {}", s))
        .filter(|s| s % 2 == 0)
        .sum();
    println!("sum (even squares 1..=10) => {}", sum_even_squares);
}
```
## Adaptery strukturalne
Kształtują przepływ: `take`, `skip`, `step_by`, `rev`, `cycle`, `chain`.
```rust
fn main() {
    let first_five: Vec<_> = (1..=10).take(5).collect();
    println!("take => {:?}", first_five);

    let after_five: Vec<_> = (1..=10).skip(5).collect();
    println!("skip => {:?}", after_five);

    let every_second: Vec<_> = (1..=10).step_by(2).collect();
    println!("step_by(2) => {:?}", every_second);

    let reversed: Vec<_> = (1..=5).rev().collect();
    println!("rev => {:?}", reversed);

    let cycled: Vec<_> = [1, 2, 3].into_iter().cycle().take(8).collect();
    println!("cycle + take(8) => {:?}", cycled);

    let combined: Vec<_> = (1..=3).chain(7..=9).collect();
    println!("chain => {:?}", combined);
}

```
UWAGA: cycle — powtarza sekwencję w nieskończoność, zawsze trzeba ograniczać np. przez `take`, inaczej program się zapętli!

