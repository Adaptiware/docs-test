Android aplikácia
=================

Účel a funkcionalita Android aplikácie
--------------------------------------

Úlohou Android aplikácie je poskytnúť strednému manažmentu a servisným pracovníkom IMS rýchly prehľad
o stave systému a upozorniť ho na dôležité stavy a udalosti. Aplikácia sa má stať denne používaným
nástrojom, ktorý veľmi jednoducho a hlavne rýchlo informuje každého pracovníka len o tom, čo pre
toho konkrétneho pracovníka je dôležité. Pomocou pár kliknutí doslova "za jazdy" sa má mať pracovník
možnosť dozvedieť, či systém, za ktorý je zodpovedný, je v poruche a kde presne a akého charakteru
porucha je.

Notifikácie majú byť konfigurovateľné individuálne podľa užívateľa, avšak toto je funkcia servera.
Aplikácia zabezpečuje len zobrazenie notifikácie za každých okolností (aj pri behu aplikácie
na pozadí a úspornom režime Android zariadenia pripojeného do Internetu len prostredníctvom mobilnej siete)
a umožňuje užívateľom potvrdiť prijatie notifikácie. Takto môže byť stredný manažment notifikovaný napr.
aj o uplynutí dlhej doby bez potvrdenia notifikácie o poruche zariadenia servisným pracovníkom.

Komponenty a termíny
--------------------

.. _locality-status:

Stanica / lokalita
~~~~~~~~~~~~~~~~~~

Stanica alebo lokalita je konkrétna elektrická stanica alebo stavba, na ktorej jeden server dohľaduje
súbor zariadení a objektov.

Medzi charakteristické a definované vlastnosti stanice patria:

- názov
- IP adresa servra
- verejný port
- verejný hostname / IP adresa
- zemepisná šírka a dĺžka
- celkový stav stanice


Stanica ( a neskôr aj technológie na tejto stanici) má primárne tieto stavy:

:ok:      stanica je v poriadku, všetky dohľadované objekty a zariadenia pracuju správne (rovnako majú stav *ok*)
:warning: min. jedna technológia alebo objekt je v najvyššom stave *warning*, inak je vsetko *ok*
:error:   min. jedna technológia alebo objekt je v najvyššom stave *error*, inak je vsetko je *ok* alebo *warning*
:offline: stanica je nedostupná


Zariadenie
~~~~~~~~~~

Zariadenie je primárne technológia, ktorá na príslušnej stanici plní svoju funkciu a dohľadový systém toto zariadenie
monitoruje a dohľaduje jeho aktuálny prevádzkový stav.


.. csv-table:: Prehľad aktuálne dohľadovaných technológii a protokolov
    :header-rows: 1
    :widths: 15, 2000

    "Technológia","Protokoly"
    Switche,"SNMP"
    Záložné zdroje,"SNMP, Modbus"
    NVR systémy,"HTTP, ICMP"
    Kamery,"HTTP, ICMP"
    Zabezpečovacie systémy,"Proprietárny protokol"
    Klimatizácie,Modbus
    Elektromery,Modbus
    Motor generátory,Modbus

Objekt
~~~~~~

Zabezpečovacie zariadenia dohľadujú stanicu a monitorujú priestory, objekty a rovnako aj technológie. Jednotlivé
zóny sa skladajú do logických skupín, ktoré sa nazývajú objekty.

Aplikácia aktuálne rozlišuje tieto typy objektov:

- domčeky ochrany
- budovy spoločných priestorov
- tampere
- hlavné a vedľajšie brány
- perimeter


Návrh skeletonu aplikácie
-------------------------

Prihlasovacia obrazovka
~~~~~~~~~~~~~~~~~~~~~~~

Tvorí ju prihlasovací formulár a logo SEPS-u. Formulár obsahuje tieto komponenty:

- input field pre prihlasovacie meno
- input field pre heslo
- checkbox pre zapamätanie si hesla
- button na prihlásenie

Po úspešnom autorizovaní sa zobrazuje Dashboard.


Dashboard
~~~~~~~~~

Primárne obsahuje mapu Slovenska, na ktorej sú geograficky presne umiestnené stanice a ich stavy. Spolu s mapou je
na dashboarde umiestnený súhrnný stav všetkých staníc.

Dotykom na mapu sa zobrazí prehľad staníc.

Prehľad staníc
~~~~~~~~~~~~~~

Tvorí ju list všetkých staníc. List obsahuje názov stanice a jej stav. Pri tomto liste je dôraz na prehľadnosť,
teda ideálne je zoznam agregovať podľa identického stavu staníc s tým, že stanice v *errore* sú umiestnené ako
prvé, stanice v stave *ok* posledné.

Dotykom na stanicu sa posúvame do detailu stanice.


Prehľad stanice
~~~~~~~~~~~~~~~

