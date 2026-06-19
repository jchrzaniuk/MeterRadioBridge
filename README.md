# MeterRadioBridge — instrukcja obsługi

Gratulacje! Trzymasz w ręku **MeterRadioBridge** — gotowe urządzenie do
**nasłuchiwania radiowych liczników** (wodomierzy, ciepłomierzy, gazomierzy,
podzielników kosztów), które nadają w standardzie **wireless M-Bus (868 MHz)** —
tym samym, którego używa większość liczników z odczytem radiowym w Polsce
i Europie.

Urządzenie odbiera ich transmisje, rozszyfrowuje (jeśli masz klucz) i pokazuje
odczyty w przejrzystym panelu na telefonie lub komputerze. Może też wysyłać dane
do Home Assistant (MQTT) albo własnego systemu (webhook).

Firmware jest już wgrany — ta instrukcja przeprowadzi Cię przez uruchomienie
i wszystkie funkcje. Nie potrzebujesz żadnego oprogramowania ani kabla; wszystko
robisz przez przeglądarkę.

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
Podłącz urządzenie kablem USB-C. Po chwili zaświeci się dioda — urządzenie startuje.

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

## 3. Panel — przegląd

Na dole są trzy zakładki:

- **Liczniki** — lista wykrytych urządzeń i ich odczyty.
- **Logi** — strumień ostatnich odebranych ramek radiowych (podgląd i diagnostyka).
- **Ustawienia** — Wi-Fi, integracje, radio, dostęp do panelu, informacje o
  systemie i zarządzanie urządzeniem.

---

## 4. Zakładka „Liczniki"

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

Zapisz przyciskiem **Dodaj licznik**. Licznik do usunięcia: **Usuń licznik**.

> 🔑 **Skąd wziąć klucz AES?** Od administratora/dostawcy liczników (spółdzielnia,
> firma rozliczeniowa, producent). Urządzenie samo nie złamie szyfrowania —
> bez klucza zobaczysz, że licznik istnieje i jak mocny ma sygnał, ale nie odczyty.

### Analiza ramki (przycisk „Analizuj ramkę na wmbusmeters.org")

W szczegółach licznika — gdy urządzenie odebrało już od niego choć jedną
transmisję — pojawia się przycisk **„Analizuj ramkę na wmbusmeters.org"**.
Otwiera on w nowej karcie zewnętrzny serwis **wmbusmeters.org** z ostatnią
odebraną ramką tego licznika i pokazuje jej szczegółową analizę.

Do czego się przydaje:
- **Ustalenie właściwego sterownika** — gdy „auto" nie pokazuje sensownych
  wartości, analiza często podpowiada, jakiego producenta/sterownika użyć
  (wtedy ustaw go w polu **Sterownik**).
- **Diagnostyka** nietypowego lub nierozpoznanego licznika.

> 🌐 **Uwaga o prywatności**: kliknięcie wysyła surową ramkę do zewnętrznej strony
> (otwiera się w nowej karcie przeglądarki). **Klucz AES nigdy nie jest wysyłany** —
> zostaje wyłącznie na urządzeniu. Dla liczników zaszyfrowanych przesyłana ramka
> i tak pozostaje zaszyfrowana; dla nieszyfrowanych zawiera bieżący odczyt.

---

## 5. Zakładka „Logi"

Strumień **ostatnich odebranych ramek radiowych**. Najprostszy sposób sprawdzić,
czy urządzenie w ogóle coś słyszy. U góry jest licznik ramek i przycisk
**Wyczyść** (czyści tylko podgląd — nie kasuje liczników ani historii). Kropka
przy ikonie zakładki sygnalizuje nowe ramki od ostatniego wejścia.

Gdy lista jest pusta, urządzenie jeszcze nic nie odebrało — sprawdź antenę,
**tryb radia** (Ustawienia → Radio) i ustawienie urządzenia względem liczników.

---

## 6. Zakładka „Ustawienia"

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
- **Host / IP**, **Port** (zwykle 1883), **Użytkownik**, **Hasło** — dane brokera.
- **Prefix** — początek tematów MQTT (np. `wmbus`).
- **Alerty MQTT** — dodatkowe powiadomienia.

