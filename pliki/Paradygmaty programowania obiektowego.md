# spowiedz studentów
Udzielając odpowiedzi na pytanie
K1 opowiedziałem o tym czym
jest dziedziczenie. Odpowiedź mi
przerwano i zapytano o to w jakiej
relacji znajdują się obiekty rodzica
i dziecka i czy jeśli się wykorzysta
obiekt dziecka w miejscu, gdzie
wymagany jest typ rodzica to co
się stanie. Powiedziałem wtedy o
polimorfizmie, zacząłem wtedy
opowiadać o tym czym jest
polimorfizm, po chwili znów mi
przerwano i dr Trajdos zapytał o
to czy wiem czym jest reguła
podstawień Liskov i żeby
opowiedział trochę o tym.
# opracowanko
## Paradygmaty programowania obiektowego.

Główne paradygmaty (filary) programowania obiektowego to: **Abstrakcja**, **Hermetyzacja (Enkapsulacja)**, **Dziedziczenie** i **Polimorfizm**. Służą one do tworzenia kodu, który jest łatwiejszy w utrzymaniu, modułowy i odwzorowuje rzeczywiste relacje biznesowe.​

---
### 1. Abstrakcja (Abstraction)

Modelowanie problemu poprzez ukrywanie zbędnych detali implementacyjnych i eksponowanie tylko istotnych cech obiektu. Pozwala skupić się na tym, _co_ obiekt robi, a nie _jak_ to robi.
- **Cel:** Zarządzanie złożonością systemu (complexity management).
- **W praktyce (Java):** Używanie interfejsów (`interface`) i klas abstrakcyjnych (`abstract class`). Użytkownik API (np. Twojego pluginu) widzi tylko metody `approve()` lub `reject()`, nie martwiąc się o to, jak pod spodem zapisywane są zmiany w bazie danych.
- **Pytanie pogłębiające na obronie:** _Czym różni się klasa abstrakcyjna od interfejsu?_
    - **Interfejs** definiuje kontrakt (zachowanie, "co potrafi"). Klasa może implementować wiele interfejsów.
    - **Klasa abstrakcyjna** definiuje tożsamość ("czym jest") i może zawierać wspólny stan (pola) oraz domyślną implementację.

### 2. Hermetyzacja / Enkapsulacja (Encapsulation)

Ukrywanie wewnętrznego stanu obiektu i wymuszanie dostępu do niego wyłącznie przez publiczne metody (interfejs). Często definiowana jako "wiązanie danych z metodami, które na nich operują".
- **Cel:** Ochrona integralności danych (invariants) i zmniejszenie sprzężenia (coupling).
- **Niuanse definicyjne:**
    - W ścisłej teorii **Enkapsulacja** to łączenie danych i zachowań w jedną "kapsułę" (klasę).
    - **Hermetyzacja** to ukrywanie danych (modyfikatory dostępu: `private`, `protected`). W praktyce te pojęcia są używane zamiennie.​
- **W praktyce:** Pola są `private`, a dostęp odbywa się przez gettery/settery (choć lepiej przez metody biznesowe, np. `bankAccount.withdraw(50)` zamiast `setBalance(getBalance() - 50)`). Dzięki temu, jeśli zmienisz sposób przechowywania danych (np. z `int` na `BigDecimal`), nie zepsujesz kodu w innych częściach systemu.

### 3. Dziedziczenie (Inheritance)

Mechanizm pozwalający jednej klasie (potomnej) przejąć cechy i metody innej klasy (bazowej). Tworzy relację typu "jest czymś" (IS-A).
- **Cel:** Ponowne użycie kodu (Reusability) i tworzenie hierarchii.
- **Pułapka (Ważne na 5.0):** Dziedziczenie jest silnym sprzężeniem (tight coupling). Zmiana w klasie bazowej może zepsuć klasy potomne (problem kruchej klasy bazowej).
- **Zasada Liskov (LSP z SOLID):** Klasa dziedzicząca musi móc zastąpić klasę bazową bez psucia działania programu. Jeśli musisz w podklasie rzucać wyjątek `NotImplementedException`, prawdopodobnie łamiesz zasadę dziedziczenia.
- **Kompozycja ponad Dziedziczenie (Composition over Inheritance):** To nowoczesne podejście. Zamiast dziedziczyć zachowanie, klasa powinna zawierać obiekt, który to zachowanie realizuje (relacja HAS-A). Jest to bardziej elastyczne (można podmienić zachowanie w runtime).​

### 4. Polimorfizm (Polymorphism)
Możliwość traktowania obiektów różnych typów w jednolity sposób (przez wspólną klasę bazową lub interfejs). "Jeden interfejs, wiele implementacji".
- **Typy polimorfizmu (częste pytanie):​
    - **Statyczny (Ad-hoc / Compile-time):** Przeciążanie metod (Overloading) – ta sama nazwa metody, ale różne parametry. Decyzja podejmowana podczas kompilacji.
    - **Dynamiczny (Subtype / Runtime):** Nadpisywanie metod (Overriding) – wywołanie metody na referencji typu ogólnego (np. `Animal`), ale wykonanie kodu z konkretnej instancji (np. `Dog`). Mechanizm ten działa dzięki **Wirtualnej Tablicy Metod (vtable)** (w C++) lub dynamicznemu wiązaniu w Javie.


KISS - Keep It Simple Stupid
DRY - Dont Repeat Yourself
YAGNI - You Aren't Gonna Need It

SOLID:

| Inicjał | Koncepcja                                                                                                                                                                                                                       |
| ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| S       | Single responsibility principle<br>Klasa powinna mieć tylko jedną odpowiedzialność (nigdy nie powinien istnieć więcej niż jeden powód do modyfikacji klasy).                                                                    |
| O       | Open/closed principle<br>Klasy (encje) powinny być otwarte na rozszerzenia i zamknięte na modyfikacje.                                                                                                                          |
| L       | Liskov substitution principle<br>Funkcje, które używają wskaźników lub referencji do klas bazowych, muszą być w stanie używać również obiektów klas dziedziczących po klasach bazowych, bez dokładnej znajomości tych obiektów. |
| I       | Interface segregation principle<br>Wiele dedykowanych interfejsów jest lepsze niż jeden ogólny.                                                                                                                                 |
| D       | Dependency inversion principle<br>Wysokopoziomowe moduły nie powinny zależeć od modułów niskopoziomowych – zależności między nimi powinny wynikać z abstrakcji.                                                                 |