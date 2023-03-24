

**POLITECHNIKA POZNAŃSKA**

**Wydział Automatyki, Robotyki i**

**Elektrotechniki**

**Protokół projektowy**

**Gra “Space Ship”**

**3 czerwiec 2022**





**1. Temat projektu**

Tematem naszego projektu jest gra typu space shooter o nazwie “Space Ship”.

Gra polega na sterowaniu statkiem znajdującym się w dolnej części ekranu led

w prawo i w lewo za pomocą przycisków ‘a’ oraz ‘d’ w celu uniknięcia

spadających z jego górnej części asteroid. Gra umożliwia także wystrzeliwanie

pocisków w celu usunięcia nadciągających asteroid za pomocą przycisku ‘w’.

Punkty są naliczane za czas, przez który uda nam się przetrwać. Gdy zostajemy

trafieni asteroidą pasek zdrowia znajdujący się nad ekranem zmniejsza się. Gdy

dojdzie do 0 gra kończy się, zdobyte punkty są ukazywane na liczniku nad

paskiem życia i gdy pobijemy znajdujący się obok najwyższy wynik jest on

zamieniany na obecny. Gra kończy się oraz zaczyna odpowiednimi napisami na

ekranie. W przypadku początku gry jest to napis “Space Ship” a w razie jej

zakończenia “Game Over ”.

**2. Funkcjonalność układu**

Nasz układ umożliwia rozgrywkę za pomocą odpowiednich przycisków

klawiatury. Wyświetla on obecny stan zdrowia statku oraz wynik jaki udało nam

się osiągnąć. Pozwala również zapisać najwyższy uzyskany w grze wynik do

momentu pobicia go. Ma też oczywiście przyciski odpowiedzialne za włączenie

i reset gry oraz przycisk ‘godmode’ który wyłącza utratę zdrowia.





**3. Omówienie każdego modułu**

