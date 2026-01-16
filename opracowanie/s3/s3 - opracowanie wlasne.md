Harmonogram projektu to plan, który odpowiada na pytania: **co robimy, w jakiej kolejności, ile to trwa i kto to wykonuje**. W praktyce powstaje on etapami: najpierw porządkujemy zakres prac, potem ustalamy zależności i czasy trwania, a na końcu dopasowujemy plan do realnej dostępności zasobów (ludzi, sprzętu).
### WBS – Work Breakdown Structure

Zwykle zaczyna się od **WBS**, czyli struktury podziału pracy. WBS polega na rozbiciu projektu na mniejsze elementy: fazy → zadania → podzadania, aż do poziomu „pakietów roboczych”, które da się jednoznacznie opisać i oszacować. WBS nie jest jeszcze harmonogramem w czasie, ale jest jego podstawą — bez niego łatwo pominąć zadania lub oszacować je zbyt ogólnie.

**WBS pomaga:**
- uporządkować zakres i odpowiedzialności,
- wycenić czas i koszty,
- przygotować podstawę do harmonogramu i kontroli postępu.
### Diagram Gantta

Kiedy znamy listę zadań i ich czasy, typową formą harmonogramu jest **diagram Gantta**. Jest to najbardziej klasyczny sposób prezentacji harmonogramu: zadania są pokazane jako **paski na osi czasu**. Zwykle zawiera:
- daty start/koniec,
- czasy trwania,
- zależności między zadaniami (np. „zaczyna się po zakończeniu X”),
- kamienie milowe (milestones).

**Zalety:** czytelna wizualizacja, łatwo zobaczyć równoległość i terminy.  
**Wady:** w dużych projektach może być mało przejrzysty i nie pokazuje tak jasno „wąskich gardeł” jak metody sieciowe.

### Metody sieciowe (CPM/PERT) i ścieżka krytyczna

Aby lepiej analizować wpływ opóźnień, stosuje się metody sieciowe, które opisują projekt jako **graf zależności zadań**.

- **CPM (Critical Path Method)** pozwala wyznaczyć **ścieżkę krytyczną** — najdłuższy ciąg zależnych zadań, który wyznacza minimalny czas trwania projektu.  
  Zadania na ścieżce krytycznej mają **zerowy zapas czasu (slack/float)**: opóźnienie któregokolwiek opóźnia cały projekt.
- **PERT** jest podobny, ale częściej używany, gdy czasy są niepewne: dla zadania przyjmuje się trzy estymacje (optymistyczną, najbardziej prawdopodobną i pesymistyczną), a potem oblicza czas oczekiwany.

### Zarządzanie czasem i zależnościami
Harmonogram buduje się na dwóch filarach:
1. **Estymacja czasu trwania zadań** – np. w dniach roboczych; często stosuje się bufor na ryzyko.
2. **Zależności między zadaniami** – typowo:
    - Finish-to-Start (B po zakończeniu A),
    - Start-to-Start (B może ruszyć, gdy A ruszy),
    - Finish-to-Finish (B kończy się razem z A).

Dobre praktyki to również:
- określanie kamieni milowych,
- planowanie iteracji / sprintów,
- regularna aktualizacja harmonogramu na podstawie postępu.

### Zarządzanie zasobami

Harmonogram musi być realny także pod kątem ludzi i sprzętu. Częstym problemem jest przeciążenie zasobów (ta sama osoba na kilka równoległych zadań). Stosuje się wtedy techniki planowania zasobów:
- **resource leveling** – wygładzanie obciążenia kosztem możliwego wydłużenia projektu,
- **resource smoothing** – wygładzanie obciążenia bez zmiany terminu końcowego (jeśli istnieje zapas czasu).

### Inne podejścia, które często się pojawiają

W zależności od projektu stosuje się też:
- **Planowanie iteracyjne (Agile/Scrum)** – harmonogram buduje się w krótkich cyklach (sprintach, np. 1–2 tygodnie): szczegółowo planuje się tylko najbliższy sprint (zakres, priorytety, cel sprintu), a dalsze prace pozostają na poziomie ogólnym w backlogu i są doprecyzowywane na bieżąco wraz z postępem i zmianami wymagań; postęp często śledzi się przez velocity i wykres burndown.
- **Harmonogram kamieni milowych** – uproszczona forma planu, w której zamiast rozpisywać wszystkie zadania, definiuje się kluczowe punkty kontrolne (np. „zakończenie analizy”, „gotowy prototyp”, „koniec testów”, „wdrożenie”) wraz z datami i kryteriami osiągnięcia; dobrze sprawdza się do raportowania dla interesariuszy i kontroli, czy projekt „dowodzi” kolejne rezultaty w terminie.