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
