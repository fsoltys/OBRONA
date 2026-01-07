# Język UML w projektowaniu oprogramowania.
UML (**Unified Modeling Language**) to znormalizowany język służący do wizualizacji, specyfikowania, konstruowania i dokumentowania elementów systemów informatycznych. Nie jest językiem programowania, lecz językiem modelowania. Standardem zarządza organizacja OMG (Object Management Group).​

---
## 1. Podział diagramów UML
## **A. Diagramy Strukturalne (Statyczne)**
Pokazują budowę systemu ("co to jest?"), jego elementy i związki między nimi, bez uwzględniania czasu.
- **Diagram Klas (Class Diagram):** Najważniejszy dla programisty. Pokazuje klasy, interfejsy i relacje (dziedziczenie, kompozycję). To wprost przekłada się na kod Javy.
- **Diagram Komponentów (Component Diagram):** Pokazuje fizyczne moduły kodu (np. plik `.jar` ) i zależności między nimi.
- **Diagram Wdrożenia (Deployment Diagram):** Pokazuje sprzęt i infrastrukturę (Węzły/Nodes), na których działa soft.

## **B. Diagramy Behawioralne (Dynamiczne)**
Pokazują zachowanie systemu ("jak to działa?"), przepływ sterowania i zmiany w czasie.
- **Diagram Przypadków Użycia (Use Case Diagram):** Pokazuje funkcjonalności z perspektywy użytkownika (Aktorów). Np. "Aktor: Reviewer" -> "Przypadek: Dodaj komentarz".
- **Diagram Sekwencji (Sequence Diagram):** Pokazuje interakcję obiektów w czasie. Kluczowe jest tu **uporządkowanie czasowe** (oś czasu biegnie w dół). Skupia się na **wymianie komunikatów** między obiektami ("Kto do kogo dzwoni i w jakiej kolejności"). Ważne są "Linie życia" (Lifelines).​
- **Diagram Aktywności (Activity Diagram):** Przypomina stary schemat blokowy (flowchart). Pokazuje przepływ sterowania (algorytm), decyzje (romby) i zrównoleglenie wątków. ("Co robić krok po kroku"). Ważne są "Tory" (Swimlanes), pokazujące kto wykonuje daną czynność.

---
## 2. Relacje w Diagramie Klas

| Związek                         | Symbol                            | Znaczenie w Javie                                                     |
| ------------------------------- | --------------------------------- | --------------------------------------------------------------------- |
| **Asocjacja**                   | Zwykła linia (czasem ze strzałką) | Luźny związek ("korzysta z"). Pole w klasie.                          |
| **Agregacja**                   | Pusty romb (◇)                    | Relacja "całość-część", ale słaba. Część może istnieć bez całości.    |
| **Kompozycja**                  | Pełny romb (◆)                    | Relacja "całość-część" silna. Część **nie może** istnieć bez całości. |
| **Dziedziczenie (Uogólnienie)** | Pusty trójkąt (△)                 | `extends` lub `implements`.                                           |

---

## 3. Model "4+1" Philippe'a Kruchtena

 UML najlepiej stosować w ramach modelu widoków architektury **4+1**:
1. **Widok Logiczny:** Diagramy Klas (struktura kodu).
2. **Widok Procesów:** Diagramy Aktywności/Sekwencji (wątki, synchronizacja).
3. **Widok Fizyczny (Wdrożenia):** Diagram Wdrożenia (serwery, chmura).
4. **Widok Implementacji:** Diagramy Komponentów/Pakietów (pliki, biblioteki).
5. **+1 (Scenariusze):** Diagramy Przypadków Użycia, które spajają to wszystko (pokazują, po co system w ogóle jest).