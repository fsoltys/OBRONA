1. K7: Poza typową historią o
śledzeniu promieni i modelach
oświetlenia zostałem poproszony
o wytłumaczenie czym w kodzie
są: promienie, modele, obiekty,
scena, jak wyznacza się (ze
wzoru i modelu fizycznego) kąt
promienia odbitego.
2. Na pytanie kierunkowe
wystarczyło odpowiedzieć tak, jak
było to opisane w opracowaniu
(krótszym), nie było żadnych
dodatkowych pytań.
3. K7: Powiedziane jak działa
światło normalnie i że RT imituje
to “na odwrót” od strony
obserwatora. Odbicia, załamania,
promienie wtórne, obliczanie
koloru na podstawie modelu
oświetlenia. Na koniec że metoda

jest ciężka obliczeniowo i że real-
time RT zazwyczaj hybrydowo,

czyli rastrowa + RT do
oświetlenia i cieni. Pytań brak.
4. K7. Czym jest raytracing, że
promienie z ekranu dla kazdego
piksela a nie ze swiatla. Kolejne
kroki przy puszczaniu promieni z
ekranu; Problemy jakie powoduje
raytracing: aliasing, duza
zlozonosc obliczeniowa, nie
wszystkie kąty brane pod uwagę.
Dodatkowo opowiedzialem o
mozliwosci przyspieszen i SI
(bounding, ograniczanie promieni,
real-time denoisery)

## opracowanko z dysku
7.    Generowanie realistycznych obrazów scen 3-D za pomocą metody śledzenia promieni.
Ray tracing to metoda renderowania obrazów cyfrowych, która symuluje fizyczne zachowanie światła
Problemy związane z symulowaniem realistycznych obrazów:
 Promień światła, zanim dotrze do oka obserwatora, może po drodze odbijać się od wielu obiektów, zmieniając przy tym kierunek
Obiekty wykonane z różnych materiałów, które mogą pochłaniać fale światła o określonej długości, a inne odbijać
Przebieg renderowania:
Symulujemy zachowanie promienia światła na scenie trójwymiarowej składającej się z różnych obiektów
Śledzenie zaczynamy od oka obserwatora, generując promień skierowany w głąb sceny
Jeśli promień:
Natrafi na obiekt:
Możemy obliczyć kolor promienia, korzystając z modelu oświetlenia (np. model Phonga)
Możemy śledzić kolejne promienie: np. wtórny, odbity, załamany (przezroczystość) i pochłonięty (zazwyczaj rekurencyjnie)
Nie trafi na obiekt:
Przyjmujemy pewien ustalony kolor takiego promienia, zazwyczaj kolor tła
Wysoki koszt obliczeniowy - dla każdego piksela wynikowego obrazu należy przeprowadzić całą procedurę
Obliczenia można przyspieszyć przez: kalkulacje równolegle, podział 