Po włączeniu liczniki z nadaną nazwą pojawią się w Home Assistant automatycznie
(autodiscovery) jako encje — nie trzeba nic dopisywać w konfiguracji HA.

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

---

### Zarządzanie (kopie, aktualizacja, reset)

Na dole zakładki Ustawienia:

- **Eksport konfiguracji** — pobiera plik z ustawieniami liczników (nazwy,
  sterowniki, klucze). Trzymaj go jako kopię zapasową.
- **Import konfiguracji** — wgrywa wcześniej zapisany plik (np. po wymianie
  urządzenia lub przywróceniu).
- **Aktualizacja firmware (OTA)** — wgranie nowej wersji oprogramowania przez
  przeglądarkę. Strona ma dwa pola: **firmware** (`firmware.bin`) oraz
  **interfejs** (`littlefs.bin` — sam panel web). Urządzenie weryfikuje obraz
  i po wgraniu się zrestartuje. Ustawienia (liczniki, MQTT, hasło…) są zachowane.
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
Gdy chcesz wyczyścić wszystko (np. zapomniane hasło panelu): **przytrzymaj
przycisk użytkownika (user button) na urządzeniu przez ok. 5 sekund**. Skasuje to
wszystkie ustawienia, liczniki i historię — urządzenie wróci do sieci
`MeterRadioBridge-Setup` jak przy pierwszym uruchomieniu.

---

## 7. Rozwiązywanie problemów

| Problem | Co zrobić |
|---|---|
| Nie widzę żadnych liczników | Daj kilka–kilkanaście minut (niektóre liczniki nadają rzadko, nawet raz na godzinę). Sprawdź, czy antena jest dokręcona, i ustaw urządzenie bliżej liczników. T1 i C1 są nasłuchiwane jednocześnie — trybu nie trzeba zmieniać. |
| Licznik widoczny, ale bez odczytów, z kłódką 🔒 | Jest zaszyfrowany — wpisz **Klucz AES-128** w jego konfiguracji. |
| Wartości wyglądają bez sensu | Zmień **Sterownik** z „auto" na właściwy dla producenta, albo sprawdź **Typ urządzenia**. |
| `meterradiobridge.local` nie otwiera się | Wejdź po adresie IP urządzenia (znajdziesz go na liście klientów routera). |
| Nie pamiętam hasła do panelu | Zrób **factory reset** przyciskiem (5 s) i skonfiguruj od nowa. |
| Nie mogę zaktualizować firmware / wyeksportować konfiguracji (błąd 403) | Te funkcje **wymagają ustawionego hasła panelu** — ustaw je w *Ustawienia → Dostęp do panelu* i spróbuj ponownie. |
| „Za dużo prób logowania" (błąd 429) | Zbyt wiele błędnych haseł pod rząd — odczekaj ok. minutę i wpisz poprawne hasło. |
| Słaby sygnał (RSSI ~ −90) | Przesuń urządzenie bliżej liczników / wyżej / z dala od powierzchni metalowych i innych nadajników. |
| Zgubiłem Wi-Fi (zmiana routera) | Urządzenie po pewnym czasie samo wystawi awaryjną sieć `MeterRadioBridge-Setup` — połącz się i skonfiguruj nowe Wi-Fi. |

---

## 8. Nierozpoznany licznik? Pomóż ulepszyć dekodowanie

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

**Sposób A — jedna ramka, najprościej.** W szczegółach licznika kliknij
**„Analizuj ramkę na wmbusmeters.org"** (sekcja 4). W otwartym adresie znajduje
się **surowa ramka w hex** — skopiuj cały ten ciąg. Wystarczy na początek, ale
do dekodera lepiej zebrać kilka.

**Sposób B — wiele ramek, zalecane.** Włącz **REST API** (Ustawienia → REST API)
i pobierz ostatnie ramki do pliku — z komputera w tej samej sieci:

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

## 9. Dobre praktyki

- **Ustaw hasło panelu** (Dostęp do panelu) — zwłaszcza z kluczami AES.
- **Zrób eksport konfiguracji** po skonfigurowaniu liczników.
- Postaw urządzenie **centralnie** względem liczników, z dobrą widocznością radiową.
- Zasilaj ze **stabilnego źródła** (dobra ładowarka USB) — urządzenie pracuje 24/7.

