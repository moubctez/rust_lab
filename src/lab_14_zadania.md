# Zadania do samodzielnego wykonania
1. Napisz **CLI**, które pobiera **wiele plików równolegle** z listy URL-i, z raportowaniem postępu (per plik i globalnie),
	Program przyjmuje listę źródeł:
	* `--input <PATH>` plik tekstowy (1 URL na linię; puste linie ignorowane).
	* `--out <DIR>`: katalog docelowy (tworzony jeśli nie istnieje).
	* Nazwa pliku:
	  * domyślnie z ostatniego segmentu ścieżki URL,
	  * jeśli brak lub kończy się `/`, generujemy nazwę (np. `file-0001`),
	  * jeśli kolizja nazw: dodajemy sufiks `(...)-1`, `(...)-2` itd.
	### Pobieranie (HTTP)
	* Asynchroniczne pobieranie z `reqwest`.
	* Obsłuż przekierowania (domyślnie OK).
	* Zapis do pliku **strumieniowo** (bez trzymania całości w RAM).
	* Jeśli `Content-Length` dostępne: pokazuj % dla danego pliku.
	* Jeśli brak `Content-Length`: pokazuj pobrane bajty i prędkość.
	### Śledzenie postępu
	* Tryb tekstowy:
	* Minimalnie:
	  * globalnie: liczba ukończonych / wszystkich, suma pobranych bajtów, prędkość,
	  * per plik: stan (`queued/downloading/done/error`), pobrane bajty, % jeśli znane.
	* Log końcowy: tabela wyników (URL → status → ścieżka pliku / błąd).
	### Dodatkowo
	* Obsłuż przerwanie (Ctrl+C): łagodne zatrzymanie i pozostawienie `.part`.
	* Pobieraj do `*.part`, a po sukcesie atomowo zmień nazwę na docelową.
	* Opcja `--resume`:

	  * jeśli istnieje `*.part`, spróbuj wznowić `Range: bytes=<offset>-` (jeśli serwer wspiera).
	  * jeśli serwer nie wspiera: pobierz od nowa.
	* Opcja `--overwrite`:
	  * bez niej: jeśli plik docelowy istnieje i jest kompletny, pomiń lub zmień nazwę.

3. Praca nad własnym projektem: aktualne źródła projektu dołączyć (tylko pliki `*.rs` i`Cargo.toml`) do wysyłanych zadań.