# Normalizacja schematu bazy danych.
Normalizacja to proces organizowania danych w bazie, mający na celu **zminimalizowanie redundancji (powtórzeń)** oraz **uniknięcie anomalii** (wstawiania, aktualizacji i usuwania). Polega na podziale dużych tabel na mniejsze i powiązaniu ich związkami. Proces ten przeprowadza się etapami zwanymi **postaciami normalnymi (1NF, 2NF, 3NF, BCNF)**

## 1. Cele normalizacji
- **Eliminacja redundancji:** Dane są przechowywane w jednym miejscu. Zmniejsza to rozmiar bazy, ale przede wszystkim ułatwia utrzymanie spójności.
- **Zapobieganie anomaliom:**
    - **Anomalia wstawiania (Insertion Anomaly):** Sytuacja, gdy nie można dodać danych do bazy, bo brakuje pełnych informacji. 
    - **Anomalia aktualizacji (Update Anomaly):** Zmiana danych w jednym miejscu wymaga aktualizacji wielu wierszy. 
    - **Anomalia usuwania (Deletion Anomaly):** Usunięcie rekordu powoduje niezamierzoną utratę innych informacji. 

## 2. Postacie Normalne
Zazwyczaj na obronie wystarczy znać 3 pierwsze postacie (3NF jest standardem przemysłowym).
### 1NF – Pierwsza Postać Normalna (Atomowość)
Tabela znajduje się w **pierwszej postaci normalnej**, jeśli:
- każda kolumna zawiera wartości **atomowe** (niepodzielne),
- nie występują powtarzające się grupy danych,
- każdy rekord jest jednoznacznie identyfikowany kluczem głównym.
Przykładowo, zamiast przechowywać listę numerów telefonów w jednej kolumnie, każdy numer powinien być osobnym rekordem w powiązanej tabeli.

### 2NF – Druga Postać Normalna (Pełna zależność funkcyjna)
Tabela spełnia **drugą postać normalną**, jeśli:
- znajduje się w 1NF,
- wszystkie atrybuty niekluczowe są **w pełni zależne od całego klucza głównego**, a nie tylko od jego części.

2NF ma znaczenie głównie dla tabel z **kluczem złożonym**. Jej celem jest usunięcie zależności częściowych, przykładowo jeśli rekord zawiera `PozycjeZamowien(ID_Zamowienia, ID_Produktu, NazwaProduktu, Cena, Ilosc)`, atrybuty `Cena` i `Nazwa` zależą od `ID_Produktu`, nie od klucza złożonego, więc powinny być wydzielone w osobnej tabeli `Produkty`
### 3NF – Trzecia Postać Normalna (Brak zależności przechodnich)
Tabela znajduje się w **trzeciej postaci normalnej**, jeśli:
- spełnia 2NF,
- nie zawiera **zależności przechodnich** między atrybutami niekluczowymi.

Oznacza to, że atrybuty niekluczowe powinny zależeć wyłącznie od klucza głównego, a nie od innych atrybutów niekluczowych. Przykładowo, jeśli kod pocztowy determinuje nazwę miasta, to miasto nie powinno znajdować się w tej samej tabeli co klient, lecz w osobnej tabeli powiązanej.

## Poprawa spójności i integralności danych
Normalizacja bezpośrednio przyczynia się do poprawy **spójności danych**, ponieważ każda informacja jest przechowywana w jednym, jednoznacznym miejscu. Zmiana danych (np. adresu klienta) wymaga aktualizacji tylko jednego rekordu, co eliminuje ryzyko sprzecznych informacji.

Dodatkowo, dobrze znormalizowany schemat ułatwia egzekwowanie **integralności danych** poprzez:
- klucze główne (unikalność rekordów),
- klucze obce (spójność relacji),
- ograniczenia (NOT NULL, UNIQUE, CHECK).

Dzięki temu baza danych staje się bardziej odporna na błędy logiczne oraz łatwiejsza w utrzymaniu i rozwoju.
## 3. Denormalizacja

**Denormalizacja** polega na świadomym wprowadzeniu kontrolowanej redundancji danych w celu **zwiększenia wydajności odczytu** i uproszczenia zapytań. Stosuje się ją głównie w systemach, gdzie liczba odczytów znacząco przewyższa liczbę zapisów (np. raportowanie, hurtownie danych). Realizuje się ją poprzez dodawanie pochodnych kolumn, łączenie tabel lub przechowywanie zagregowanych danych. Odbywa się to kosztem większego ryzyka niespójności, dlatego wymaga dodatkowych mechanizmów kontroli (triggery, logika aplikacyjna).