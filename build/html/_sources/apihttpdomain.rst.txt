Základný popis HTTP RESTful API
===============================

Úvod
----

Android aplikácia komunikuje s decentralizovanými servrami pomocou HTTP RESTful API. API beží na každej
stanici. Preto aplikácia podľa potreby komunikuje s konkretnym servrom. Časť informácii sa pomocou
synchronizácie údajov nachádza na každej stanici. Pre príklad, pre účely autorizácie stačí aplikácii
nadviazať spojenie s jedinou stanicou. API tejto stanice poskytne informácie o tejto stanici, ale aj
celkový stav ostatných staníc. Následne po výbere inej stanice už Android aplikácia musí začať komunikovať
s danou stanicou priamo a s jej API, z ktorého získava informácie o jej zariadeniach, objektoch a eventoch.

V aplikácii je zoznam staníc, verejná IP adresa / hostname a verejný port. Po úspešnom nadviazaní spojenia
s prvou stanicou tá poskytne aktuálny zoznam staníc a aplikácia tak možno objaví stanicu, ktorú nemá
v svojich nastaveniach uloženú, doplní si ju podľa API a začne komunikovať aj s touto novou stanicou.

Rovnako by si aplikácia mala po nadviazaní spojenia zaktualizovať údaje o ostatných staniciach, teda
ich verejnú IP adresu, port, názov atď.


Šifrovanie SSL komunikácie
--------------------------

Spojenie s HTTP RESTful API je šifrované HTTPS protokolom. SSL certifikát nie je podpísaný štandardnou
autorizačnou autoritou. Pre dodatočné zvýšenie bezpečnosti komunikácie sa podarí spojenie vytvoriť jedine
za podmienky, ze klient (Android aplikácia) pre nadviazanie spojenia použije *client.key*, ktorý bol
vygenerovaný pri vytvorení certifikátu.

Preto je potrebné pre komunikáciu s API integrovať funkcionalitu, ktorá umožní vytvoriť spojenie
s použitím Openssl *client.key*.

Ak je pokus o vytvorenie spojenia bez tohto kľúča alebo s nesprávnym kľúčom, webserver danej stanice a
daného servra tento request **odmietne**.


Použité metódy a dátová vrstva
------------------------------

API primárne využíva pre endpointy HTTP metódy **GET**, vytváranie nových záznamov alebo ich aktualizáciu
sa používa **POST**, **PUT** alebo **PATCH**.

Dáta v požiadavkách aj odpovediach sú zapúzdrené pomocou formátu **JSON**.

Pre definíciu autorizácie sa používa **Authorization** v hlavičke requestu.

Autentifikácia
--------------

Po úspešnom vytvorení HTTPS spojenia musí request obsahovať korektnú API autentifikáciu. Aktuálne
podporuje API dva druhy autentifikácie.


ApiKey autentifikácia
~~~~~~~~~~~~~~~~~~~~~

Pri tomto type autentifikácie sa v hlavičke nachádza jednotný klúč, ktorým je možné sa autorizovať na všetky
servre. Tento spósob je vhodný na testovanie API a nie je určený pre finálne produkčné nasadenie.

*Ukážka requestu*

.. sourcecode:: http

   GET /api/v1/user/ HTTP/1.1
   Host: 1.2.3.4
   Authorization: ApiKey user@example.com:m3h8hclq36kmbrz2n5j5ln8r8l7rx8744

Uživateľská autentifikácia
~~~~~~~~~~~~~~~~~~~~~~~~~~

Tento typ autentifikácie je určený pre produkčné nasadenie. Je tvorené prihlasovacím menom a heslom
z prihlasovacieho formulára z prvej obrazovky.

*Ukážka requestu*

.. sourcecode:: http

   GET /api/v1/user/ HTTP/1.1
   Host: 1.2.3.4
   Authorization: Basic aaf02698a36500

kde hash *aaf02698a36500* je vygenerovaný ako `base64_encode("user@example.com:secret_password")`.


Stručný prehľad API endpointov
------------------------------

.. csv-table:: Prehľad API endpointov a ich kategória
    :header-rows: 1

    "Popis","URI","Kategória"
    "Simple schéma pre stanicu ABBA","/api/v1/abba-simple/","Schémy"
    "Detail schéma pre stanicu ABBA","/api/v1/abba-detail/","Schémy"
    "Schéma pre IMS stanice","/api/v1/abba-universal/","Schémy"
    "Kamery","/api/v1/camera/","Technológie"
    "Elektrometre","/api/v1/electrometer/","Technológie"
    "Galaxy ústredne","/api/v1/galaxy/","Technológie"
    "Galaxy ústredne","/api/v1/galaxy-full/","Technológie"
    "Galaxy ústredne","/api/v1/galaxy-group-full/","Technológie"
    "Galaxy ústredne","/api/v1/galaxy-pseudo-notification/","Technológie"
    "Galaxy ústredne","/api/v1/galaxy-zone/","Technológie"
    "Klimatizáciey","/api/v1/klima/","Technológie"
    "Linksys switche","/api/v1/linksys/","Technológie"
    "Metel switche","/api/v1/metel/","Technológie"
    "Metel switche","/api/v1/metel-full/","Technológie"
    "Metel switche","/api/v1/poe-port/","Technológie"
    "Motor generátory","/api/v1/motogen/","Technológie"
    "NVR","/api/v1/nvr/","Technológie"
    "Riello záložné zdroje","/api/v1/riello-ups/","Technológie"
    "Technoware záložné zdroje","/api/v1/ups/","Technológie"
    "ABBA alarmy","/api/v1/ups-alarm/","Technológie"
    "ABBA rôzne","/api/v1/ups-misc/","Technológie"
    "Eventy","/api/v1/event/","Udalosti"
    "Preklady","/api/v1/field-translation/","Systémové"
    "MAR parametre","/api/v1/mar-description/","Systémové"
    "Skupiny","/api/v1/group/","Logické"
    "Stanice","/api/v1/locality/","Logické"
    "Uživatelia","/api/v1/user/","Uživatelia"
    "Monitoring","/api/v1/monitoring/","Systémové"
