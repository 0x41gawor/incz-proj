
# Monitorowanie bezpieczeństwa osób starszych
wstęp
przegląd literatury
persona
opis techniczny
schemat
algorytm działania
testy
wnioski (weryfikacja poprawność decyzji)

## 1 Wstęp
Definiuje problem, zawiera opis persony.
### 1.1 Persona
Projekt dotyczy domów spokojnej starości, domów seniora. 
Z jednej strony mamy pacjentów - osoby starsze, które zmagają się z różnymi chorobami, wymagają opieki zdrowotnej, nieustannie należy monitorować ich stan zdrowia. 
Z drugiej strony mamy personel, który sprawuje opiekę nad pacjentami, monitoruje ich stan zdrowia.

### 1.2 Problem
Dokonywanie manualnie pomiarów pulsu oraz pilnowanie lokalizacji kilkudziesięciu osób starszych w domach opieki dla kilkuosobowego personelu jest niewydajne, drogie i niebezpieczne dla pacjentów.
###  1.3 Rozwiązanie problemu
To co rozwiązałoby problem to odciążenie personelu od wykonywania nieustannych pomiarów parametrów zdrowia pacjentów. Do tej pory personel osobiście odszukiwał pacjenta i dokonywał pomiaru używając ciśnieniomierza. Wadami takiego rozwiązania jest czas dotarcia do pacjenta, oraz obciążenie obowiązkiem pomiaru ograniczonej ilości osób, co ogranicza częstotliwość pomiarów i niweluje szanse na wykrycie potencjalnego ryzyka dla zdrowia pacjenta. Pomiary pulsu wykonywane automatycznie przez urządzenie, które pacjent nosiłby przy sobie, zwiększyłoby częstotliwość tych pomiarów. Dodatkowo urządzenie pozwoliłoby na monitorowanie stanu zdrowia pacjentów z jednego miejsca. Urządzenie mogłoby zoptymalizować częstotliwość wykonywania pomiarów, przykładowo zwiększając ją w przypadkach rosnącego ryzyka. Za to w przypadkach, gdzie parametry zdrowia pacjenta nie wskazują na żadne sytuacje zagrażające jego zdrowiu i życiu, pomiary mogły by być dokonywane rzadziej w celu zaoszczędzenia energii. 

Dodatkowym odciążeniem personelu, zwłaszcza w domach opieki z dużym terenem, po którym pacjenci mogą się swobodnie poruszać, byłaby możliwość monitorowania lokalizacji pacjenta. Pozwoliłoby to na ocenę czy pacjent nie oddalił się zbyt daleko lub na natychmiastowe namierzenie pacjenta w sytuacjach krytycznych. 

Automatyzacja pomiarów pozwala na szybsze wykrywanie sytuacji krytycznych, a dostęp do lokalizacji pacjenta przyspiesza czas reagowania na takie sytuacje.

## 2 Opis techniczny
### 2.1 Schemat
#### Kluczowe elementy
Prototyp urządzenia zawiera trzy kluczowe elementy:
- pulsometr
	- służący do monitorowania pulsu pacjenta
	- częstość pomiarów dostosowana do stanu pacjenta lub skonfigurowana przez personel
- moduł GPS
	- dający możliwość namierzenia lokalizacji pacjenta (link do Google Maps)
- moduł GSM
	- realizacja komunikacji między personelem, a urządzeniem za pomocą SMS
	- możliwość konfiguracji urządzenia
#### 2.2 Architektura rozwiązania
![](1.png)
### 2.3 Algorytm działania
#### 2.3.1 Opis słowny
Urządzenie dokonuje pomiarów pulsu domyślnie co 5 minut. Jeśli puls uznawany jest za mieszczący się w normie, urządzenie na podany numer (należący do osoby z personelu) wysyła pomiar raz dziennie. Jeśli wartość pulsu odbiega od normy pomiary wysyłane są do personelu za każdym razem. 
Dodatkowo urządzenie na żądanie personelu jest w stanie wygenerować link do Google Maps z aktualną lokalizacją.

#### 2.3.2 Protokół komunikacji z urządzeniem za pomocą SMS
Konfiguracja SMS pozwala na:
- dodanie/usunięcie numeru telefonu, na który wysyłane są pomiary
- ustawienie zakresu wartości pulsu uznawanego za normę
- zażądanie lokalizacji urządzenia
##### 2.3.2.1 Ustawienie numeru telefonu, na który wysyłane są pomiary
`tel <xxx xxx xxx>`

Parametry:<br>
`xxx xxx xxx` - numer telefonu, na który wysyłane mają być pomiary

Działanie:<br>
Urządzenie przechowuje numer, na które wysyłane są wiadomości SMS po pomiarze. Komenda powoduje podmianę tego numeru.

##### 2.3.2.3 Ustawienie zakresu wartości pulsu uznawanego za normę
`pulse norm <min> <max>`

Parametry:<br>
`min` - minimalna wartości pulsu uznawana za normę<br>
`max` - maksymalna wartości pulsu uznawana za normę<br>

Działanie: <br>
Urządzenie dokonując pomiaru pulsu sprawdza czy jego wartość mieści się w normie. Jeśli nie urządzenie przechodzi w `PANIC` mode i pomiary wysyłane są co 5 minut, a nie raz dziennie.

##### 2.3.2.4 Zażądanie lokalizacji urządzenia
`loc req`

Działanie:<br>
Po otrzymaniu wiadomości urządzenie odczytuje lokalizację za pomocą modułu GPS, generuje link do Google Maps.