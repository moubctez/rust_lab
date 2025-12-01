# Zadania do samodzielnego wykonania

1. Dlaczego poniższy kod nie kompiluje się poprawnie? Co należy zrobić aby się kompilował?

   ```rust,compile_fail
   mod house {
       mod bedroom {
           fn sleep() {}
       }
   }

   fn main() {
       house::bedroom::sleep();
   }
   ```

2. Pobrać źródła [rata_demo](https://detox.wi.zut.edu.pl/pb/rust/src/rata_demo/), uruchomić,
   podzielić na moduły (najlepiej aby każda struktura była w osobnym module), dołożyć obsługę
   klawisza Enter, które wciśnięcie ma powodować wejście do katalogu, na którym znajduje się kursor.

3. Pobrać źródła [ggez_demo](https://detox.wi.zut.edu.pl/pb/rust/src/ggez_demo/) lub
   [piston_demo](https://detox.wi.zut.edu.pl/pb/rust/src/piston_demo/), uruchomić, podzielić na
   moduły (najlepiej aby każda struktura była w osobnym module), dołożyć zmianę koloru węża: im
   dalej od głowy wąż powinien być ciemniejszy. Wymyśleć i dodać własne udoskonalenie(a).
