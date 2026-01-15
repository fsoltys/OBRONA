# Model warstwowy TCP/IP.
## 1. Definicja i Architektura

Model TCP/IP (Transmission Control Protocol / Internet Protocol) to **czterowarstwowy** model odniesienia, który jest standardem przemysłowym dla Internetu. W przeciwieństwie do modelu ISO/OSI (który jest modelem _koncepcyjnym_), TCP/IP jest modelem _implementacyjnym_. Sam model jak i zestaw protokołów TCP/IP został zaprojektowany w otwartej architekturze i nie jest ograniczony żadnymi patentami ani prawami autorskimi.

**Kluczowa cecha inżynierska:** Niezależność od medium transmisyjnego (może działać na światłowodzie, miedzi, falach radiowych) oraz niezależność od sprzętu.

---
## 2. Szczegółowy opis warstw (od góry)

## **I. Warstwa Aplikacji (Application Layer)**
Odpowiada za interfejs między użytkownikiem a siecią oraz kodowanie danych. W modelu TCP/IP łączy ona funkcje trzech górnych warstw modelu OSI (Aplikacji, Prezentacji, Sesji).
- **Zadania szczegółowe:**
    - Negocjacja formatu danych (np. MIME types w HTTP).
    - Zarządzanie sesją (ustanawianie, utrzymanie, zrywanie logicznego połączenia).
    - Szyfrowanie/Deszyfrowanie (np. TLS/SSL w HTTPS).
- **Protokoły i porty (Well-known ports):**
    - **SSH (22):** Szyfrowana sesja terminalowa.
    - **DNS (53):** Rozwiązywanie nazw domenowych (UDP dla zapytań, TCP dla transferu stref).
    - **SMTP ( )**: 
    - **HTTP/HTTPS (80/443):** Transfer hipertekstu.
- **Jednostka danych:** Dane (Data) / Strumień (Stream).

## **II. Warstwa Transportowa (Transport Layer)**
Warstwa zapewniająca kanał komunikacyjny między poszczególnymi aplikacjami na komunikujących się ze sobą urządzeniach, dzieląca dane na segmenty po stronie nadawczej i składająca po stronie odbiorczej, kierująca do każdej z aplikacji ruch sieciowy na podstawie przypisanego jej numeru portu. Analogicznie w modelu OSI istnieje warstwa transportowa z takimi samymi zadaniami.
- **Kluczowe mechanizmy:**
    - **Multipleksacja:** Użycie **numerów portów** (16-bitowych, 0-65535) do rozróżniania usług uruchomionych na tym samym IP.
    - **Kontrola przepływu (Flow Control):** Mechanizm **Sliding Window** (Okno przesuwne) w TCP – odbiorca informuje nadawcę, ile danych może jeszcze przyjąć (pole `Window Size` w nagłówku), by nie przepełnić bufora.
    - **Kontrola zatorów (Congestion Control):** Algorytmy (np. TCP Reno, CUBIC) zapobiegające zapchaniu routerów po drodze (mechanizm Slow Start).
- **Protokoły:**
    1. **TCP (Transmission Control Protocol):**
        - Połączeniowy (3-way handshake: SYN -> SYN-ACK -> ACK).
        - Niezawodny (Potwierdzenia ACK, retransmisje po timeoutcie).
        - Uporządkowany (Sequence Numbers w nagłówku pozwalają złożyć pakiety w dobrej kolejności).
    2. **UDP (User Datagram Protocol):**
        - Bezpoczątkowy (Connectionless), brak gwarancji dostarczenia, minimalny narzut (nagłówek ma tylko 8 bajtów).
- **Jednostka danych:** Segment (TCP) / Datagram (UDP).

## **III. Warstwa Internetowa (Internet Layer)**
Odpowiada za logiczne adresowanie (IP) i routing (wybór trasy) pakietów przez różne sieci (Inter networking). Działa w trybie "best-effort" (nie gwarantuje dostarczenia – to rola wyższej warstwy TCP). Stanowi odpowiednik warstwy sieciowej modelu OSI.
- **Protokoły:**
    - **IP (IPv4 / IPv6):**
        - **IPv4:** Adres 32-bitowy. Nagłówek zawiera m.in. `TTL` (Time To Live) – licznik, który zmniejsza się o 1 na każdym routerze. Gdy osiągnie 0, pakiet jest odrzucany (zapobiega pętlom w routingu).
        - **Fragmentacja:** Jeśli pakiet jest większy niż MTU (Maximum Transmission Unit) łącza, router dzieli go na mniejsze kawałki (fragmenty).
    - **ICMP (Internet Control Message Protocol):** Protokół diagnostyczny (używany przez `ping` i `traceroute`). Zgłasza błędy (np. "Destination Unreachable", "TTL Exceeded").
    - **ARP (Address Resolution Protocol):** Mapuje logiczny adres IP na fizyczny adres MAC w sieci lokalnej (działa na granicy warstwy 2 i 3).
