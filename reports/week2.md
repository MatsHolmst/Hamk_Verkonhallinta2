# Week2 report
## SNMP käytönotto

*käytetty gemini oppimaan git merge että sain päivitetty sinun uusin repo minun repoon*

### Johdanto

**Mitä on SMNP**

Jos tiedät vesi kiehuu 100C niin se on paljon helpompaa vältää että se kiehuu jos pystyt seuraa mittarilla veden asteet ja sulke lieden just kun se kiehuu. Tämä mittari on kuin SMNP. SNMP valvoo ja näyttää sinulle verkkossa olevat laiteiden tiedot ja eri mittarit. Tällä pystyt valvomaan sinun verkkossi ja helpottaa vianhaku mutta myös ennaltaehkäisä häiriöitä verkkossa. 

SNMP koostu agentistä joka on laitella joka haluat seurata ja manageri joka kerää agenteistä tiedot yhteen paikaan. 


### Asennus

Miten SNMP-agentti asennettiin?

Kirjaudu laiteelle: 
*apt install snmp snmpd -y*

Mitä konfiguraatiomuutoksia tehtiin? 

lisättiin:

view   systemonly  included   .1.3.6.1.2
että saadan kaikki tiedot näytille, muuten oli rajoitettu määrä vaan 
näytillä

### Kerätyt tiedot

**Suoritetut SNMP-kyselyt** 

Tein näitä kyselyt localhostilla koska se verkko ongelmalle ei tullut vielä vastausta. Mutta vaihtaamalla "localhost" siihen koneseen mistä haet tiedot(esim web1) niin pystyt kysymään asioita samas konesta. 

root@web1:~# snmpget -v2c -c public localhost sysName.0

root@web1:~# snmpget -v2c -c public localhost sysDescr.0

root@web1:~# snmpget -v2c -c public localhost sysUpTime.0
DISMAN-EVENT-MIB::sysUpTimeInstance = Timeticks: (14011306) 1 day, 14:55:13.06


**Keskeiset komentotulosteet ja havainnot:**

SNMPv2-MIB::sysName.0 = STRING: web1
Tarkoita että järjestelmän nimi on "web1"

SNMPv2-MIB::sysDescr.0 = STRING: Linux web1 6.18.33.2-microsoft-standard-WSL2 #1 SMP PREEMPT_DYNAMIC Thu Jun 18 21:54:43 UTC 2026 x86_64
Kuvaa järjestelmää. Esim sen on linux, on nimeltä web1 ja pyörii virtualisoitu wsl alustana


DISMAN-EVENT-MIB::sysUpTimeInstance = Timeticks: (14011306) 1 day, 14:55:13.06
Näyttää että järjestelmä on ollut ylhällä (eli käynistetty päälle) reilusti yli päivän


|     **Laite**     	|                    **Nimi**                   	|                 **Käyttöjärjestelmä**                	|                              **Uptime**                              	|
|:-----------------:	|:---------------------------------------------:	|:----------------------------------------------------:	|:--------------------------------------------------------------------:	|
| **web1**          	| SNMPv2-MIB::sysName.0 = STRING: web1          	| SNMPv2-MIB::sysDescr.0 = STRING: Linux web1          	| DISMAN-EVENT-MIB::sysUpTimeInstance = Timeticks: (206734) 0:34:27.34 	|
| **db1**           	| SNMPv2-MIB::sysName.0 = STRING: db1           	| SNMPv2-MIB::sysDescr.0 = STRING: Linux db1           	| DISMAN-EVENT-MIB::sysUpTimeInstance = Timeticks: (158866) 0:26:28.66 	|
| **branch-client** 	| SNMPv2-MIB::sysName.0 = STRING: branch-client 	| SNMPv2-MIB::sysDescr.0 = STRING: Linux branch-client 	| DISMAN-EVENT-MIB::sysUpTimeInstance = Timeticks: (14633) 0:02:26.33  	|




**Verkkorajapinnat**

SNMP:n avulla kerätyt verkkorajapintatiedot 

root@web1:/etc/snmp# snmpwalk -v2c -c public localhost ifDescr

IF-MIB::ifDescr.1 = STRING: lo

IF-MIB::ifDescr.55 = STRING: eth0

IF-MIB::ifDescr.90 = STRING: eth1



**Rajapintojen tunnistaminen ja tulkinta** 

ip addr hain ip ja huomasin että

Lo = loopback 172.0.0.1/8

eth0 on hallinta verkko 172.20.20.2/24

eth1 on verkko ulos 10.10.20.101/24




**OID-analyysi** 

Käytetyt OID-objektit:

sysName.0 järjestelmän

sysDescr.0 järjestelmän kuvaus 

sysUpTime.0 järjestelmän ylös olo aika (kuin kauan ollut päällä)

ifDescr: Verkkorajapintojen tieto

ifOperStatus: Mitkä verkkorajapintoja ovat käytössä


Saa tietoa että mitä järjestelmän nimi, kuvaa siitä ja mitkä verkkorajapintoja on. Voisin kuvitella että jos sulla on verkko hallinnassa näet ip joka vie paljon kaistaa niin voit käyttä snmpget ja ip'llä saada suoranimi ja tieto niin sieltä pääset eteenpäin tutkimaan miksi juuri se tietty kone vie paljon kaistaa. 

Pohdinta 

Mitä opit tehtävän aikana? 
Ekat joka jäi mieleen oli mibs ja oid että niillä pystyy hakemaan tiettyä asioita. Opin myös Mergeamaan meidän repot että sain sinun uudet versiot myös minuun repoon. myöskin sitä että se on tärkeä conffissa olla riitävän avaoin että saa ne tiedot ulos mitä tarviaa. Alan myös tuntea itteeni luontaisempi linuxin käyttöön ja tuntuu että liikun ja tutkin vapaasti ja ei vaan tutorialin kanssa. 


Mitkä ovat SNMP:n tärkeimmät hyödyt? 

Kun on riitävästi data järjestelmästä, onko se sitten verkko vai vaan kone, niin voi aktiivisesti seuraa mitä tapahtuu. Ja kun on riittävästi data niin alkaa nähdä patternit ja niiden avulla voi enaltaehkästä jo tulevat vaarat. Voi nopeasti paikantaa mistä kohdasta tulee häiriöitä ja pysähtää niitä ennen kun isompi ongelma syntyy ja voi myös optimoida järjestelmä että kaikki toimii paremmin. Tässä SNMP data keräily on olenainen että voi sitten siirtä data johon missä on helpompia nähdä sen 


Mitkä ovat SNMP:n rajoitukset tai haasteet? 
No manualiset kyselyt tuhansille konelle voi olla haastavaa ja myös mibs tietojen sekavuus kommentorivillä voi olla haaste. Onks miten salattu en tiedä?

Mitä tietoa SNMP:n avulla voidaan kerätä?
Verkkoliikenne, Laiteiden hardware tila (cpu kuorimitus cpu lämpö...), laiteiden tieto kun järjestelmä nimet.


Mitä ongelmia yhteisöpohjaisessa SNMPv2:ssa on?
Nopealla googlauksella on selvästi tietoturva. Data ei kryptata, ei authenticaatio. 

Missä tilanteissa käyttäisit mieluummin SNMPv3:a?
Kun haluat salasanat takaa dataa. 