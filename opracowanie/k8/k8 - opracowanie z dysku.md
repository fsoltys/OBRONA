# Mechanizmy systemu operacyjnego wspomagające synchronizację procesów.
Synchronizacja służy do koordynacji procesów/wątków współdzielących zasoby, aby uniknąć **wyścigu (Race Condition)**. Podstawowe mechanizmy dostarczane przez jądro systemu (kernel) to **Muteksy**, **Semafory** i **Zmienne warunkowe**, a na niższym poziomie **Blokady wirujące (Spinlocki)**.

---
## 1. Podstawowe mechanizmy synchronizacji
## **Muteks (Mutex - Mutual Exclusion)**
- **Co to jest:** Binarny "zamek" (0/1). Tylko jeden wątek może go posiadać w danej chwili. Jeśli inny wątek chce wejść, a muteks jest zajęty, system operacyjny (OS) **usypia** ten wątek (context switch).
- **Własność:** Muteks ma "właściciela". Tylko wątek, który zamknął muteks, może go otworzyć.
- **Koszt:** Uśpienie wątku jest kosztowne (przełączanie kontekstu zajmuje czas procesora), dlatego muteksy są dobre dla dłuższych sekcji krytycznych (np. zapis do pliku/bazy).
- **Przykład:** `synchronized` w Javie, `sync.Mutex` w Go.​
## **Semafor (Semaphore)**
- **Co to jest:** Licznik całkowity (nieujemny) z dwiema operacjami atomowymi: `P` (Wait/Acquire - zmniejsz) i `V` (Signal/Release - zwiększ).
- **Typy:**
    - _Binarny:_ Działa jak muteks, ale nie ma właściciela (jeden wątek może zablokować, inny odblokować).
    - _Licznikowy:_ Pozwala na dostęp do zasobu $N$ wątkom naraz (np. pula połączeń do bazy danych).
- **Różnica Muteks vs Semafor:** Muteks służy do ochrony danych (wykluczania), semafor służy do sygnalizacji i sterowania przepływem (np. "zadanie skończone, możesz brać wynik").​
## **Blokada wirująca (Spinlock)**
- **Co to jest:** Wątek nie idzie spać, tylko "kręci się" w pętli `while(isLocked) {}` sprawdzając, czy blokada puściła (busy-waiting).
- **Zalety:** Brak kosztownego przełączania kontekstu. Bardzo szybki dla _bardzo krótkich_ sekcji krytycznych.
- **Wady:** Zjada procesor (100% użycia rdzenia). W systemach jednoprocesorowych bez sensu. Używane głęboko w jądrze Linuxa.​​
## **Monitor**
- **Co to jest:** Wysokopoziomowa konstrukcja (np. w Javie), która łączy w sobie muteks (do ochrony danych) i zmienne warunkowe (do czekania). Metody oznaczone jako `synchronized` to w praktyce wejście do monitora obiektu​

---
## 2. Klasyczne problemy synchronizacji
## **Problem Czytelników i Pisarzy**
- **Scenariusz:** Baza danych. Wielu może czytać naraz, ale tylko jeden pisać (i nikt wtedy nie czyta).
- **Rozwiązanie:** Muteks dla pisarza i licznik dla czytelników. To podstawa implementacji `ReadWriteLock`.
## **Ucztujący filozofowie**
Głównym celem jest uniknięcie dwóch stanów patologicznych:
- **Zakleszczenie (Deadlock):** Sytuacja, w której każdy filozof podniósł lewy widelec i czeka na prawy. Prawy jest zajęty przez sąsiada, który też czeka. Nikt nie może ruszyć dalej. System "wisi".​
- **Zagłodzenie (Starvation):** Sytuacja, w której system działa (nie ma deadlocka), ale konkretny filozof pechowo nigdy nie dostaje obu widelców (bo szybcy sąsiedzi ciągle mu je podbierają).
## 2. Rozwiązania (Jak to naprawić?)
## **A. Rozwiązanie Naiwne (Błędne)**
- _Algorytm:_ "Weź lewy widelec, potem weź prawy. Jak masz oba – jedz."
- _Wada:_ Prowadzi do natychmiastowego **Deadlocka**, jeśli wszyscy zgłodnieją w tej samej milisekundzie (wszyscy mają lewy, nikt nie ma prawego).

## **B. Rozwiązanie z Kelnerem (Arbitrem) – **​
- _Pomysł:_ Wprowadzamy dodatkowy semafor (Kelnera), który pozwala siąść do stołu tylko 4 filozofom naraz.
- _Dlaczego działa?_ Jeśli przy stole jest max 4 osoby, a widelców jest 5, to zgodnie z zasadą szufladkową Dirichleta, przynajmniej jeden filozof będzie miał dostęp do dwóch widelców. Zje, odłoży sztućce i zwolni miejsce.
- _Wada:_ Ogranicza współbieżność (jeden filozof stoi w kolejce, mimo że mógłby jeść, bo jego widelce są wolne).
## **C. Rozwiązanie Asymetryczne (Hierarchia Zasobów) – **​
- _Pomysł:_ Ponumerujmy widelce od 0 do 4. Zasada: **Zawsze najpierw bierz widelec z niższym numerem.**
- _Działanie:_
    - Filozofowie 0, 1, 2, 3 biorą: najpierw Lewy (niższy), potem Prawy (wyższy).
    - **Filozof 4 (Ostatni):** Jego lewy to `4`, a prawy to `0`. Musi wziąć najpierw `0` (prawy), a potem `4` (lewy).
- _Efekt:_ Łamie to cykliczną zależność (Circle Wait Condition). Ostatni filozof rywalizuje o widelec `0` z pierwszym filozofem. Jeśli przegra – nie weźmie żadnego (nie zablokuje widelca `4`), więc jego sąsiad może jeść. **Eliminuje Deadlock.**

---
## 3. Zmienne Warunkowe (Condition Variables)
Bardzo ważne w Javie (`wait()`, `notify()`) i Go (`sync.Cond`).
- **Mechanizm:** Pozwalają wątkowi "spać" wewnątrz sekcji krytycznej (zwolnić muteks), czekając na spełnienie pewnego warunku (np. "czy bufor nie jest już pusty?").
- **Schemat działania:**
    1. Weź Muteks.
    2. Sprawdź warunek (w pętli `while`!).
    3. Jeśli niespełniony -> `wait()` (OS zdejmuje blokadę i usypia wątek).
    4. Inny wątek zmienia stan i robi `signal()`/`notify()`.
    5. Obudzony wątek ponownie bierze Muteks i sprawdza warunek.