- **Jednostka danych:** Pakiet (Packet).

## **IV. Warstwa Dostępu do Sieci (Network Access Layer)**
Najniższa warstwa, odpowiadająca za fizyczne przesłanie bitów do medium i sterowanie dostępem do tego medium (MAC - Media Access Control). W modelu OSI odpowiada warstwom Łącza Danych i Fizycznej.
- **Zadania:**
    - Adresowanie fizyczne (Adres MAC – 48-bitowy).
    - Detekcja błędów w ramce (suma kontrolna CRC/FCS na końcu ramki).
    - Framing (oznaczanie początku i końca ramki).
- **Technologie:** Ethernet (IEEE 802.3), Wi-Fi (IEEE 802.11).
- **Jednostka danych:** Ramka (Frame).

Każda z warstw jest niezależna od pozostałych. Przykładowo, na poziomie warstwy aplikacji po drugiej stronie łącza widoczna jest bezpośrednio aplikacja, z którą zostało nawiązane połączenie, niezależnie od trasy pokonywanej przez pakiety w warstwie internetowej. Z kolei z perspektywy warstwy internetowej po przeciwnej stronie łącza znajduje się następne urządzenie na trasie posiadające adres IP, niezależnie od fizycznej struktury sieci i przykładowo liczby przełączników, jakie musi pokonać ramka przenosząca pakiet IP. Dzięki temu można łatwo wykorzystać w sieci różne technologie i z punktu widzenia wyższych warstw nie ma na przykład znaczenia, czy fizycznie transmisja odbywa się kablem w standardzie Ethernet, czy bezprzewodowo w jednym ze standardów Wi-Fi – dowolny rodzaj danych da się przesłać dowolnym łączem.

---
## 3. Proces Enkapsulacji (Szczegóły nagłówków)
1. **Dane:** `[HTTP Request]`
2. **Segment TCP:** `[Port Źródłowy | Port Docelowy | Sekwencja | ... ] + [Dane]`
3. **Pakiet IP:** `[IP Źródłowe | IP Docelowe | TTL | Protokół (TCP) ] + [Segment]`
4. **Ramka Ethernet:** `[MAC Docelowy | MAC Źródłowy | EtherType (IP)] + [Pakiet] + [Suma CRC]`

**Ważny szczegół:** Routery pośredniczące w Internecie "rozpakowują" dane tylko do warstwy 3 (IP), modyfikują TTL, przeliczają sumę kontrolną nagłówka IP i pakują w nową ramkę (z nowym adresem MAC kolejnego routera). Warstwa Transportowa (4) jest analizowana dopiero u odbiorcy (End-to-End).

---

## 4. Porównanie TCP/IP vs OSI (Tabela techniczna)

|Warstwa modelu OSI (7)|Odpowiednik w TCP/IP (4)|Funkcja i protokoły|
|---|---|---|
|**7. Aplikacji**|**Aplikacji**|Procesy sieciowe dla aplikacji (HTTP, FTP, SMTP)|
|**6. Prezentacji**|_(włączona w Aplikacji)_|Formatowanie danych, szyfrowanie (SSL/TLS), kompresja|
|**5. Sesji**|_(włączona w Aplikacji)_|Zarządzanie sesjami między aplikacjami (RPC)|
|**4. Transportowa**|**Transportowa**|Host-to-Host, niezawodność (TCP, UDP, SCTP)|
|**3. Sieciowa**|**Internetowa**|Adresowanie logiczne i routing (IP, ICMP, IPSec, IGMP)|
|**2. Łącza Danych**|**Dostępu do sieci**|Adresowanie fizyczne (MAC), obsługa błędów ramki (Ethernet, PPP)|
|**1. Fizyczna**|**Dostępu do sieci**|Sygnały elektryczne/optyczne, okablowanie, złącza|

1. **Na czym polega MTU i MSS?**
    - **MTU (Maximum Transmission Unit):** Maksymalny rozmiar ramki (zazwyczaj 1500 bajtów dla Ethernetu).
    - **MSS (Maximum Segment Size):** Maksymalna ilość danych w segmencie TCP (MTU minus nagłówki IP i TCP). Negocjowane podczas 3-way handshake.
2. **Jaka jest różnica między switchem (L2) a routerem (L3)?**
    - Switch (przełącznik) działa na adresach MAC (warstwa Dostępu do sieci) i przesyła ramki tylko w obrębie jednej sieci lokalnej.
    - Router działa na adresach IP (warstwa Internetowa) i przenosi pakiety _pomiędzy_ różnymi sieciami.