---

## 10. Wspierane liczniki (sterowniki)

Urządzenie ma wbudowany zestaw **sterowników** rozpoznających liczniki różnych
producentów. Dla większości standardowych liczników wystarcza **auto (generyczny
DIF/VIF)** — konkretny sterownik z listy poniżej wybierz dopiero wtedy, gdy „auto"
nie pokazuje sensownych wartości (sekcja 4 → *Sterownik*). Pełną, aktualną listę
masz zawsze w panelu (pole *Sterownik*) oraz pod `GET /api/drivers`.

Sterowniki w podziale na **mierzone medium** (pogrubione = rodziny potwierdzone na
realnych licznikach):

### 💧 Woda (wodomierze)
| Sterownik | Producent / linia |
|---|---|
| **`mkradio3`, `mkradio3a`, `mkradio4`, `mkradio4a`** | Techem MK Radio 3/4 |
| **`izar`** | Diehl / IZAR / Sappel / Hydrometer (PRIOS, nieszyfrowany) |
| **`apator162`, `apatorna1`** | Apator |
| `multical21` | Kamstrup |
| `minomess` | Minol |
| `supercom587`, `sensusrf95` | Sensus |
| `qwater` | Qundis |
| `istameter` | ista |

### 🔥 Ciepło (ciepłomierze)
| Sterownik | Producent / linia |
|---|---|
| **`compact5`, `vario451`** | Techem |
| **`apator172`** | Apator |
| `multical302` | Kamstrup |
| `landisgyrus450`, `ultraheat` | Landis+Gyr / Sensus |
| `qheat` | Qundis |
| `wme5` | ciepłomierz ultradźwiękowy |

### 🌡️ Podzielniki kosztów ciepła (HCA)
| Sterownik | Producent / linia |
|---|---|
| **`fhkvdataiii`, `fhkvdataiv`** | Techem FHKV |
| **`bfw240radio`** | Brunata |
| `apatoreitn` | Apator |
| `qcaloric` | Qundis |
| `sontex868` | Sontex |

### ⚡ Energia elektryczna
| Sterownik | Producent / linia |
|---|---|
| **`amiplus`** | Iskra / Apator (np. Otus3) |
| `omnipower` | Kamstrup |
| `abb` | ABB |
| `emhelectric` | EMH |
| `esyswm` | EasyMeter |
| `elster` | Elster / Honeywell |
| `siemens` | Siemens |

### 🟡 Gaz (gazomierze)
| Sterownik | Producent / linia |
|---|---|
| `apator08` | Apator |
| `aerius` | gazomierz radiowy |

> Część liczników (np. Diehl/Sappel oraz wodne Apatora) nadaje **nieszyfrowane**
> mimo własnościowego formatu — urządzenie odczyta je **bez klucza**. Pozostałe
> wymagają **klucza AES** (kłódka 🔒). Lista wspieranych modeli rośnie z każdą
> aktualizacją firmware — jeśli Twojego licznika brakuje, patrz sekcja 8
> („Nierozpoznany licznik? Pomóż ulepszyć dekodowanie").

---

## 11. Słowniczek

- **wM-Bus** — radiowy standard liczników mediów (woda, ciepło, gaz) na 868 MHz.
- **T1 / C1** — warianty trybu nadawania liczników; oba na 868.95 MHz, łapane
  jednocześnie. T1 to najczęstszy tryb wodomierzy, C1 m.in. podzielniki Qundis.
- **AES-128** — szyfrowanie transmisji; klucz to 32 znaki szesnastkowe.
- **Sterownik (driver)** — sposób interpretacji danych konkretnego producenta.
- **RSSI** — siła odbieranego sygnału w dBm (bliżej 0 = mocniejszy).
- **MQTT** — protokół do przesyłania danych m.in. do Home Assistant.
- **OTA** — aktualizacja oprogramowania „po powietrzu", przez przeglądarkę.

---

*Wersję oprogramowania i czas pracy sprawdzisz w **Ustawienia → System**. Przy
problemach technicznych pomocne są liczniki diagnostyczne — w tej samej sekcji
oraz pod adresem `http://meterradiobridge.local/api/status` (pole `diag`).*
