
# Generowanie realistycznych obrazów scen 3-D za pomocą metody śledzenia promieni.

## 1. Generowanie obrazu (Wprowadzenie)

Ray Tracing to technika z rodziny **Global Illumination** (Oświetlenie Globalne). W przeciwieństwie do lokalnych modeli (jak w starej rasteryzacji), tutaj obiekty "wiedzą o sobie" nawzajem (odbicia, cienie rzucane przez inne obiekty).

**Kluczowa różnica:**
- **Rasteryzacja (Klasyczna):** Rzutujemy trójkąty na ekran (Object-order). Szybka, ale słaba w odbiciach.
- **Ray Tracing:** Dla każdego piksela szukamy obiektu w świecie (Image-order). Wolny, ale fotorealistyczny.

---

## 2. Algorytm Śledzenia Promieni (Krok po kroku)
Proces dla pojedynczego piksela $(x,y)$:
1. **Generacja promienia:** Tworzysz wektor $D$ od oka przez punkt na ekranie $(x,y)$.
2. **Szukanie przecięcia (Intersection):**
    - Testujesz promień z _każdym_ obiektem na scenie (z użyciem struktur przyspieszających).
    - Wybierasz ten obiekt, który jest **najbliżej** (najmniejsze $t > 0$).
3. **Obliczenie koloru (Shading):**
    - W punkcie trafienia liczysz tzw. **Model Oświetlenia Lokalnego** (np. Phonga) – patrz punkt 3.
4. **Rekurencja (Promienie wtórne):**
    - Jeśli obiekt jest lustrzany -> generujesz **promień odbity** ($R$).
    - Jeśli przezroczysty -> generujesz **promień załamany** ($T$).
    - Generujesz **promienie cienia** (Shadow Rays) do każdego źródła światła, by sprawdzić, czy punkt jest widoczny dla światła.
5. **Stop:** Gdy osiągniesz limit odbić (np. 5) lub waga promienia spadnie prawie do zera.

---
## 3. Model Oświetlenia Phonga (Phong Lighting Model)
Służy do obliczenia koloru punktu na powierzchni, uwzględniając materiał i światło. Składa się z sumy trzech wektorów/komponentów:
I=Iambient+Idiffuse+IspecularI
1. **Ambient (Otoczenie):** Stały kolor, "rozjaśnia cienie", symuluje światło rozproszone wielokrotnie odbite, którego nie liczymy dokładnie.
    - _Wzór:_ $k_a \cdot i_a$
2. **Diffuse (Rozproszenie - Lamberta):** Światło matowe. Zależy od kąta padania światła. Powierzchnia jest najjaśniejsza, gdy światło pada prostopadle.
    - _Wzór:_ $k_d \cdot i_d \cdot (N \cdot L)$ (gdzie $N$ to normalna, $L$ to wektor do światła, a $\cdot$ to iloczyn skalarny, czyli cosinus kąta).
3. **Specular (Odblask lustrzany):** Biała plamka światła na błyszczącej kuli. Zależy od kąta obserwacji (widzisz odblask tylko gdy patrzysz pod odpowiednim kątem).
    - _Wzór:_ $k_s \cdot i_s \cdot (R \cdot V)^n$ (gdzie $R$ to wektor odbity, $V$ to wektor do oka, a $n$ to "shininess" - im większe, tym mniejsza i ostrzejsza plamka).[](http://mchwesiuk.pl/wp-content/uploads/2019/04/Grafika-Komputerowa-W5.pdf)​

---

## 4. Metody Cieniowania (Interpolacja) – Gouraud vs Phong
Częste pytanie o różnicę między modelem oświetlenia a metodą cieniowania (interpolacji):[](https://wp.faculty.wmi.amu.edu.pl/GRKW4.pdf)​
- **Cieniowanie Gourauda (Interpolacja koloru):**
    - Liczysz model oświetlenia (wzór Phonga) **tylko w wierzchołkach** trójkąta.
    - Kolor wnętrza trójkąta jest uśredniany (interpolowany liniowo).
    - _Zaleta:_ Szybkie.
    - _Wada:_ "Gubi" odblaski (specular) wewnątrz trójkąta. Wygląda kanciasto (efekt pasm Macha).
- **Cieniowanie Phonga (Interpolacja wektora normalnego):**
    - Interpolujesz **wektor normalny** dla każdego piksela wewnątrz trójkąta.
    - Liczysz model oświetlenia (wzór Phonga) **dla każdego piksela osobno**.
    - _Zaleta:_ Idealnie gładkie odblaski.
    - _Wada:_ Wolniejsze (ale w dobie GPU to standard).

---

## 5. Zaawansowane pojęcia (BRDF i Sampling)
- **BRDF (Dwukierunkowa Funkcja Rozkładu Odbicia):**​
    - To matematyczny opis materiału. Funkcja $f(L, V)$, która mówi: "jeśli światło pada z kierunku $L$, to ile procent energii odbije się w kierunku oka $V$?".
    - Dla lustra BRDF to "szpilka" (odbija tylko w jednym kierunku). Dla kartki papieru to półkula (rozprasza wszędzie).
- **Super-sampling / Antyaliasing:**
    - Problem: "Schodki" na krawędziach (aliasing).
    - Rozwiązanie: Zamiast jednego promienia przez środek piksela, wypuszczasz np. 4 lub 16 losowych promieni w obrębie piksela i uśredniasz wynik (Twoje notatki wspominają o "metodzie próbkowania przestrzeni").

## 6. Porównanie Ray Tracing vs Radiosity (Metoda Energetyczna)

| Cecha         | Ray Tracing                               | Radiosity (Metoda Energetyczna)                                                |
| ------------- | ----------------------------------------- | ------------------------------------------------------------------------------ |
| **Metoda**    | Śledzenie promieni (punktowe)             | Podział na płaty (meshing) i bilans energii                                    |
| **Efekty**    | Ostre cienie, lustra, szkło               | Miękkie cienie, Color Bleeding (czerwona podłoga barwi białą ścianę)           |
| **Zależność** | Zależy od pozycji kamery (View-dependent) | Niezależne od kamery (View-independent) - raz policzone działa z każdej strony |
| **Koszt**     | Rośnie z rozdzielczością ekranu           | Rośnie ze stopniem komplikacji geometrii                                       |
