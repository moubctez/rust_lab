# Zadania do samodzielnego wykonania

1. Do celów kryptograficznych zdefiniowany jest klucz jako:

   ```rust
   const KEY_LEN: usize = 32;
   struct Key([u8; KEY_LEN]);
   ```

   Napisz metodę, która z ciągu znaków będących cyframi szesnastkowymi utworzy nową strukturę `Key`.
   Metoda powinna zwrócić błąd w przypadku gdy:

   - podany ciąg znaków jest zbyt długi
   - w podanym ciągu znaków znajdują się niedozwolone znaki

   Taki błąd może być zaimplementowany na przykład tak:

   ```rust
   pub enum DecodeError {
      InvalidLength(usize),
      InvalidChar(char),
   }
   ```