Obdobný pohľad ako prehľad staníc, obsahom ale už sú detaily konkrétnej stanice. Pre predpokladanú početnosť
technológií a objektov je ideálne agregovať podľa typu, teda sumárny stav všetkých kamier, sumárny stav všetkých
switchov atď. Samostatnú skupinu so sumárnym stavom by mali tvoriť aj zabezpečované objekty.

Prehľad typu technológie alebo objektov
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Prehľad už všetkých zariadení daného typu, teda napríklad po výbere v predch. pohľade *Kamery* tento pohľad
už zobrazuje list všetkých kamier, agregovaných podľa stavu, teda najprv skupina kamier v stave *error* atď.

Po výbere zariadenia posledný pohľad v tomto strome a to je detail zariadenia.

Detail zariadenia
~~~~~~~~~~~~~~~~~

Posledný pohľad je detail zariadenia. Zariadenia sú alebo tvorené jednoduchšou datovou štruktúrou alebo zložitejšou,
môže ísť až o 3-úrovňový strom.

Príkladom jednoduchého zariadenia je kamera, ktorá pozostáva z takýchto parametrov:

:name: názov kamery
:ipaddress: IP adresa kamery
:position_index: pozícia kamery na prehľadovej schéme
:status: stav kamery
:type: typ kamery
:has_http: kamera je testovaná cez HTTP protokol
:use_ping: kameta je testovaná cez ICMP a ping

                Príkladom zložitejšieho zariadnia je napr. switch, ktorý okrem parametrov switcha má aj 0..N portov a
                každý port má rovnako svoje parametre.

                Parametre switcha:

:name: názov kamery
:ipaddress: IP adresa kamery
:position_index: pozícia kamery na prehľadovej schéme
:status: stav kamery
:poePortsNumber: počet portov
:deviceStatus: stav zariadenia
:networkIp: network IP

                A parametre každého portu:

:description: popis
:is_masked: maskovanie portu
:poePortCurrent: prúd
:poePortEnable: port aktívny
:poePortIndex: index portu
:poePortStatus: status portu
:poePortVoltage: napätie
:portLinkStatus: stav link portu


Operácie na vybraných objektoch a technológiách
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Vybrané technológie alebo objekty umožňujú užívateľovi cez Android aplikáciu zadávať operácie a s technoóliou
alebo objektom pracovať. Jedná sa o definovanú operáciu, ktorá vyžaduje vytvoriť **príkaz**, zadať ho cez API *POST*
requestom a následne testovať definovaný čas či sa operáciu podarilo vykonať.

Napr. pre PoE porty switchov je potrebné vybraný port **zapnúť** alebo **vypnúť**. Rovnako objekty
Galaxy ústredne je potrebné **zaarmovať**, **odarmovať** alebo **potvrdiť poplach**.

Push notifikácie
~~~~~~~~~~~~~~~~

Servre poskytujú cez API a **Websockets push notifikácie** pre Eventy. Aplikácia teda v pozadí musí
držať permanentné spojenie s API a po obdržaní PUSH eventu cez Websockets si cez API vyžiada
podrobné informácie o vzniknutom Evente a následne o tom cez Android notifikácie upozorňuje
uživateľa.

Pre notifikácie je potrebné vytvoriť aj mechanizmus kvitovania / potvrdzovania Eventov, ktorý bude zrealizovaný
HTTP requestom typu PATCH al. PUT do API endpointu eventu.

Prehľad eventov
~~~~~~~~~~~~~~~

Časová os Eventov, ktoré vznikli na príslušnej stanici, ich detail a stav ako aj stav kvitovania.


Nastavenia
~~~~~~~~~~

Aplikácia umožní uživateľovi upraviť, doplniť alebo zmeniť zoznam staníc, odhlásiť sa. Ďalej pre jednotlivé obrazovky
definované v bodoch 2.3.3 až  2.3.5 je potrebné umožniť užívateľovi filtrovanie a triedenie záznamov.

Požiadavky na vizuál a UI
-------------------------

Aplikácia musí spĺňať požiadavky na vizuál adekvátny aktuálnym trendom pri vývoji moderných mobilných aplikácii
pre Android. Mala by byť tvorená v zmysle definície
`Material Designu <https://www.google.com/design/spec/material-design/introduction.html>`_ od Google a podporovať
Android OS 4.0 a vyššie.

Distribúcia aplikácie
---------------------

Aplikácia je určená pre úzky okruh ľudí a pracuje s citlivými údajmi. Nebude distribuovaná pre verejnosť,
práve naopak, bude potrebné mať pod kontrolou, kto si bude môcť aplikáciu nainštalovať. Preto
je potrebné klientovi pripraviť riešenie distribúcie aplikácie za týchto podmienok izolované
od verejného Intenetu, aby používatelia s už nainštalovanou aplikáciou mali možnosť ľahko ale bezpečne
aplikáciu aktualizovať a flexibilne tak sledovať jej vývoj.
