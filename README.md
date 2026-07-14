# MeterRadioBridge — instrukcja obsługi

Gratulacje! Trzymasz w ręku **MeterRadioBridge** — gotowe urządzenie do
**nasłuchiwania radiowych liczników** (wodomierzy, ciepłomierzy, gazomierzy,
podzielników kosztów), które nadają w standardzie **wireless M-Bus (868 MHz)** —
tym samym, którego używa większość liczników z odczytem radiowym w Polsce
i Europie.

Urządzenie łapie ich transmisje, odszyfrowuje je (o ile masz klucz) i pokazuje
odczyty w panelu — na telefonie albo komputerze. Dane może też przekazywać dalej:
do Home Assistant przez MQTT lub do własnego systemu przez webhook.

Firmware jest już wgrany. Ta instrukcja poprowadzi Cię przez uruchomienie i po
kolei przez wszystkie funkcje. Nie instalujesz żadnego programu, nie podłączasz
kabla do komputera — robisz wszystko w przeglądarce.

---

## 1. Co będzie potrzebne

- Urządzenie **MeterRadioBridge** — kompletne, w obudowie, z dołączoną anteną
  (dostajesz gotowy produkt, nie osobne moduły do złożenia).
- Zasilanie **USB-C** (ładowarka telefonu 5 V lub port USB komputera).
- Telefon lub komputer z Wi-Fi i przeglądarką.
- Twoja domowa sieć **Wi-Fi 2,4 GHz** (urządzenie nie obsługuje 5 GHz).
- *Opcjonalnie:* klucze AES-128 do Twoich liczników (jeśli nadają zaszyfrowane)
  — dostajesz je od dostawcy/administratora liczników.

> 📡 **Antena**: upewnij się, że antena jest dokręcona przed włączeniem.
> Zasięg jest najlepszy, gdy urządzenie stoi centralnie, z dala od powierzchni
> metalowych.

---

## 2. Pierwsze uruchomienie

### Krok 1 — podłącz zasilanie
Podłącz urządzenie kablem USB-C. Po chwili odezwie się dioda LED: dwa błyski,
sekundy przerwy i znowu dwa — to znaczy, że urządzenie wstało i wystawiło własną
sieć konfiguracyjną. Co oznaczają pozostałe sposoby mrugania, opisuje rozdział 3.

### Krok 2 — połącz się z jego siecią konfiguracyjną
Przy pierwszym uruchomieniu (lub gdy nie zna jeszcze Twojego Wi-Fi) urządzenie
tworzy **własną sieć Wi-Fi**:

| | |
|---|---|
| Nazwa sieci (SSID) | **`MeterRadioBridge-Setup`** |
| Hasło | **`meterbridge`** |

Na telefonie/komputerze połącz się z tą siecią. Większość urządzeń od razu
otworzy stronę konfiguracji. Jeśli nie — wpisz w przeglądarce adres:

```
http://192.168.4.1
```

### Krok 3 — wskaż swoje Wi-Fi
W panelu wejdź w **Ustawienia → WiFi**:
1. Kliknij **Skan otoczenia → Skanuj**, żeby zobaczyć sieci w zasięgu.
2. Wybierz swoją sieć (pole **Sieć (SSID)**) i wpisz **Hasło**.
3. Zapisz. Urządzenie połączy się z Twoim Wi-Fi i zrestartuje.

> Jeśli sieci nie ma na liście — sprawdź, czy to pasmo **2,4 GHz** (5 GHz nie jest
> obsługiwane). Niektóre routery rozgłaszają oba pod tą samą nazwą; to w porządku.

### Krok 4 — wejdź do panelu na swoim Wi-Fi
Po połączeniu urządzenie jest dostępne w Twojej sieci pod adresem:

```
http://meterradiobridge.local
```

Gdyby `meterradiobridge.local` nie działało (część routerów tego nie wspiera), sprawdź adres
IP urządzenia na liście klientów routera i wejdź pod `http://<adres-ip>`.

> 📱 **Dodaj do ekranu domowego**: na iPhonie otwórz `meterradiobridge.local` w Safari →
> *Udostępnij* → *Dodaj do ekranu początkowego*. Panel będzie się otwierał jak
> normalna aplikacja.

---

## 3. Przycisk i dioda LED

Na urządzeniu jest **jeden przycisk** i **jedna dioda LED**. Ilekroć w tej
instrukcji pada słowo „przycisk" albo „dioda LED", chodzi właśnie o nie — nie ma
innych, więc nie ma czego mylić.

### Co pokazuje dioda LED

| Dioda | Co się dzieje |
|---|---|
| powoli rozjaśnia się i gaśnie, jak oddech (cykl ok. 6 s) | praca normalna — urządzenie jest w Twoim Wi-Fi i nasłuchuje liczników |
| dwa błyski, potem ok. 2 s ciemno — i tak w kółko | urządzenie wystawia własną sieć `MeterRadioBridge-Setup`: albo nie zna jeszcze Twojego Wi-Fi, albo nie potrafi się do niego połączyć |
| szybkie miganie, kilka razy na sekundę | trzymasz przycisk — trwa odliczanie do przywrócenia ustawień fabrycznych |
| trzy szybkie błyski i restart | ustawienia fabryczne przywrócone |

Dioda **nie sygnalizuje odbioru ramek** — mruga tak samo, gdy liczniki nadają,
jak i wtedy, gdy w eterze jest cisza. To, czy urządzenie coś słyszy, sprawdzisz
w zakładce **Logi**.

### Co robi przycisk

Krótkie naciśnięcie **nie robi nic** i tak ma być. (Dawniej przełączało tryb
radia, ale T1 i C1 są dziś nasłuchiwane jednocześnie, więc nie ma czego
przełączać — patrz rozdział 7 → *Radio*.) Przycisk służy do jednej rzeczy:
**przywrócenia ustawień fabrycznych**.

### Przywrócenie ustawień fabrycznych (factory reset)

Przydaje się, gdy zapomnisz hasła do panelu, przenosisz urządzenie do zupełnie
innej sieci albo oddajesz je komuś dalej i chcesz zabrać z niego swoje dane.

1. Urządzenie ma być **zasilone i normalnie pracować**. Nic nie odłączasz, nie ma
   żadnego trybu serwisowego.
2. **Wciśnij przycisk i trzymaj.** Po pół sekundy dioda zacznie szybko migać —
   to znak, że odliczanie ruszyło.
3. **Trzymaj przez 5 sekund.** Gdy czas minie, dioda błyśnie trzy razy,
   a urządzenie samo się zrestartuje. Wtedy możesz puścić przycisk.
