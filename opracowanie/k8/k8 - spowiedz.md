1. - powiedziałam bardzo krótko
czym są semafory i mutexy i
zanim zdążyłam wspomnieć o
reszcie to padło pytanie: które z
tych mechanizmów są używane w
systemach typu Unix?
2. opisałem semafory, mutexy
(semafor binarny), conditional
variable. Pytania co do sposobu
implementacji (sprzętowej i/lub
programowej, funkcje P i V), co
może pójść nie tak przy
zajmowaniu semafora
(zakleszczenie itp).
Wspomniałem że sygnały, pipe i
fifo można wykorzystać do
synchronizacji procesów - jeden
proces wysyła do drugiego
wiadomość którą on rozumie jako
“czekaj mordo” i czeka ze swoim
wykonywaniem na np. kolejną
wiadomość.
Wydaje mi się, że początkowo
komisja chciała innej interpretacji
tematu niż podałem, ale później
sami drążyli temat
3. Na czym polega wielowątkowość i
jakie problemy mogą z nią
powstać (wyścigi, zagłodzenia,
zakleszczenia) i uogólnienia do
wielu procesów.
Bełkot o mutexach, semaforach i
zmiennych warunkowych
(wszystko wrzuciłem do jednego
wora) i krótkie wspomnienie o
monitorach.
A co właściwie faktycznie dzieje
się z wątkiem który czeka w
ramach semaforu?
4. Co się dzieje z wątkiem (albo
procesem?) kiedy jest “pod
wpływem” .wait()?
Ogólnie to nie odpowiedziałem na
to pytanie tylko lałem wodę.
Powiedziałem, że sobie czeka na
zwolnienie zasobów i jest w pętli
czy coś w tym stylu. Odjęli mi za
to 0.5 oceny chyba XD
5. K8 Wyjaśnienie pojęć: system
operacyjny, proces, wątek, kiedy
pojawia się problem
synchronizacji procesów, co to
jest sekcja krytyczna i jakie niesie
zagrożenia (zakleszczenie,
zagłodzenie, wyścig),
mechanizmy synchronizacji –
kolejki komunikatów, semafory,
monitory, zmienne
synchronizujące (mutex i
warunkowa). Brak pytań od
komisji.