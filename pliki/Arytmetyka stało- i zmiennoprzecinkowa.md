1. Tu już było grubo. Pytania
bardzo szczegółowe. Dlaczego
nie używamy tylko systemu ZM?
Czym się różni U1 od U2 i jak
przykładowo zapisujemy liczby.
Jakie znam błędy i problemy w
systemach. W
zmiennoprzecinkowych użyłem
słowa IEEE 754 i zapytali jakie są
jeszcze inne standardy. Czym
właściwie jest wykładnik i za do
odpowiada + coś o mantysie.
Dlaczego wykładnik może być
ujemny?. Czym się różni quiet
NaN od signaled NaN? Jakie są
problemy w
zmiennoprzecinkowych?

2. Mówiłem z opracowania z
dysku.
Pytania: jakie są konkretne kody
stało przecinkowe (U2, naturalny,
znak moduł ... itp.)?
jakie są szczególne przypadki w
ieee754 (zero, NaN,
nieskończoność ... itp.)?
jakie są wady i zalety danych
kodów/jakie są problemy z
kodami/jak te kody działają (np.
czy zakres U2 jest symetryczny -
w sensie podział na ujemne i
dodatnie)?
co to jest U1 jakie ma wady i
zalety?
czy dzielenie w asemblerze na
liczbach całkowitych jest
przemienne/jakie takie dzielenie
ma wady/jakie tam są problemy
(np. te flagi co tam CPU wystawia
trzeba było podać konkretnie
chyba)?
jak jest sygnalizowany błąd w

stałym przecinku (chodziło o
overflow)?
jak jest z zakresem operandów a
wyniku w stałym przecinku (jakie
muszą być żeby operacja była
poprawną)?

3. Dla kierunkowego wystarczyło
powiedzieć to, co jest w
opracowaniach + dostałem
pytanie czym się różnią, kiedy się
jaki wykorzystuje i jak różnią się
pod względem dokładności + czy
można opisać w formacie
zmienno przecikowym coś innego
niż liczbę (chodziło o NaN i
nieskończoność)

4. Arytmetyka z opracowania, kazał

coś powiedzieć o zmienno-
przecinkowych i o tym jakie

można zapisać za ich pomocą
wartości specjalne.

5. K2 - nie zdążyłem praktycznie nic
powiedzieć sam tylko
odpowiadałem na zadane pytania,
dostałem pytanie co rozumiem
przez zakres, następnie zapytano
mnie o dokładność stałego i
zmiennego przecinka i na końcu
było pytanie o wartości specjalne
w ieee 754

# Opracowanie z dysku
# Arytmetyka stało- i zmiennoprzecinkowa.
Arytmetyka stałoprzecinkowa (Fixed-point) i zmiennoprzecinkowa (Floating-point) to dwa sposoby reprezentowania ułamków w systemach komputerowych. Różnią się miejscem "przecinka" (separatora dziesiętnego):
- **Stałoprzecinkowa:** Przecinek jest ustawiony na stałe. Gwarantuje stałą precyzję (dokładność), ale ma mały zakres.
- **Zmiennoprzecinkowa:** Przecinek "pływa" (jest dynamiczny). Pozwala zapisać liczby bardzo małe i bardzo duże, ale kosztem precyzji.​

---

## 1. Arytmetyka Stałoprzecinkowa (Fixed-Point)
- **Jak działa:** Liczba jest przechowywana jako liczba całkowita (np. `long`), a my umawiamy się, że np. ostatnie 2 cyfry to część ułamkowa (czyli dzielimy przez 100).
    - _Przykład:_ Kwota 12,50 zł jest zapisana w bazie jako `1250` (groszy).
- **Zalety:**
    - **Determinizm:** 1.1 + 2.2 zawsze da 3.3. Brak błędów zaokrągleń wynikających z konwersji binarnych (dla operacji dziesiętnych).
    - **Wydajność:** Operacje są realizowane przez CPU tak szybko jak na zwykłych intach (szczególnie ważne w systemach embedded bez jednostki FPU).​
- **Wady:** Mały zakres liczb (szybko następuje przepełnienie - overflow).

## 2. Arytmetyka Zmiennoprzecinkowa (Floating-Point)
- **Standard:** IEEE 754 (najważniejszy standard w informatyce).
- **Budowa:** Liczba składa się z 3 części: `Znak` | `Wykładnik (Cecha)` | `Mantysa (Ułamek)`.
    - Wzór: $(-1)^S \times 2^{Wykładnik-Bias} \times Mantysa$.
- **Problem precyzji:**
    - Wiele liczb dziesiętnych (np. 0.1) ma **nieskończone rozwinięcie binarne** (tak jak 1/3 w systemie dziesiętnym to 0.333...). Komputer musi uciąć ten ciąg, co wprowadza błąd.
    - _Słynny przykład:_ `0.1 + 0.2 != 0.3` (w Pythonie/Javie/JS wynik to `0.30000000000000004`).
- **Typy danych (Java):**
    - `float` (32-bit, precyzja ~7 cyfr znaczących).
    - `double` (64-bit, precyzja ~15 cyfr znaczących).

