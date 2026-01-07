1. dotyczy relacyjnych BD,
spójność, anomalie, 1, 2, 3 NF ->
opis + przykłady, BC, 4, 5 NF ->
nieudolny opis + nieudolne przy
kłady. Pytanie o wady (objętość
danych, złożone zapytania)

2. Przy pytaniu kierunkowym trzeba
opisać dwie postacie normalne
(dowolne). Warto przygotować
sobie przykłady jak dana baza
danych nie spełnia założeń danej
postaci i jak należy ją
zmodyfikować, żeby spełniała.

3. Przy normalizacji przy drugiej
postaci padło pytanie o rodzaj
uzależnienia funkcyjnego

# opracownaie z dysku
# Normalizacja schematu bazy danych.
Normalizacja to proces organizowania danych w bazie, mający na celu **zminimalizowanie redundancji (powtórzeń)** oraz **uniknięcie anomalii** (wstawiania, aktualizacji i usuwania). Polega na podziale dużych tabel na mniejsze i powiązaniu ich związkami. Proces ten przeprowadza się etapami zwanymi **postaciami normalnymi (1NF, 2NF, 3NF, BCNF)**​

---

## 1. Cele normalizacji (Dlaczego to robimy?)
- **Eliminacja redundancji:** Dane są przechowywane w jednym miejscu. Zmniejsza to rozmiar bazy, ale przede wszystkim ułatwia utrzymanie spójności.
- **Zapobieganie anomaliom:**
    - **Anomalia wstawiania (Insertion Anomaly):** Sytuacja, gdy nie można dodać danych do bazy, bo brakuje pełnych informacji. _Przykład:_ W tabeli `Studenci_Przedmioty` nie możesz dodać nowego przedmiotu, jeśli jeszcze żaden student się na niego nie zapisał (musiałbyś wstawić NULL jako ID studenta, co może być zabronione przez PK).​
    - **Anomalia aktualizacji (Update Anomaly):** Zmiana danych w jednym miejscu wymaga aktualizacji wielu wierszy. _Przykład:_ Jeśli nazwisko wykładowcy jest wpisane przy każdym studencie, jego zmiana (np. po ślubie) wymaga edycji tysięcy rekordów. Błąd w jednym wierszu powoduje niespójność danych.​
    - **Anomalia usuwania (Deletion Anomaly):** Usunięcie rekordu powoduje niezamierzoną utratę innych informacji. _Przykład:_ Usunięcie ostatniego studenta z kursu powoduje, że tracimy informację o tym, że taki kurs w ogóle istnieje (jeśli kursy są tylko atrybutem studenta).​

## 2. Postacie Normalne (Teoria + Przykłady)
Zazwyczaj na obronie wystarczy znać 3 pierwsze postacie (3NF jest standardem przemysłowym).
## **1NF – Pierwsza Postać Normalna (Atomowość)**
- **Zasada:** Wartości w kolumnach muszą być atomowe (niepodzielne). Brak powtarzających się grup danych.
- **Zły przykład:** Tabela `Changes` ma kolumnę `reviewers`, w której po przecinku wpisane są ID recenzentów: `"101, 102, 105"`.
- **Rozwiązanie (1NF):** Każdy recenzent w osobnym wierszu lub (lepiej) wydzielenie tabeli łącznikowej `Change_Reviewers`.​

## **2NF – Druga Postać Normalna (Pełna zależność funkcyjna)**
- **Zasada:** Baza jest w 1NF **ORAZ** każda kolumna niebędąca kluczem zależy od **całego** klucza głównego, a nie tylko od jego części (dotyczy tabel z kluczem złożonym).
- **Zły przykład:** Tabela `Oceny` z kluczem złożonym `(StudentID, CourseID)` zawiera kolumnę `CourseName`.
    - `CourseName` zależy tylko od `CourseID` (części klucza), a nie od `StudentID`.
- **Rozwiązanie (2NF):** Wyciągnięcie `CourseName` do osobnej tabeli `Courses`.

## **3NF – Trzecia Postać Normalna (Brak zależności przechodnich)**
- **Zasada:** Baza jest w 2NF **ORAZ** kolumny niekluczowe nie mogą zależeć od innych kolumn niekluczowych (tylko bezpośrednio od klucza głównego).
- **Zły przykład:** Tabela `Users` zawiera: `UserID` (PK), `ZipCode`, `City`.
    - `City` zależy od `ZipCode`, a `ZipCode` od `UserID`. To zależność przechodnia: `UserID -> ZipCode -> City`.
- **Rozwiązanie (3NF):** Tabela `ZipCodes` (`ZipCode`, `City`) i w tabeli `Users` zostaje tylko `ZipCode`.​

---
## 3. Denormalizacja (Kiedy łamać zasady?)
To kluczowy punkt dla inżyniera – pokazujesz, że rozumiesz kompromis między teorią a wydajnością (performance trade-off).
- **Co to jest:** Świadome wprowadzanie redundancji (duplikacji) do znormalizowanej bazy.​
- **Kiedy stosować:**
    1. **Read-heavy systems:** Gdy system wykonuje dużo więcej odczytów niż zapisów (np. raportowanie, hurtownie danych).
    2. **Unikanie JOIN-ów:** Zbyt wiele złączeń tabel (JOIN) w zapytaniu SQL drastycznie spowalnia aplikację.
    3. **Obliczenia kosztowne (Precomputed fields):** Np. przechowywanie aktualnego `total_score` w tabeli `Changes`, zamiast liczyć sumę głosów za każdym razem przy wyświetlaniu listy zmian.​
- **Wady:** Ryzyko niespójności danych (musisz pamiętać o aktualizacji w wielu miejscach – np. triggerami lub w kodzie aplikacji) oraz większe zużycie dysku.​