# opracowanie
Pierwszy z nich, model **TCP/IP** określany jest jako **model protokołów.** Każda z jego warstw wykonuje konkretne zadania, do realizacji który wykorzystywane są konkretne protokoły. Model **ISO/OSI** natomiast zwany **modelem odniesienia**, stosowany jest raczej do analizy, która pozwala lepiej zrozumieć procesy komunikacyjne zachodzące w sieci oraz stanowi wzór do projektowania rozwiązań sieciowych zarówno sprzętowych jak i programowych.


## tcp/ip
W przypadku modelu TCP/IP wyróżnić możemy **4 warstwy**, są nimi warstwa: 
**aplikacji**
**transportu**
**internetowa** 
**dostępu do sieci**.

**Warstwa aplikacji** udostępnia użytkownikom możliwość korzystania z usług sieciowych, takich jak WWW, poczta elektroniczna, wymiana plików, połączenia terminalowe czy komunikatory. Swoim uczniom mówię zawsze, że jest to warstwa najbliższa użytkownikowi, ponieważ to właśnie ona pozwala nam w pełni korzystać z dobrodziejstw współczesnych usług sieciowych. Kiedy siadamy przed komputerem i uruchamiamy np. przeglądarkę internetową to korzystamy z sieci właśnie na poziomie warstwy aplikacji.

Niżej mamy **warstwę transportu**, której głównym zadaniem jest sprawna obsługa komunikacji pomiędzy urządzeniami. W warstwie tej dane dzielone są na mniejsze części, następnie opatrywane są dodatkowymi informacjami pozwalającymi zarówno przydzielić je do właściwej aplikacji na urządzeniu docelowym, jak i pozwalającymi złożyć je na urządzeniu docelowym w odpowiedniej kolejności.

Dalej jest **warstwa internetowa**, której głównym zadaniem jest odnalezienie najkrótszej i najszybszej drogi do urządzenia docelowego przez sieć rozległą, podobnie jak robią to samochodowe GPS’y, ale także adresowanie danych z wykorzystaniem adresów logicznych (adresów IP).

No i na koniec mamy warstwę **dostępu do sieci**, która koduje dane do postaci czystych bitów (zer i jedynek) i przekazuje je do medium transmisyjnego i także je adresuje, tym razem poprzez adresy fizyczne (adresy MAC).

## iso/osi
Na samej górze tego modelu wyróżnić możemy **warstwę aplikacji** i tutaj tak naprawdę jej funkcję są bardzo podobne do tych z modelu TCP/IP, no bo pozwalają użytkownikom końcowym sieci korzystać z aplikacji sieciowych.

Dalej mamy **warstwę prezentacji**, która to przekazuje do warstwy aplikacji informacje o zastosowanym formacie danych, np. informuje jakie typy plików będą przesyłane, odpowiada ona również za odpowiednie zakodowanie danych na urządzeniu źródłowym i ich dekodowanie na urządzeniu docelowym.

Niżej jest **warstwa sesji**, zarządzająca sesjami użytkowników korzystających np. ze stron WWW czy komunikacji video.

Idąc dalej mamy warstwę **transportu**, czyli ponownie dokładnie to samo co w modelu TCP/IP, zarówno w jednym jak i w drugim przypadku funkcje tej warstwy są dokładnie takie same.

Następnie mamy **warstwę sieci**, która jest odpowiednikiem warstwy internetowej modelu TCP/IP czyli znowu bardzo podobne funkcje, takie jak adresowanie i wyznaczanie najlepszej ścieżki przesyłu danych.

Dalej idąc w dół mamy **warstwę łącza danych**, której głównym zadaniem jest kontrola dostępu do medium transmisyjnego, a także adresowanie danych, tym razem jednak w celu przesyłania ich pomiędzy hostami w sieci LAN.

No i na koniec **warstwa fizyczna**, która koduje dane do postaci czystych bitów (zer i jedynek) i przesyła je poprzez medium transmisyjne do odpowiednich urządzeń.

Oba modele są do siebie dość podobne. Różnice jakie występują widoczne są w **górnych warstwach** gdzie w przypadku modelu ISO/OSI dokonano podziału, aż na 3 warstwy, a w przypadku modelu TCP/IP te same funkcje realizowane jest tylko poprzez jedną warstwę. Podobne różnice widać w **dolnych warstwach**, gdzie w modelu ISO/OSI mamy dwie oddzielne warstwy łącza danych i fizyczną, a w przypadku modelu TCP/IP tylko jedną, warstwę dostępu do sieci.


![[Zrzut ekranu 2026-01-7 o 13.15.43.png]]