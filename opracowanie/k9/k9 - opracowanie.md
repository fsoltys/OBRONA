Programowalne układy cyfrowe PLD (ang. programmable logical device) to rodzaj układów elektronicznych, których struktura pozwala na ustalenie funkcjonalności poprzez ich użytkownika. Są one programowane przy pomocy dedykowanych języków HDL (np. Verilog, VHDL), co pozwala na łatwą konfigurację i modyfikację ich zastosowań. Potrafią realizować proste funkcje logiczne (AND, OR, NOT, itd.), pracując:
- **Sekwencyjnie** - stan wyjść zależy od stanów wejść oraz **poprzedniego stanu układu** (pamięć, przerzutniki).
- **Kombinacyjnie** - stan wyjść zależy wyłącznie od aktualnych stanów wejść,

## Najbardziej popularne typy układów PLD:
### SPLD 
Najbardziej podstawowe układy, bazują na matrycach bramek logicznych AND i OR. W zależności od typu mogą być programowalne różne bramki
- **PAL** - programowalne AND
- **PLE** - programowalne OR
- **PLA** - programowalne AND i OR
### CPLD 
Bardziej złożone, bazują na makrokomórkach, które są łączone w bloki funkcyjne. Każda makrokomórka stanowi prostą funkcję logiczną. Makrokomórki mogą się wymieniać informacjami np. poprzez przeniesienie. Zezwalają na korzystanie z większej ilości zmiennych i tworzenie bardziej złożonych funkcji.

**Pojęcia kluczowe:**
- **Makrokomórka** – podstawowy element CPLD realizujący funkcję logiczną, często zawierający przerzutnik umożliwiający pracę sekwencyjną.
- **Blok funkcyjny** – grupa makrokomórek połączonych wspólną strukturą połączeń.
- **Przeniesienie (carry)** – mechanizm umożliwiający szybkie przekazywanie informacji między makrokomórkami, np. w operacjach arytmetycznych.

### **FPGA** 
Najbardziej złożone z układów PLD, bazujące na macierzy bloków logicznych z możliwością bezpośredniego programowania. Dodatkowo, zawierają bloki RAM, co pozwala na tworzenie tablic wartości LUT (ang look-up table). FPGA zawierają znacznie większe ilości bloków logicznych od innych typów PLD, a dodatkowo bloki IOB (ang. input-output block) również oferują możliwości programowania.

**Pojęcia kluczowe:**
- **CLB (Configurable Logic Block)** – podstawowy blok logiczny FPGA, zawierający elementy realizujące funkcje kombinacyjne i sekwencyjne.
- **LUT (Look-Up Table)** – tablica przechowująca wyniki funkcji logicznej, umożliwiająca szybkie odwzorowanie zależności wejście–wyjście.
- **Blok RAM** – wbudowana pamięć umożliwiająca przechowywanie danych lub implementację bardziej złożonych funkcji logicznych.
- **IOB (Input–Output Block)** – programowalny blok odpowiedzialny za komunikację FPGA ze światem zewnętrznym.