# spowiedz
1. Przy pytaniu kierunkowym trzeba
opisać każdą z warstw modelu
TCP/IP. Następnie dostałem 2
pytania: czym różni się model
TCP/IP od modelu ISO/OSI oraz
poproszono mnie o wymienienie
urządzeń działających w
poszczególnych warstwach
TCP/IP.`
2. K4 - oprócz opisu poszczególnych
sieci zostałem jeszcze spytany o
drugi model (ISO/OSI) i o jego
warstwy

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