## 3. Porównanie i Pułapki

| Cecha                  | Stałoprzecinkowa (`BigDecimal`, `int` skalowany) | Zmiennoprzecinkowa (`float`, `double`)             |
| ---------------------- | ------------------------------------------------ | -------------------------------------------------- |
| **Zastosowanie**       | Finanse, waluty, liczniki precyzyjne.            | Grafika 3D, fizyka, statystyka, nauka (AI).        |
| **Szybkość**           | Wolniejsza (w Javie `BigDecimal` jest obiektem). | Bardzo szybka (wspierana sprzętowo przez FPU/GPU). |
| **Błędy**              | Brak (w zakresie reprezentowalnym).              | Błędy zaokrągleń (akumulują się!).                 |
| **Łączność dodawania** | $(A+B)+C = A+(B+C)$ (ZACHOWANA)                  | $(A+B)+C \neq A+(B+C)$ (NIE ZAWSZE!)​              |

1. **Dlaczego dodawanie `float` nie jest łączne?**
    - Bo przy dodawaniu małej liczby do bardzo dużej, ta mała może "zniknąć" (underflow) z powodu wyrównywania wykładników. Jeśli dodamy `(Mała + Mała) + Duża`, wynik może być inny niż `Mała + (Mała + Duża)`.​
2. **Jak porównywać liczby `double` w kodzie?**    
    - _Nigdy:_ `if (a == b)`
    - _Zawsze:_ `if (Math.abs(a - b) < EPSILON)`, gdzie Epsilon to bardzo mała wartość (np. $10^{-9}$)

## Operacje Zmiennoprzecinkowe (Standard IEEE 754)
Wszystkie operacje odbywają się w 4 krokach:
1. **Rozpakowanie:** Wyodrębnienie znaku, cechy (wykładnika) i mantysy (z dodaną ukrytą jedynką `1.xxxxx`).
2. **Operacja właściwa:** Na mantysach.
3. **Normalizacja:** Przesunięcie mantysy tak, by znów zaczynała się od `1,...` i korekta wykładnika.
4. **Zaokrąglenie:** Dopasowanie do dostępnej liczby bitów.

---
## 1. Dodawanie (i Odejmowanie)
Kluczowa zasada: **Nie można dodawać mantys, jeśli wykładniki są różne.** To tak jak dodawanie $1.5 \cdot 10^2 + 2.0 \cdot 10^4$. Musisz sprowadzić je do wspólnego wykładnika (większego).
**Przykład:** `2.5 + 0.125` (dziesiętnie)
1. **Reprezentacja binarna:**
    - $A = 2.5 = 10.1_{(2)} = 1.01 \times 2^1$ (Wykładnik: 1)
    - $B = 0.125 = 0.001_{(2)} = 1.00 \times 2^{-3}$ (Wykładnik: -3)
2. **Wyrównanie cech (do większej, czyli 1):**
    - Przesuwamy przecinek w $B$ w lewo o 4 pozycje ($1 - (-3) = 4$).
    - $B' = 0.0001 \times 2^1$
3. **Dodawanie mantys:**
    - $1.0100$ (mantysa A)
    - $+ 0.0001$ (przesunięta mantysa B)
    - $= 1.0101$
4. **Wynik:** $1.0101 \times 2^1 = 10.101_{(2)} = 2 + 0.5 + 0.125 = 2.625$.
5. **Pułapka:** Jeśli przesunięcie w kroku 2 jest większe niż liczba bitów mantysy (23 bity dla `float`), mniejsza liczba staje się zerem. To tzw. **utrata precyzji**.

---

## 2. Mnożenie
Jest prostsze niż dodawanie, bo nie wymaga wyrównywania wykładników.  
Zasada: **Mnożymy mantysy, dodajemy wykładniki.**
**Przykład:** `3.0 * 6.0`
1. **Reprezentacja:**
    - $A = 3.0 = 1.1 \times 2^1$
    - $B = 6.0 = 1.1 \times 2^2$
2. **Działania:**
    - **Wykładniki:** $1 + 2 = 3$ (w rzeczywistości dodajemy też tzw. _bias_ 127).
    - **Mantysy:** $1.1 \times 1.1 = 1.001_{(2)}$ (czyli $1.5 \times 1.5 = 2.25$ dziesiętnie).
3. **Normalizacja:**
    - Wynik mantysy $10.01$ (musimy mieć format `1.xxx`).
    - Przesuwamy przecinek w lewo: $1.001 \times 2^1$.
    - Zwiększamy wykładnik o 1: $3 + 1 = 4$.
4. **Wynik:** $1.001 \times 2^4 = 1.001 \times 16 = 9 \times 2 = 18.0$ (poprawnie).

---
## 3. Dzielenie
Zasada odwrotna do mnożenia: **Dzielimy mantysy, odejmujemy wykładniki.**
1. Podziel mantysy ($M_A / M_B$).
2. Odejmij wykładniki ($W_A - W_B$).
3. Znormalizuj (jeśli wynik mantysy $< 1$, przesuń w prawo i zmniejsz wykładnik).