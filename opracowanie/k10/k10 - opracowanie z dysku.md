# Fizyczne nośniki danych – stosowane technologie, struktury oraz metody kodowania informacji
## 1. Nośniki Magnetyczne (HDD - Hard Disk Drive)
Technologia najstarsza, ale wciąż używana (Big Data, backup) ze względu na cenę za terabajt.
## **A. Struktura fizyczna**
- **Talerz (Platter):** Aluminiowy lub szklany krążek pokryty warstwą ferromagnetyczną (tlenki żelaza/chromu).
- **Głowica (R/W Head):** Unosi się na poduszce powietrznej (efekt aerodynamiczny) nad wirującym talerzem (w odległości kilku nanometrów!). Nie dotyka powierzchni (kontakt = awaria _Head Crash_).
- **Zapis:** Prąd w cewce głowicy generuje pole magnetyczne, które ustawia dipole magnetyczne na powierzchni (zmienia polaryzację N-S lub S-N).​

## **B. Struktura logiczna (Geometria CHS)**
Adresowanie historyczne to **CHS (Cylinder-Head-Sector)**, obecnie zastąpione przez logiczne adresowanie **LBA (Logical Block Addressing)**.
1. **Ścieżka (Track):** Koncentryczne okręgi na talerzu.
2. **Sektor:** Najmniejsza jednostka zapisu (fizycznie 512 bajtów lub nowsze 4096 bajtów - 4Kn).
3. **Cylinder:** Zbiór ścieżek o tym samym numerze na wszystkich talerzach (wszystkie głowice są na tym samym cylindrze jednocześnie).

## **C. Metody kodowania**
Dane binarne nie są zapisywane wprost (jako 0 i 1), ale jako zmiany polaryzacji (flux transitions).
1. **FM (Frequency Modulation):** Metoda archaiczna. Każdy bit ma sygnał zegarowy. Mała gęstość (1 bit = 2 przemagnesowania).
2. **MFM (Modified FM):** Eliminacja nadmiarowych sygnałów zegarowych. Podwoiła gęstość zapisu względem FM. Stosowana w dyskietkach i starych HDD.​
3. **RLL (Run Length Limited):** Współczesny standard. Pozwala na kodowanie grupy bitów (np. 2,7) w ciąg zmian pola magnetycznego tak, by unikać zbyt gęstych lub zbyt rzadkich zmian. Zwiększa pojemność o 50%+ względem MFM.​

---
## 2. Nośniki Półprzewodnikowe (SSD - Solid State Drive)
Brak części ruchomych. Pamięć trwała oparta na pułapkowaniu elektronów.
## **A. Struktura komórki pamięci (Flash NAND)**
- Podstawą jest tranzystor **FGMOS (Floating Gate MOSFET)**.
- **Bramka pływająca (Floating Gate):** Izolowana elektrycznie warstwa, w której "więzimy" elektrony.
    - _Naładowana bramka_ = logiczne "0" (elektrony blokują przepływ prądu w tranzystorze).
    - _Pusta bramka_ = logiczne "1".
    - _Kasowanie:_ Wymaga wysokiego napięcia (Tunelowanie Fowlera-Nordheima), by wyrzucić elektrony z bramki. Dlatego zapis jest wolniejszy niż odczyt, a komórki się zużywają (izolator pęka).​

## **B. Rodzaje komórek (Gęstość zapisu)**

- **SLC (Single Level Cell):** 1 bit na komórkę (stan: naładowana/pusta). Najszybsza, najtrwalsza (100k cykli), najdroższa (serwery).
- **MLC (Multi):** 2 bity (4 poziomy naładowania).
- **TLC (Triple):** 3 bity (8 poziomów napięcia). Standard w domowych SSD.
- **QLC (Quad):** 4 bity (16 poziomów). Tanie, ale wolne i mało trwałe.​

## **C. Architektura NAND vs NOR**
- **NAND:** Komórki połączone szeregowo. Dostęp do danych stronami (Pages). Świetne do zapisu dużych plików (dlatego jest w SSD i pendrive'ach).
- **NOR:** Komórki równolegle. Dostęp losowy do każdego bajtu (jak w RAM), ale mała gęstość. Używana w BIOS/Firmware routerów.[](https://students.mimuw.edu.pl/ZSO/Wyklady/00_stud/flash/flash.html)​

---

## 3. Nośniki Optyczne (CD, DVD, Blu-ray)
Zapis i odczyt za pomocą wiązki lasera.
## **A. Struktura fizyczna**
- **Pity (Wgłębienia) i Landy (Pola):** Informacja jest wytłoczona na poliwęglanowym krążku.
- **Zasada odczytu:** Laser świeci na ścieżkę.
    - Odbicie od Landu -> fotodioda widzi światło.
    - Trafienie w krawędź Pitu -> światło ulega rozproszeniu i interferencji (wygaszeniu). Fotodioda widzi ciemność.
- **Logika:** "1" to **zmiana** (krawędź pit/land). "0" to brak zmiany (płaski pit lub płaski land).[](https://eduinf.waw.pl/inf/alg/002_struct/0047.php)​

## **B. Różnice w technologii**
Gęstość zależy od długości fali lasera (im krótsza fala, tym mniejszy pit można wypalić).

|Typ|Laser|Długość fali|Pojemność (1 warstwa)|Odstęp ścieżek|
|---|---|---|---|---|
|**CD**|Podczerwony|780 nm|700 MB|1.6 $\mu m$|
|**DVD**|Czerwony|650 nm|4.7 GB|0.74 $\mu m$|
|**Blu-ray**|Niebieski|405 nm|25 GB|0.32 $\mu m$|

## **C. Kodowanie kanałowe**
- **EFM (Eight-to-Fourteen Modulation) w CD:** Każde 8 bitów danych zamienia się na 14 bitów kodu.
    - _Cel:_ Zapobieganie pojawieniu się zbyt wielu jedynek obok siebie (co oznaczałoby zbyt gęste pity, niemożliwe do odczytania przez optykę) oraz zbyt długich ciągów zer (gubienie synchronizacji).
    - W EFM między dwiema jedynkami muszą być min. 2 zera i max. 10 zer.
- **EFMPlus w DVD:** Ulepszona wersja (8 na 16 bitów), bardziej efektywna.

---

## Pytania "Killer" na obronę:
1. **Dlaczego SSD zwalnia, gdy jest zapełniony?**
    - _Odp:_ Bo nie można nadpisać danych w Flash "w miejscu" (jak w HDD). Trzeba najpierw skasować cały blok (Block Erase), a potem zapisać nową stronę. Jeśli brakuje wolnych bloków, kontroler musi w tle czyścić stare (Garbage Collection), co zajmuje czas.
2. **Jaka jest różnica między interfejsem a protokołem w SSD?**
    - _Interfejs fizyczny:_ SATA, M.2, PCIe.
    - _Protokół logiczny:_ AHCI (stary, dla HDD), NVMe (nowy, równoległy, stworzony dla SSD).
3. **Co to jest Wear Leveling?**
    - Algorytm w kontrolerze SSD, który równomiernie zużywa komórki pamięci, zapisując dane w różnych miejscach, aby jedna komórka nie "umarła" szybciej niż inne.