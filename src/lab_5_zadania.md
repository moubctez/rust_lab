# Zadania do samodzielnego wykonania
1. Zdefiniuj strukturę `Book`, która zawiera pola:
    - `title` – napis (`String`),
    - `author` – napis (`String`),
    - `pages` – liczba całkowita (`u32`),
    - `available` – wartość logiczna (`bool`).
    
    Następnie:
    - Utwórz kilka instancji tej struktury.
    - Wypisz na ekran tytuły wszystkich książek, które są dostępne (`available == true`).

2. Utwórz strukturę krotkową `RGB(u8, u8, u8)` reprezentującą kolor. Napisz funkcję `print_color`, która przyjmuje parametr typu RGB i wypisuje jego składniki w formacie:
   ```text
   R=..., G=..., B=...
   ```

3. Zdefiniuj enumerację Shape, która może przyjmować wartości:
   ```rust,ignore
   Circle { radius: f64 },
   Rectangle { width: f64, height: f64 },
   Square(f64).
   ```
   Napisz funkcję `area(shape: Shape) -> f64`, która zwraca pole danego kształtu, wykorzystując dopasowywanie wzorców (`match`).

4. Zdefiniuj strukturę User zawierającą pola:
   ```rust,ignore
   id: u32,
   name: String,
   active: bool
   ```
	Dodaj metody:
   ```rust,ignore
	fn new(id: u32, name: String) -> Self // tworzy nowego użytkownika (domyślnie aktywnego)
	fn deactivate(&mut self) // ustawia active na false
	fn is_active(&self) -> bool //zwraca informację o aktywności.
	```
	Przetestuj wszystkie metody w funkcji main.
	
5. Rozszerz strukturę:

   ```rust,ignore
	struct Rectangle {
		width: u32,
		height: u32,
	}
	```
	o następujące metody:
   ```rust,ignore
	fn area(&self) -> u32
	fn can_contain(&self, other: &Rectangle) -> bool // zwraca true, jeśli bieżący prostokąt może pomieścić other (oba wymiary większe).
	fn resize(&mut self, width: u32, height: u32) //zmienia rozmiary prostokąta.
	```
	Napisz prosty program testujący powyższe metody.

6. Do zdefiniowanych struktur:
   ```rust,ignore
	struct User {
		id: u32,
		name: String,
		active: bool,
	}

	struct Users {
		list: Vec<User>,
	}
	```
	Dodaj metody:
	```rust,ignore
	fn new() -> Self //tworzy pustą kolekcję,
	fn add(&mut self, user: User) //dodaje użytkownika,
	fn find_by_name(&self, name: &str) -> Option<&User> //zwraca użytkownika o danym imieniu,
	fn active_users(&self) -> Vec<&User> //zwraca tylko aktywnych użytkowników,
	fn deactivate_all(&mut self) //ustawia active = false dla wszystkich.
	```
	W main utwórz kolekcję, dodaj kilku użytkowników i przetestuj wszystkie metody.