![image](https://user-images.githubusercontent.com/48961236/227413656-1b3b294b-0c7a-48ef-a738-1df1e6605a5b.png)

***Rys 4.1** Moduł odpowiedzialny za sprawdzenie kolizji i usuniecie strzału. (strzał2.circ)*

Od lewej wchodzą wartości strzału z rejestru. Przechodzi on przez bufor, który

kontrolowany jest zanegowaną wartością kolizji. Gdy taka się wydarzy bufor

zatrzymuje wartość strzału, który w następnym przeskoku zegara jest

podmieniany przez inną wartość. Całość wychodzi na rejestr przesuwny

odpowiedzialny za ruchy pocisków.



![image](https://user-images.githubusercontent.com/48961236/227413758-aa84f1c5-3ae3-45ce-8c81-9b8be6d491b4.png)

***Rys 4.2** Moduł odpowiedzialny za sterowanie statkiem (sterowanie.circ)*

Od lewej podawana jest informacja z bloku klawiatury jaki przycisk został

wciśnięty, sygnał ten przesyłany jest na 3 komparatory dla przycisków ‘A’, ’D’ i

‘W’. Następnie sygnał podawany jest na blok licznika w taki sposób, że przycisk

‘A’ go zwiększa a przycisk ‘D’ go zmniejsza. Rezultat tego działania

przekazywany jest na bloki ‘shifter’ ustawione powyżej które przesuwają bity

ustawionego z góry wartościami stałymi modelu statku odpowiednio w prawo

bądź w lewo. Wyjścia shifterów przechodzą na 4 pierwsze kolumny ekranu led.




![image](https://user-images.githubusercontent.com/48961236/227413790-e4d7f284-0ef5-4ce1-beeb-40ab384044a8.png)

***Rys 4.3** Moduł odpowiedzialny za wykrywanie kolizji (kolizja.circ)*

Od lewej widzimy 3 wejścia, odpowiednio od dołu jest to sygnał strzału z

kolejnej klatki, sygnał zawierający rozkład asteroid w danej linii i sygnał

zawierający obecną informacje o strzale. Obecny strzał i asteroida przesyłana

są porównywane komparatorem, osobno dla strzału i asteroidy następnie

informacja ta zostaje podłączona do MUX’a jako wartość podstawowa i

przesyłana na wyjście. Wartości strzału i asteroidy przesyłane są również na

bramkę XOR która dzięki MUX’owi przesyła na wyjście wartość, asteroidy lub

wartość asteroidy bez miejsca, w którym wystąpiła kolizja. Informacja o

asteroidach jest również porównywana z wartością strzału z następnej klatki

(co eliminuje problem mijania się bitów, gdyż kolizja jest sprawdzana dla

obecnego położenia jak i poprzedniego położenia strzału). Po wystąpieniu

kolizja wartość przetrzymywania jest jeszcze przez jeden ‘tick’ zegara (dzięki

dwóm rejestrom) by rejestry przesuwne mogły zarejestrować zmiana w

przesuwaniu bitów i pobrały wartości bez miejsc w których wystąpiła kolizja.




![image](https://user-images.githubusercontent.com/48961236/227413817-4ad7bb28-22b6-47fd-bbef-ce00061b5e80.png)

***Rys 4.4** Moduł odpowiedzialny za asteroidy i strzały na ekranie (ekran.circ)*

Od Lewej widzimy dwa wejścia 16 bitowe. Na to usytuowane wyżej przesyłana

jest informacja o strzale, która wchodzi na MUX’a, który początkowo wypuszcza

na rejestr przesuwny wartość 0, lecz gdy wykryje wciśnięcie przycisku strzału,

przesyła jeden bit z miejsca, gdzie znajduje się obecnie statek. Na wejście

poniżej wpuszczamy wartości, które zmieniają się co ‘tick‘ zegara i są to

wartości losowe, które następnie przesyłamy na rejestr przesuwny, który jest





podłączany odwrotnie niż ten od strzału (by asteroidy spadały a strzał leciał w

górę). Kolejnym wejściem znajdującym się nad rejestrem odpowiedzialnym za

asteroidy jest informacja o włączeniu gry, która uruchamia rejestry. Pod nimi

znajduje się wejście odpowiedzialne za zerowanie rejestrów. Rejestr od

asteroid zawiera maksymalną liczbę wyjść (32). 4 ostatnie odpowiadają za

linie, które zawierają statek. Ta m nigdy nie dojdzie do kolizji strzału z asteroidą

(statek nie strzela na boki) dlatego wyjścia zostały wysunięte na dół i pętla

zwrotna do rejestry by asteroidy został zachowany po wykryciu kolizji.

Informacja z asteroid i strzał przysyłana jest na moduł odpowiedzialny za

wykrywanie kolizji. Na wyjściu tego modułu odpieramy informacje o kolizji ze

strzałem (s1-s28), asteroidą, wartości wysyłane na rejestr przesuwny

odpowiedzialny za asteroidy (w1-w28) i wartości wysyłane na ekran. Kolizja z

asteroidą przesyłana jest na bramkę OR i informacja z tej bramki podłączona

jest do wejścia rejestru przesuwnego odpowiedzialnego by wartości przestały

być wpisywane z początku, lecz by można było cały rejestr zamienić na nasze

wartości tj. w2-w28. Ostatnie wejście jest zawsze zerowe by animacja była

płynna. Podobnie sytuacja wygląda dla strzału, informacja o kolizji wchodzi na

blok, który pozwala na przejście tylko tym strzałom które nie miały kolizji i

wysyłane są na rejestr.




![image](https://user-images.githubusercontent.com/48961236/227413851-818eb216-03d3-4106-9c9e-b1b8cb3387aa.png)

***Rys 4.5** Moduł odpowiedzialny za logikę gry (Game.circ)*

Po prawo znajdują się bloki wychodzące na ekran led, które wyświetlają całą

rozgrywkę. Pierwsze cztery od dołu są odpowiedzialne za generacje modelu

statku, reszta za spadające asteroidy i lecące pociski. Informacja o tym

pozyskiwana jest z modułu odpowiedzialnego za ruch strzał i asteroid. Sygnał

zawierający statek (dane wychodzące z moduł odpowiedzialnego za ruch i

generowanie statku) i strzał w 4 dolnych linach ekranu jest przesyłany na 2

bramki, w prawo na bramki OR by statek i asteroidy były dobrze wyświetlane





na ekranie i w lewo na bramki and by sprawdzić kolizje z każdej możliwej

strony. Informacja ta podłączona jest do zegara i kontrolowana jest przez bufor,

dzięki któremu możemy grać na ‘godmode’. Po każdym ‘ticku’ wartość licznika

jest zwiększana i przesyłana na shifter, który przesuwa ‘pasek zdrowa’ przy

każdej kolizji. W lewym dolnym rogu znajduje się licznik i zapisywanie

największego wyniku. Wartość licznika zwiększana jest każdorazowo, gdy gra

jest uruchomiona i przeminą napisy, poprzez ‘tick’ zegara. Informacja o

punktacji przesyłana jest na rejestr, który jest porównywany z rejestrem

zawierającym największa wartość i jeżeli wartość obecna przebija poprzedni

wynik, to nadpisujemy rejestr odpowiedzialny za ‘najwyższy wynik’. W lewej

części tego modułu znajduje się logia odpowiedzialna za wyświetlanie napisów

po rozpoczęciu i zakończeniu gry. Na bramkę AND wchodzi informacja czy gra

jest uruchomiona (przycisk wł/wył) oraz czy gra jeszcze nie trwa. Jeżeli obie te

wartości są prawdą to umożliwiamy zwiększanie wartości licznika, który

wybiera kolejne wartości w pamięci ROM zawierające napisy ‘Space ship’. Gdy

natrafi na maksymalną wartość w pamięci (‘ffff’). Informacja o tym zostaje

przesłana na przerzutnik typu D co rozpoczyna grę. Po przegranej przerzutnik

jest resetowany co pozwala na dalszą iterację pamięci zawierająca napisy

końcowe. Asteroidy powstają dzięki dwóm generatorom liczb losowych.

Pierwszy (ten niżej) może wylosować 0 lub 1, czyli wystąpienie lub

niewystąpienie asteroidy, drugi 4 bitowy ustala o jaką wartość ma być

przesunięta asteroida (w jakim miejscu ma się pojawić). Wartość ta przesyłana

jest na mux’a, który zmienia się w zależności czy na ekranie mają pojawić się

napisy czy sama gra, a z mux ’a przechodzi na moduł odpowiedzialny za kontrole

strzału i asteroid.





**4. Interfejs układu**

![image](https://user-images.githubusercontent.com/48961236/227413888-41e4cc9a-ead6-4e7b-85a1-bbe2fd491ab6.png)

***Rys 5.1** Interfejs gry*

Na powyższym rysunku znajduje się cały interfejs stworzonej przez nas gry. Jest

to kolejno: 1 - Wyświetlacz najwyższego wyniku, 2 - Wyświetlacz obecnego

wyniku, 3 – Pasek życia statku, 4 – Ekran, na którym odbywa się rozgrywka, 5 –

Klawiatura służąca do sterowania statkiem, 6 – przycisk resetujący grę, 7 –

przycisk włączający grę, 8 - przełącznik włączający/wyłączający utratę zdrowia

przez gracza (funkcja testowa).





**Możliwości rozwoju**

Udało nam się stworzyć wszystkie przewidywane funkcjonalności. Jeśli chodzi

o ewentualne rozwijanie projektu w przyszłości to można by było dodać różne

modele statków które strzelały by w inny sposób lub spadające w czasie gry

ulepszenia do broni. Można by również dodać przeciwników, którzy po

osiągnięciu przez gracza odpowiedniej ilości punktów pojawili by się na ekranie

i zaczęli do niego strzelać utrudniając tym samym rozgrywkę.

**5. Schemat logiczny**

![image](https://user-images.githubusercontent.com/48961236/227413913-b0744de5-9544-4601-b344-937a0ac9d0a7.png)

***Rys7.1** - Schemat logiczny stworzonego układu*