4. Po restarcie urządzenie znów wystawia sieć **`MeterRadioBridge-Setup`**
   (hasło `meterbridge`) — konfigurujesz je od nowa, tak jak w rozdziale 2.

Puszczenie przycisku przed upływem 5 sekund przerywa procedurę: nic nie zostaje
skasowane, dioda wraca do zwykłego mrugania. Przypadkowe naciśnięcie jest więc
nieszkodliwe.

> ⚠️ **Reset kasuje wszystko, co ustawiłeś.** Przepadają: nazwa i hasło Twojego
> Wi-Fi, hasło do panelu, lista liczników razem z nazwami i **kluczami AES**,
> ustawienia MQTT, webhooka, REST API i strefy czasowej oraz historia odczytów.
> Zostaje wgrane oprogramowanie — reset **nie cofa** wersji firmware ani nie
> przywraca tej, z którą urządzenie wyszło z fabryki.

> 💾 **Zanim zresetujesz, zrób kopię.** *Ustawienia → Zarządzanie → Eksport
> konfiguracji* zapisuje liczniki i klucze do pliku, a po resecie wracają jednym
> kliknięciem (*Import konfiguracji*). Jest tu haczyk: jeśli resetujesz właśnie
> dlatego, że **nie pamiętasz hasła panelu**, to eksport jest niedostępny (wymaga
> zalogowania) i klucze AES trzeba będzie wpisać ręcznie jeszcze raz. Warto
> trzymać kopię zawczasu.

Jeśli chcesz tylko zrestartować urządzenie, a nie czyścić ustawień, użyj
*Ustawienia → Zarządzanie → **Uruchom ponownie***. Przycisk takiej funkcji nie ma.

---

## 4. Panel — przegląd

Na dole są trzy zakładki:

- **Liczniki** — lista wykrytych urządzeń i ich odczyty.
- **Logi** — strumień ostatnich odebranych ramek radiowych (podgląd i diagnostyka).
- **Ustawienia** — Wi-Fi, integracje, radio, dostęp do panelu, informacje o
  systemie i zarządzanie urządzeniem.

---

## 5. Zakładka „Liczniki"

Każdy licznik, który urządzenie usłyszy, **pojawia się tu automatycznie** — nie
musisz nic konfigurować, żeby go zobaczyć. Konfiguracja przydaje się, by nadać
mu nazwę i (dla zaszyfrowanych) odczytać wartości.

### Wyszukiwanie i filtry
Na górze listy jest pole **„Szukaj"** (filtruje po nazwie lub numerze licznika)
oraz **filtry** — „pigułki" z liczbą sztuk: **Wszystkie**, **Skonfigurowane**
i wg rodzaju: **Woda**, **Ciepła woda**, **Ciepło**, **HCA** (podzielniki),
**Energia**, **Gaz**, **Czujniki**. Filtry rodzajowe pokazują się tylko wtedy,
gdy masz takie liczniki. Kliknij pigułkę, by zawęzić listę.

Przy każdym liczniku zobaczysz:

| Symbol / pole | Znaczenie |
|---|---|
| 🔒 kłódka | Transmisja jest zaszyfrowana — potrzebny klucz AES, by odczytać wartości |
| 🔋 ikona baterii | Licznik zgłasza niski poziom baterii |
| ⚠️ trójkąt | Licznik zgłasza błąd/alarm |
| pasek sygnału (RSSI) | Siła odbioru — im bliżej 0, tym lepiej (np. −60 jest lepsze niż −90) |
| czas | Kiedy ostatnio słyszano licznik |

Kliknij licznik, żeby otworzyć jego szczegóły: aktualny odczyt, zużycie,
ostatnie pomiary i zakres min–max, a niżej **konfigurację**.

### Numer licznika a jego zapis w telegramie
Panel i logi pokazują numer licznika **normalnie** — tak, jak czytasz go z
tarczy. Ale jeśli zajrzysz w **surową ramkę** (hex w zakładce „Logi" lub przy
analizie), ten sam numer wygląda inaczej: jest zapisany **bajtami w odwrotnej
kolejności** (kodowanie BCD, little-endian). To normalne dla wM-Bus, nie błąd.

Numer dzieli się na pary cyfr (bajty), a w telegramie idą one **od końca**:

```
numer na liczniku:   12 34 56 78
w surowej ramce:     78 56 34 12
```

Żeby odczytać numer z ramki, robisz to samo wstecz — bierzesz cztery bajty
adresu i odwracasz ich kolejność. Urządzenie robi to za Ciebie, więc na liście
i w odczytach masz już numer w czytelnej postaci; odwrócony zapis zobaczysz tylko
patrząc bezpośrednio w bajty telegramu.

