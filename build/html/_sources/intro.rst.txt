Základný popis projektu
=======================

Tento dokument obsahuje informácie pre prácu so softwárovým produktom vyvinutým spoločnosťou Contineo.
Slúži ako podklad pre vypracovanie cenovej ponuky na dielo, jeho rozsah a obsah nenahrádza
finálný referenčný manuál k danému dielu.

Čo je "Dohľadový systém IMS staníc"
-----------------------------------

Dohľadový systém IMS staníc bol vyvinutý špeciálne pre potreby spoločnosti SEPS a.s.. Je to
komplexný dohľadový systém pozostávajúci z rôznorodých komponentov a to ako softvérových, tak
hardvérových. Jeho primárna funkcia je dohľadovať vybrané technológie a upozorniť v prípade
potreby náležité osoby o udalostiach, ako sú výpadky, nežiadúce stavy až po narušenie bezpečnosti
staníc a to ako samotným stavom, tak prienikom nežiadúcich osôb do sledovaných priestorov alebo
k sledovaným technológiám.


Decentralizácia dohľadového systému
-----------------------------------

Z dôvodu komplexnosti celej SEPS siete a umožneniu dohľadu aj v situáciách, kedy dôjde
k prerušeniu spojenia medzi jednotlivými stanicami, alebo medzi centrálnou serverovňou,
je potrebné zabezpečiť možnosť dohľadovať technológie pre manažment, riadiacich
pracovníkov na nižšej úrovni, lokálnych dispečérov až po kompetentných pracovníkov
v danej lokalite.

Ďalším používateľom dohľadového systému je spoločnosť,
ktoré zabezpečujú servis a údržbu dohľadového systému. Preto je lokálny server,
na ktorom beží dohľadový server umiestnený vždy v konkrétnej lokalite a jeho funkčnosť
nie je podmienená spojením s ostatnými stanicami, alebo s centrálnou serverovňou.
Aj v prípade straty konektivity teda lokálny server plnohodnotne dohľaduje technológie
v svojej lokalite, zaznamenáva zmeny stavov, nežiadúce udalosti a notifikuje o nich
spôsobom, ktorý je v danom momente možný.


Aplikačné servre a ich decentralizácia
--------------------------------------

V každej lokalite sa nachádza serverovská aplikácia, ktorá podľa stavu siete:

- pracuje samostatne a dohľaduje technológie v svojej sieti, alebo
- v prípade funkčnosti prepojov medzi ostatnými stanicami si vymieňa potrebné informácie s ostanými
  servrami