> Drobiazg na później: u niektórych liczników za nadawanie odpowiada osobny
> moduł radiowy z własnym numerem — wtedy na liście jako `id` widać numer
> nadajnika, a numer z tarczy bywa przenoszony **w środku** telegramu (firmware
> pokazuje go w logach jako „↳ licznik …"). Jeśli numery się nie zgadzają, to
> może być właśnie ten przypadek — porównaj, zanim uznasz licznik za obcy.

### Wodomierze Techem — dwie ramki z jednego licznika

Wodomierze **Techem** to najczęstszy przypadek, w którym jeden licznik nadaje
**dwie różne ramki**: jedną **niekodowaną** (jawną) i jedną **zaszyfrowaną**.
Nie jest to błąd ani duplikat — to dwa równoległe „światy" tego samego modułu
radiowego. Warto rozumieć, który numer skąd pochodzi, bo w grę wchodzą **trzy**
identyfikatory naraz.

**Trzy numery, które trzeba rozróżnić:**

| Numer | Skąd | Gdzie go widać |
|---|---|---|
| **Numer z tarczy** | wydrukowany na liczniku wody | na liście jako `dial` / „numer z tarczy" |
| **ID modułu radiowego (DLL)** | nadany fabrycznie modułowi-nadajnikowi | na liście jako `id`, adres w nagłówku ramki |
| — | (payload zaszyfrowany) | niedostępny bez klucza |

Moduł radiowy jest **fizycznie osobnym urządzeniem** doczepionym do wodomierza,
więc ma **własny numer**, inny niż ten z tarczy. Stąd biorą się dwa numery dla
jednego licznika.

**Jak wyglądają obie ramki — na Twoich rzeczywistych licznikach:**

```
                          ┌─ RAMKA NIEKODOWANA (jawna) ────────────────┐
                          │  tryb T1 · sterownik mkradio3              │
                          │  adres w ramce = ID MODUŁU                 │
   Zimna woda             │  NIESIE: odczyt objętości                 │
   tarcza 35205528  ──────┤  numer z tarczy NIE występuje w tej ramce │
   moduł  01411489        └───────────────────────────────────────────┘
                          ┌─ RAMKA KODOWANA (zaszyfrowana) ────────────┐
                          │  tryb C1 · typ 0x37 „konwerter radiowy"   │
                          │  adres w ramce = ID MODUŁU (ten sam)      │
                          │  jawny nagłówek: NUMER Z TARCZY 35205528  │
                          │  payload: zaszyfrowany (mode 5) 🔒        │
                          └───────────────────────────────────────────┘
```

Ten sam schemat dla ciepłej wody: tarcza **34541205** ↔ moduł **10279416**.

**Zależności między numerami — o co tu chodzi:**

- **ID modułu** (`01411489`, `10279416`) to numer, którym licznik przedstawia
  się w eterze — w **obu** ramkach jest taki sam. To jego adres na liście.
- **Numer z tarczy** (`35205528`, `34541205`) — ten, który znasz z licznika —
  **nie pojawia się w ramce jawnej w ogóle**. Występuje **tylko** w jawnym
  nagłówku ramki zaszyfrowanej. Dlatego urządzenie potrafi go pokazać przy
  liczniku (para moduł ↔ tarcza), mimo że sam klucz jest nieznany.
- **Odczyt** (objętość wody) niesie **wyłącznie ramka niekodowana**. To na niej
  stoi cały odczyt Techema — dlatego liczniki działają automatycznie, sterownik
  `mkradio3`, **bez żadnego klucza**.
- **Ramka zaszyfrowana** jest przydatna z jednego powodu: to z niej wyciągamy
  **numer z tarczy** do sparowania. Co niesie jej payload — nie wiadomo i bez
  klucza się nie dowiemy (patrz niżej).

> 🔒 **Klucza do ramki Techem nie da się odzyskać.** W odróżnieniu od niektórych
> liczników z fabrycznym lub uproszczonym kluczem, Techem używa pełnego,
> losowego klucza per licznik — bruteforce nie wchodzi w grę. Ale nie jest on
> do niczego potrzebny: **cały odczyt masz z ramki jawnej**. Klucz zmieniłby coś
> tylko, gdybyś chciał zajrzeć do zaszyfrowanego payloadu, a ten i tak najpewniej
> powtarza dane, które już widzisz.

> **W skrócie:** jeśli na liście zobaczysz przy jednym wodomierzu Techem numer
> `id` inny niż numer z tarczy — tak ma być. `id` to moduł radiowy, numer z
> tarczy urządzenie dokłada z osobnej, rzadszej ramki zaszyfrowanej. Nie kasuj
> „obcego" licznika, zanim nie sprawdzisz, czy to nie moduł Twojego własnego.

### Konfiguracja licznika

W szczegółach licznika (lub przyciskiem **+ Dodaj licznik**, jeśli chcesz dodać
go ręcznie, zanim się odezwie) ustawiasz:

- **ID licznika (8 cyfr)** — numer urządzenia (przy ręcznym dodawaniu).
- **Nazwa** — przyjazna etykieta, np. „Zimna woda · łazienka".
- **Sterownik** — sposób dekodowania odczytów:
  - **auto (generyczny DIF/VIF)** — dobry domyślny wybór, działa dla większości
    standardowych liczników.
  - konkretny sterownik — wybierz, jeśli wiesz, jakiego producenta jest licznik
    i „auto" nie pokazuje sensownych wartości.
- **Typ urządzenia** — np. Wodomierz (zimna/ciepła woda), Ciepłomierz, Gazomierz,
  Licznik energii elektrycznej, Podzielnik kosztów (HCA), Czujnik/inne. Wpływa
  tylko na ikonę i opis; jeśli zostawisz „Nieznany", dostroi się z odebranej ramki.
- **Klucz AES-128** — 32 znaki szesnastkowe (0–9, A–F). Wpisz, jeśli licznik
  nadaje zaszyfrowany (kłódka 🔒). Bez prawidłowego klucza wartości nie zostaną
  odczytane. Przycisk **Usuń zapisany klucz** czyści wcześniej zapisany klucz.

> **Wodomierze Sensus iPerl (i pokrewne Itron)** — na tarczy numer bywa
> wydrukowany z prefiksem, np. „8 SEN 2086 3703". Do pola **ID licznika** wpisz
> po prostu osiem cyfr po oznaczeniu „SEN" (tu: `20863703`) — firmware sam
> dopasuje je do ramki (nie odwracasz bajtów, nie przeliczasz na hex). iPerl ma
> **wbudowany klucz fabryczny**, więc pola **Klucz AES** nie wypełniasz — licznik
> czyta się automatycznie (sterownik `iperl`).

Zapisz przyciskiem **Dodaj licznik**. Licznik do usunięcia: **Usuń licznik**.

> 🔑 **Skąd wziąć klucz AES?** Od administratora/dostawcy liczników (spółdzielnia,
> firma rozliczeniowa, producent). Urządzenie samo nie złamie szyfrowania —
> bez klucza zobaczysz, że licznik istnieje i jak mocny ma sygnał, ale nie odczyty.

---

## 6. Zakładka „Logi"

Strumień **ostatnich odebranych ramek radiowych**. Najprostszy sposób sprawdzić,
czy urządzenie w ogóle coś słyszy. U góry jest licznik ramek i przycisk
**Wyczyść** (czyści tylko podgląd — nie kasuje liczników ani historii). Kropka
przy ikonie zakładki sygnalizuje nowe ramki od ostatniego wejścia.

Gdy lista jest pusta, urządzenie jeszcze nic nie odebrało — sprawdź antenę,
**tryb radia** (Ustawienia → Radio) i ustawienie urządzenia względem liczników.

---

## 7. Zakładka „Ustawienia"

Sekcje opisane w kolejności od góry panelu.

### Dostęp do panelu (ustaw hasło!)
Ustaw **Użytkownik** i **Hasło**, żeby panel wymagał logowania. Puste pole
użytkownika = brak hasła (każdy w sieci ma dostęp do podglądu i ustawień).

> 🔐 **Operacje wrażliwe wymagają ustawionego hasła.** Dopóki nie ustawisz hasła
> panelu, **zablokowane** są (zwracają błąd 403):
> - **Aktualizacja firmware (OTA)** — żeby nikt z Twojej sieci nie wgrał obcego
>   oprogramowania na świeżym urządzeniu,
> - **Eksport i import konfiguracji** — bo plik zawiera **klucze AES** Twoich
>   liczników.
>
> Podgląd liczników i pierwsza konfiguracja Wi-Fi działają bez hasła, ale by
> aktualizować firmware lub robić kopie — **najpierw ustaw hasło tutaj**. Tym
> bardziej, jeśli wpisujesz klucze AES.

> 🛡️ **Ochrona przed zgadywaniem hasła:** po kilku błędnych próbach logowania
> urządzenie chwilowo blokuje kolejne (na ok. minutę). To normalne — odczekaj
> i wpisz poprawne hasło.

### WiFi
Sieć, z którą łączy się urządzenie. Tu też zmienisz ją na inną (**Skan otoczenia**
pomaga wybrać). Po zmianie urządzenie się zrestartuje.

### MQTT (integracja z Home Assistant)
Pozwala wysyłać odczyty do **Home Assistant** lub innego brokera MQTT.
- **Włącz MQTT** — przełącznik.
- **Host / IP**, **Port** (zwykle 1883; z TLS zwykle 8883), **Użytkownik**, **Hasło** — dane brokera.
- **TLS / SSL** i **Certyfikat CA (PEM)** — szyfrowanie połączenia, opis niżej.
- **Prefix** — początek tematów MQTT (np. `wmbus`).
- **Alerty MQTT** — dodatkowe powiadomienia.

Po włączeniu liczniki z nadaną nazwą pojawią się w Home Assistant automatycznie
(autodiscovery) jako encje — nie trzeba nic dopisywać w konfiguracji HA.

Zmiany ustawień MQTT stosują się **od razu, bez restartu urządzenia** — mostek
sam rozłącza się i łączy z nowymi parametrami.

### MQTT po TLS (szyfrowanie i weryfikacja brokera)

Włączenie **TLS / SSL** szyfruje połączenie z brokerem (podsłuch w sieci nie
zobaczy danych ani hasła MQTT). Przełącznik sam podpowie port **8883**.

Samo szyfrowanie nie sprawdza jednak, **z kim** rozmawiasz. Do tego służy pole
**Certyfikat CA (PEM)** — wklej tu certyfikat urzędu (CA), który podpisał
certyfikat Twojego brokera:
- **broker w chmurze** (np. HiveMQ Cloud) — certyfikat CA znajdziesz
  w dokumentacji dostawcy;
- **własny broker** (np. Mosquitto) — plik `ca.crt` z przepisu poniżej.

Z ustawionym CA mostek weryfikuje certyfikat brokera (podpis, adres, daty
ważności) i **odmówi połączenia z podszywającym się serwerem**. Pod polem
widać, komu mostek ufa: nazwę CA, wystawcę, datę ważności i **odcisk SHA-256**
— możesz go porównać z odciskiem na serwerze
(`openssl x509 -in ca.crt -noout -fingerprint -sha256`).

Zasady pola: puste przy zapisie = zachowaj obecny certyfikat; wklejony tekst =
ustaw nowy; **Usuń CA** = wróć do samego szyfrowania bez weryfikacji.

> Weryfikacja certyfikatu wymaga poprawnego czasu — urządzenie musi mieć
> synchronizację NTP (sekcja System pokaże `NTP: ✓ OK`).

#### Własny broker Mosquitto z TLS (przepis)

Na maszynie z brokerem (przykład: Linux, mosquitto 2.x):

> ⚠️ **Łączysz się po adresie IP?** Wpisz ten adres w certyfikacie **dwa razy**:
> raz jako `IP.1` i raz jako `DNS.1`. Mostek (mbedtls w ESP32) sprawdza adres
> tylko po wpisach `DNS` — bez tego drugiego odrzuci certyfikat, a w logu brokera
> zobaczysz `bad certificate`. Widać to w polu `[alt]` poniżej.

```bash
# 1. Własny urząd (CA) — 10 lat; klucz ca.key trzymaj w tajemnicy
openssl genrsa -out ca.key 2048
openssl req -x509 -new -key ca.key -sha256 -days 3650 \
  -subj "/CN=MQTT-CA-MOJDOM" -out ca.crt

# 2. Certyfikat serwera — w SAN adresy, którymi łączą się klienci
#    (adres IP jako IP.x ORAZ DNS.x — patrz uwaga nad blokiem)
cat > san.cnf <<EOF
[req]
distinguished_name = dn
req_extensions = ext
prompt = no
[dn]
CN = MQTT-MOJDOM
[ext]
subjectAltName = @alt
keyUsage = critical,digitalSignature,keyEncipherment
extendedKeyUsage = serverAuth
[alt]
IP.1  = 192.168.1.100
DNS.1 = 192.168.1.100
DNS.2 = localhost
EOF
openssl genrsa -out server.key 2048
openssl req -new -key server.key -config san.cnf -out server.csr
openssl x509 -req -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial \
  -days 3650 -sha256 -extensions ext -extfile san.cnf -out server.crt

# 3. Konfiguracja mosquitto — dodatkowy listener TLS (jawny 1883 może zostać)
sudo install -m 644 server.crt /etc/mosquitto/certs/
sudo install -m 640 -g mosquitto server.key /etc/mosquitto/certs/
sudo tee /etc/mosquitto/conf.d/tls.conf >/dev/null <<EOF
listener 8883
certfile /etc/mosquitto/certs/server.crt
keyfile /etc/mosquitto/certs/server.key
EOF
sudo systemctl restart mosquitto
```

W panelu mostka: **Port** `8883`, **TLS / SSL** włączone, do pola
**Certyfikat CA (PEM)** wklej zawartość `ca.crt` — i Zapisz. Test z innego
komputera: `mosquitto_sub -h 192.168.1.100 -p 8883 --cafile ca.crt -v -t 'wmbus/#'`.

### Webhook
Wysyła odczyt jako **HTTP POST (JSON)** pod wskazany adres przy każdym pomiarze
nazwanego licznika — przydatne do własnych skryptów/systemów.
- **Włącz webhook**, **URL (HTTP POST)** — np. `http://serwer:5678/webhook`.

### REST API
**Włącz REST API**, by odpytywać urządzenie z własnych skryptów/systemów. Adresy
zaczynają się od `http://meterradiobridge.local` (lub adresu IP), a odpowiedzi są w formacie
**JSON**. Jeśli ustawiłeś hasło panelu, zapytania wymagają go (HTTP Basic Auth,
np. `curl -u użytkownik:hasło ...`).

Najważniejsze endpointy:

| Metoda | Adres | Opis |
|---|---|---|
| GET | `/api/status` | Stan urządzenia: wersja, czas pracy, Wi-Fi, NTP oraz `diag` (liczniki ramek, błędów, CRC, nieudanych deszyfrowań, duplikatów, resetów) |
| GET | `/api/meters` | Lista wykrytych liczników z ostatnimi odczytami, sygnałem i statusem |
| GET | `/api/drivers` | Lista dostępnych sterowników (do pola „Sterownik") |
| GET | `/api/frames` | Ostatnie surowe ramki radiowe (podgląd/diagnostyka) |
| GET | `/api/settings` | Bieżące ustawienia urządzenia |
| GET | `/api/wifi_scan` | Skan sieci Wi-Fi w zasięgu |
| GET | `/api/config/export` | Pobranie kopii konfiguracji liczników (JSON) |
| POST | `/api/config/import` | Wgranie zapisanej konfiguracji liczników |
| POST | `/api/meter` | Dodanie/zmiana licznika — `{id, name, driver, key, active}` |
| GET / DELETE | `/api/meter/<id>` | Odczyt / usunięcie pojedynczego licznika |
| POST | `/api/restart` | Restart urządzenia |

Przykład — szybki podgląd statusu z komputera:

```
curl http://meterradiobridge.local/api/status
```

### Radio (tryb nasłuchu)
**T1 + C1 (868.95 MHz) — automatycznie.** Tryby T1 i C1 nadają na tej samej
częstotliwości (różni je tylko sposób kodowania), więc urządzenie nasłuchuje
ich **jednocześnie** i samo rozpoznaje tryb każdej odebranej ramki. Nie musisz
nic wybierać ani przełączać — liczniki T1 (np. wodomierze) i C1 (np. podzielniki
ciepła Qundis) są łapane razem, bez utraty ramek.

### Alerty MQTT
- **Alert „licznik zaginął" — godziny** — po ilu godzinach ciszy wysłać alert,
  że licznik przestał nadawać (`0` = wyłączone). Wymaga włączonego MQTT i nadanej
  nazwy licznika; alert idzie na temat `{prefix}/{id}/alert`.

### Strefa czasowa
- **Strefa** — żeby czasy w panelu były lokalne. (Do systemów zewnętrznych dane
  wychodzą zawsze w czasie uniwersalnym UTC — to celowe i prawidłowe.)

### System (informacje)
Pola **tylko do odczytu** — bieżący stan urządzenia: **Wersja** oprogramowania,
**IP**, **WiFi RSSI** (siła sygnału), **Wolny heap** (wolna pamięć), **Uptime**
(czas pracy), liczba **Liczników** oraz status **NTP** (synchronizacja zegara
z internetu — daje poprawne znaczniki czasu). Te same liczniki diagnostyczne są
też dostępne pod `http://meterradiobridge.local/api/status` (pole `diag`).

### Aktualizacja

Urządzenie z dostępem do internetu **aktualizuje się samo**: mniej więcej co
6 godzin sprawdza, czy jest nowe wydanie, i instaluje je w tle (krótki restart,
odczyty i ustawienia zostają). Każde wydanie jest **podpisane cyfrowo** —
urządzenie odrzuci obraz uszkodzony albo podmieniony.

- **Aktualizuj automatycznie** — możesz wyłączyć; wtedy aktualizujesz ręcznie
  przyciskiem poniżej.
- **Sprawdź teraz** — od razu sprawdza i, jeśli jest nowsza wersja, instaluje ją.
  Wynik zobaczysz pod przyciskiem (wraz z krótkim opisem wydania).

> 🌐 **Urządzenie w sieci bez internetu?** Pobierz najnowsze wydanie ze strony
> [meterradiobridge.pl/firmware](https://meterradiobridge.pl/firmware) — plik
> `firmware.bin` **i** podpis `.sig` (możesz na telefon) — a potem wgraj je przez
> *Zarządzanie → Aktualizacja z pliku*.

---

### Zarządzanie (kopie, aktualizacja, reset)

Na dole zakładki Ustawienia:

- **Eksport konfiguracji** — pobiera plik z ustawieniami liczników (nazwy,
  sterowniki, klucze). Trzymaj go jako kopię zapasową.
- **Import konfiguracji** — wgrywa wcześniej zapisany plik (np. po wymianie
  urządzenia lub przywróceniu).
- **Aktualizacja z pliku (.bin)** — ręczne wgranie wydania pobranego z
  [meterradiobridge.pl/firmware](https://meterradiobridge.pl/firmware): wskaż
  plik `firmware.bin` **i** podpis `.sig`, urządzenie zweryfikuje podpis
  wydania i po wgraniu się zrestartuje. Strona ma też drugie pole — **interfejs**
  (`littlefs.bin`, sam panel web) — używane przy aktualizacjach interfejsu.
  Ustawienia (liczniki, MQTT, hasło…) są zachowane.
- **Uruchom ponownie** — zwykły restart.

> 🔐 **Eksport, import i aktualizacja firmware wymagają ustawionego hasła panelu**
> (*Ustawienia → Dostęp do panelu*). Bez hasła te funkcje są zablokowane (błąd
> 403) — to ochrona kluczy AES i przed wgraniem obcego oprogramowania. Ustaw
> hasło, żeby z nich korzystać.

### Konfiguracja jest bezpieczna
Twoje ustawienia liczników (nazwy, klucze) są zapisane w trwałej pamięci
urządzenia i **przeżywają restart oraz aktualizację firmware**. Mimo to warto od
czasu do czasu zrobić **Eksport konfiguracji** — to Twoja kopia na wszelki wypadek.

### Kopia zapasowa i bezpieczeństwo

Konfiguracja liczników (nazwy, sterowniki, **klucze AES**) jest cenna — warto
mieć jej kopię. Masz trzy mechanizmy; pierwszy wystarcza większości użytkowników.

**1. Eksport / import ręczny (zalecane).** W *Ustawienia → Zarządzanie*:
*Eksport konfiguracji* pobiera plik `.json` (na telefon/komputer), *Import
konfiguracji* go przywraca — jednym kliknięciem. Plik trzymaj w bezpiecznym
miejscu (zawiera klucze AES). **Wymaga ustawionego hasła panelu** (*Dostęp do
panelu*) — bez niego eksport/import są zablokowane (ochrona kluczy).

**2. Przypominanie o kopii (przełącznik „Przypominaj o kopii", domyślnie WŁ.).**
Po dodaniu/zmianie licznika panel przypomni Ci o zrobieniu kopii. Możesz wyłączyć
w *Ustawienia → Kopia zapasowa*.

**3. Kopia do MQTT (retained) — opcjonalne, domyślnie WYŁĄCZONE.**
Po włączeniu urządzenie publikuje całą konfigurację jako wiadomość *retained* na
temat `{prefix}/_backup/config`. Broker przechowuje ostatnią wersję, więc kopia
żyje poza urządzeniem i przetrwa nawet jego wymianę (przywracasz, wczytując tę
wiadomość z brokera/Home Assistant i importując).

> ⚠️ **ZAGROŻENIE BEZPIECZEŃSTWA — przeczytaj przed włączeniem.**
> Konfiguracja zawiera **klucze AES Twoich liczników**. Włączając kopię do MQTT:
> - **klucze AES opuszczają urządzenie** i trafiają na broker, i to **trwale**
>   (wiadomość retained zostaje, dopóki jej nie skasujesz),
> - **każdy z dostępem do brokera uzyskuje te klucze** — a więc może odczytywać
>   dane Twoich liczników (zużycie), a w pewnych scenariuszach je podszywać,
> - na **publicznym lub źle zabezpieczonym brokerze oznacza to wyciek kluczy**.
>
> Włączaj **wyłącznie** na **własnym, zaufanym, zabezpieczonym** brokerze:
> z uwierzytelnianiem i listami ACL, najlepiej po **TLS**, **nieeksponowanym do
> internetu**. Przy włączaniu panel poprosi o potwierdzenie tego ryzyka.
>
> **Wyłączenie:** po wyłączeniu przełącznika i restarcie urządzenie czyści
> wiadomość retained (publikuje pustą). Pamiętaj jednak, że broker mógł ją wcześniej
> zalogować/zreplikować — w razie wątpliwości rozważ wymianę kluczy w licznikach.

Jeśli nie masz pewności — **nie włączaj kopii do MQTT** i korzystaj z eksportu
ręcznego. Daje tę samą ochronę bez wystawiania kluczy poza urządzenie.

### Factory reset (przywrócenie ustawień fabrycznych)
W panelu nie ma takiego przycisku — ustawienia fabryczne przywraca się
**przyciskiem na urządzeniu**: wciskasz go i trzymasz przez **5 sekund**, aż
dioda LED błyśnie trzy razy i urządzenie się zrestartuje. Kasuje to wszystkie
ustawienia, liczniki (z kluczami AES) i historię, a urządzenie wraca do sieci
`MeterRadioBridge-Setup` jak przy pierwszym uruchomieniu. Krok po kroku i co
dokładnie ginie — **rozdział 3**.

---

## 8. Rozwiązywanie problemów

| Problem | Co zrobić |
|---|---|
| Nie widzę żadnych liczników | Daj kilka–kilkanaście minut (niektóre liczniki nadają rzadko, nawet raz na godzinę). Sprawdź, czy antena jest dokręcona, i ustaw urządzenie bliżej liczników. T1 i C1 są nasłuchiwane jednocześnie — trybu nie trzeba zmieniać. |
| Licznik widoczny, ale bez odczytów, z kłódką 🔒 | Jest zaszyfrowany — wpisz **Klucz AES-128** w jego konfiguracji. |
| Wartości wyglądają bez sensu | Zmień **Sterownik** z „auto" na właściwy dla producenta, albo sprawdź **Typ urządzenia**. |
| `meterradiobridge.local` nie otwiera się | Wejdź po adresie IP urządzenia (znajdziesz go na liście klientów routera). |
| Nie pamiętam hasła do panelu | Przywróć ustawienia fabryczne: trzymaj **przycisk** na urządzeniu przez **5 s**, aż dioda błyśnie trzy razy (rozdział 3). Skasuje to również liczniki i klucze AES — trzeba je wpisać na nowo. |
| Dioda mruga „dwa błyski i przerwa", choć urządzenie ma być w moim Wi-Fi | Nie udało mu się połączyć, więc wystawił sieć `MeterRadioBridge-Setup` — połącz się z nią i sprawdź nazwę sieci oraz hasło (rozdział 2). Częsta przyczyna: sieć jest na paśmie 5 GHz albo router jest za daleko. |
| Nie mogę zaktualizować firmware / wyeksportować konfiguracji (błąd 403) | Te funkcje **wymagają ustawionego hasła panelu** — ustaw je w *Ustawienia → Dostęp do panelu* i spróbuj ponownie. |
| „Za dużo prób logowania" (błąd 429) | Zbyt wiele błędnych haseł pod rząd — odczekaj ok. minutę i wpisz poprawne hasło. |
| Słaby sygnał (RSSI ~ −90) | Przesuń urządzenie bliżej liczników / wyżej / z dala od powierzchni metalowych i innych nadajników. |
| Zgubiłem Wi-Fi (zmiana routera) | Urządzenie po pewnym czasie samo wystawi awaryjną sieć `MeterRadioBridge-Setup` — połącz się i skonfiguruj nowe Wi-Fi. |
| MQTT z TLS nie łączy się (a bez TLS działa) | 1) Sprawdź port (TLS zwykle **8883**). 2) Sprawdź NTP (*System* → `NTP: ✓ OK`) — weryfikacja certyfikatu wymaga poprawnego czasu. 3) Jeśli łączysz się **po adresie IP** z własnym CA: adres musi być w certyfikacie serwera wpisany w SAN **także jako DNS** (patrz przepis w sekcji MQTT po TLS); objaw po stronie brokera to `bad certificate` w logu. 4) Upewnij się, że wkleiłeś CA **brokera**, nie inny certyfikat — pod polem widać nazwę i odcisk zaufanego CA. |

---

## 9. Nierozpoznany licznik? Pomóż ulepszyć dekodowanie

Czasem licznik jest **widoczny** (urządzenie odbiera jego ramki), ale:
- pokazuje **„Inny"/brak sterownika** i żadnych odczytów, albo
- pokazuje **bezsensowne wartości** mimo wybranego sterownika.

To znaczy, że dla tego konkretnego modelu nie mamy jeszcze poprawnego dekodera.
Możesz pomóc go dorobić — i to **bardzo skutecznie**, bo najlepszym materiałem do
napisania dekodera są **prawdziwe ramki z prawdziwego licznika wraz ze stanem
z jego tarczy**. Z takich zgłoszeń budujemy bazę testową, dzięki której kolejne
wersje firmware rozpoznają coraz więcej liczników.

> 💡 **Najważniejsza rzecz:** odczyt **z tarczy/wyświetlacza licznika** (np.
> `123,456 m³`) zapisany razem z **datą i godziną**. To „punkt odniesienia",
> bez którego ramka jest tylko ciągiem bajtów — a z nim da się rozłożyć ją na
> czynniki i napisać dekoder. Jeśli licznik ma kilka pól (np. stan bieżący
> i rozliczeniowy), spisz wszystkie.

### Co zebrać

1. **Kilka ramek tego licznika** (im więcej, tym lepiej — najlepiej zebrane
   w **różnym czasie**, np. co kilka godzin/dni, żeby wartości się zmieniły).
2. **Odczyt z tarczy** licznika + **data i godzina** odczytu (patrz ramka wyżej).
3. **Oznaczenia z naklejki/tabliczki** licznika: producent, model, numer seryjny.
4. *(opcjonalnie)* **Klucz AES**, jeśli licznik jest zaszyfrowany (kłódka 🔒)
   i chcesz, żeby dało się go zdekodować — patrz uwaga o prywatności niżej.

### Jak pobrać ramki

**Sposób A — jedna ramka, najprościej.** Włącz **REST API** (Ustawienia →
REST API) i pobierz szczegóły licznika — z komputera w tej samej sieci
(`<numer>` = numer licznika z panelu):

```
curl http://meterradiobridge.local/api/meter/<numer>
```

W odpowiedzi pole `raw_hex` to **surowa ramka w hex** — skopiuj cały ten ciąg.
Wystarczy na początek, ale do dekodera lepiej zebrać kilka.

**Sposób B — wiele ramek, zalecane.** Pobierz ostatnie ramki do pliku:

```
curl http://meterradiobridge.local/api/frames > ramki.json
```

Plik zawiera listę ostatnich ramek; każda ma m.in. pola:
- `id_str` — numer licznika (po nim rozpoznasz „swój"),
- `mfr`, `type` — producent i typ urządzenia,
- `enc` — czy zaszyfrowana, `parsed` — czy się zdekodowała,
- `raw_hex` — **surowa ramka** (to jest najważniejsze).

Powtórz pobranie po jakimś czasie i dołącz kilka plików — wtedy widać, jak
zmieniają się wartości (to bardzo pomaga w odgadnięciu, który bajt jest licznikiem).

> Jeśli `meterradiobridge.local` nie działa, użyj adresu IP urządzenia (lista klientów
> routera albo Ustawienia). Gdy ustawiłeś hasło panelu, dodaj `-u użytkownik:hasło`.

### Gdzie zgłosić — załóż issue na GitHubie

Zgłoszenia o nieczytających się / nierozpoznanych licznikach przyjmujemy jako
**issue** w publicznym repozytorium projektu:

> 🐙 **[github.com/jchrzaniuk/MeterRadioBridge/issues](https://github.com/jchrzaniuk/MeterRadioBridge/issues)**
> → przycisk **New issue**. Potrzebne jest darmowe konto GitHub.

W zgłoszeniu dołącz (im więcej, tym szybciej powstanie dekoder):

1. **Ramki hex** — wklej zawartość pliku `ramki.json` albo same wartości `raw_hex`
   (możesz w bloku kodu \`\`\` … \`\`\`).
2. **Odczyt z tarczy** licznika **+ data i godzina** odczytu.
3. **Model licznika** z naklejki: producent, model, numer seryjny.
4. *(opcjonalnie)* **klucz AES**, jeśli licznik jest zaszyfrowany — patrz uwaga
   o prywatności niżej (issue jest **publiczne**, więc klucza i numeru seryjnego
   nie podawaj, jeśli traktujesz je jako wrażliwe).

### Prywatność — zanim założysz issue

> ⚠️ **Issue na GitHubie jest PUBLICZNE** — widzi je każdy. Wszystko, co tam
> wkleisz, staje się jawne. Przemyśl to:

- **Ramka nieszyfrowana** zawiera **bieżący odczyt** Twojego licznika (zużycie).
  Jeśli traktujesz to jako dane wrażliwe — rozważ, zanim wkleisz publicznie.
- **Klucza AES NIE wklejaj do publicznego issue.** To sekret Twojego licznika —
  pozwala odczytać wszystkie jego transmisje. Bez klucza też pomożesz (ramka +
  model i odczyt z tarczy). Jeśli dekoder będzie wymagał klucza, poprosimy
  o przesłanie go **kanałem prywatnym** (np. wiadomość prywatna na GitHubie).
- **Numer seryjny** bywa wrażliwy — możesz go pominąć lub zamazać; producent
  i model wystarczą do rozpoznania.

> 🙌 Dzięki takim zgłoszeniom lista wspieranych liczników rośnie. Każdy nowy,
> potwierdzony odczytem z tarczy egzemplarz trafia do testów i zostaje wsparty
> w kolejnych aktualizacjach firmware (OTA).

---

## 10. Dobre praktyki

- **Ustaw hasło panelu** (Dostęp do panelu) — zwłaszcza z kluczami AES.
- **Zrób eksport konfiguracji** po skonfigurowaniu liczników.
- Postaw urządzenie **centralnie** względem liczników, z dobrą widocznością radiową.
- Zasilaj ze **stabilnego źródła** (dobra ładowarka USB) — urządzenie pracuje 24/7.

---

## 11. Wspierane liczniki (sterowniki)

Urządzenie ma wbudowany zestaw **sterowników** rozpoznających liczniki różnych
producentów. Dla większości standardowych liczników (Kamstrup, Landis+Gyr, Sensus,
Diehl i inne nadające zgodnie z normą EN 13757) wystarcza **auto (generyczny
DIF/VIF)** — po odszyfrowaniu czyta je bez dedykowanego sterownika. Konkretny
sterownik z listy poniżej wybierz dopiero wtedy, gdy „auto" nie pokazuje sensownych
wartości (sekcja 5 → *Sterownik*). Pełną, aktualną listę masz zawsze w panelu (pole
*Sterownik*) oraz pod `GET /api/drivers`.

### ✅ Czytane automatycznie (sterownik „auto", bez wyboru)

Liczniki nadające zgodnie z normą **EN 13757** urządzenie czyta **samo**, bez
wybierania sterownika — jawne od razu, **zaszyfrowane po wpisaniu klucza AES**
(sekcja 5). Poniżej rodziny i modele potwierdzone, że działają na „auto" (większość
zweryfikowana realnymi ramkami). To **nie wymaga** żadnego z dedykowanych
sterowników z dalszej części.

| Medium | Producent / model | Klucz |
|---|---|---|
| 💧 Woda | **Minol / Zenner** (Minomess), **Sontex** (Supercom 587), **Qundis / Zenner** (QWater 5.5) | bez klucza |
| 💧 Woda | **Axioma** (Qalcosonic), Kamstrup, Sensus, Diehl i inne EN 13757 | jawne: bez; szyfrowane: 🔒 |
| 🔥 Ciepło | **Landis+Gyr** (UltraHeat T550 / UH50), **Qundis** (QHeat 5.5) | bez klucza |
| 🔥 Ciepło | Kamstrup (Multical), Techem / L+G (OEM) i inne EN 13757 | jawne: bez; szyfrowane: 🔒 |
| 🔥 Gaz | **Elster / Honeywell**, **Diehl** (Aerius) | bez klucza |
| ⚡ Energia | **ABB** (B23 / B24) i inne EN 13757 | jawne: bez; szyfrowane: 🔒 |
| 🌡️ HCA | **Sontex** (868) | bez klucza |

> Jeśli Twojego producenta nie ma na liście, a licznik nadaje wg EN 13757 (większość
> nowych liczników radiowych) — i tak zostanie odczytany na „auto". Dedykowane
> sterowniki poniżej są tylko dla formatów **własnościowych**, gdzie sam EN nie
> wystarcza.

---

Poniżej **dedykowane sterowniki** do formatów własnościowych (gdzie sam EN nie
wystarcza), w podziale na medium. **Pogrubione = potwierdzone na realnych licznikach.**
🔒 = wymaga klucza AES licznika (od dostawcy/administratora).

### 💧 Woda (wodomierze)
| Sterownik | Producent / linia | Klucz |
|---|---|---|
| **`mkradio3`, `mkradio3a`, `mkradio4`, `mkradio4a`** | Techem MK Radio 3/4 | bez klucza |
| **`izar`** | Diehl / IZAR / Sappel / Hydrometer (PRIOS) | bez klucza |
| **`apator162`, `apatorna1`** | Apator (wodomierze) | bez klucza (auto) |
| **`apator_water_b6`** | Apator AT-WMBUS (wodomierz, format 05/07) | bez klucza (auto) |
| **`apator172`** | Apator AT-WMBUS-17-2 (nakładka JS-02 Smart+/C+) | zależnie od nakładki |
| **`iperl`** | Sensus iPerl | klucz fabryczny (wbudowany) |
| **`hydrus`** | Diehl Hydrus | klucz fabryczny (auto); per-instalacja 🔒 |
| **`bmt`** | BMeters Hydrodigit | bez klucza (auto) |
| `istawater` | ista | 🔒 |

### 🔥 Ciepło (ciepłomierze)
| Sterownik | Producent / linia | Klucz |
|---|---|---|
| **`compact5`, `vario451`** | Techem | bez klucza |
| **`sharky`** | Diehl Sharky | klucz fabryczny |
| **`bmt`** | BMeters (Hydrocal M3) | bez klucza |
| standard EN | Techem / Landis+Gyr (OEM) | jawne: bez; szyfrowane: 🔒 |

### 🌡️ Podzielniki kosztów ciepła (HCA)
| Sterownik | Producent / linia | Klucz |
|---|---|---|
| **`fhkvdataiii`, `fhkvdataiv`** | Techem FHKV | iii: bez; iv: 🔒 |
| **`bfw240radio`** | BFW (podzielnik radiowy) | bez klucza |
| **`qcaloric`** | Qundis (kompakt CI=0x72; Caloric 5.5) | bez klucza |
| **`apatoreitn`** | Apator EITN (podzielnik elektroniczny) | bez klucza |

### ⚡ Energia elektryczna
| Sterownik | Producent / linia | Klucz |
|---|---|---|
| **`amiplus`** | Iskra IE.5 / Apator / Elgama GAMA | EGM (Tauron/PGE): 🔒 |
| `omnipower` | Kamstrup OmniPower | 🔒 |

### 🔥 Gaz (gazomierze)
| Sterownik | Producent / linia | Klucz |
|---|---|---|
| **`unismart`** | Apator UniSmart / Amiteq | bez klucza (auto) |
| **`apator08`** | Apator (gazomierz/wodomierz, format 08) | bez klucza |
| standard EN „1A" | Apator / Apt (jawne) | bez klucza |

> **Klucze:** część liczników (Diehl/Sappel PRIOS, wodne Apatora, BMeters, niektóre
> Techemy) nadaje **nieszyfrowane** mimo własnościowego formatu — urządzenie odczyta
> je **bez klucza**. Pozostałe wymagają **klucza AES** (🔒), który dostajesz od
> dostawcy/administratora liczników.
>
> **Repeater w sieci?** Urządzenia infrastruktury (np. repeater Fidelix) urządzenie
> rozpoznaje i oznacza — nie pomyli ich z licznikiem.
>
> Lista rośnie z każdą aktualizacją firmware — jeśli Twojego licznika brakuje, patrz
> sekcja 9 („Nierozpoznany licznik? Pomóż ulepszyć dekodowanie").

---

## 12. Słowniczek

- **wM-Bus** — radiowy standard liczników mediów (woda, ciepło, gaz) na 868 MHz.
- **T1 / C1** — warianty trybu nadawania liczników; oba na 868.95 MHz, łapane
  jednocześnie. T1 to najczęstszy tryb wodomierzy, C1 m.in. podzielniki Qundis.
- **id (adres nadawcy)** — numer identyfikujący nadawcę w telegramie. Zwykle
  równy numerowi na liczniku; gdy nadaje osobny moduł radiowy, to numer modułu,
  a numer z tarczy bywa w środku telegramu (patrz „↳ licznik" / `tplid`).
- **AES-128** — szyfrowanie transmisji; klucz to 32 znaki szesnastkowe.
- **Sterownik (driver)** — sposób interpretacji danych konkretnego producenta.
- **RSSI** — siła odbieranego sygnału w dBm (bliżej 0 = mocniejszy).
- **MQTT** — protokół do przesyłania danych m.in. do Home Assistant.
- **OTA** — bezprzewodowa aktualizacja oprogramowania, przez przeglądarkę.

---

*Wersję oprogramowania i czas pracy sprawdzisz w **Ustawienia → System**. Przy
problemach technicznych pomocne są liczniki diagnostyczne — w tej samej sekcji
oraz pod adresem `http://meterradiobridge.local/api/status` (pole `diag`).*
