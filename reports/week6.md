
## 1. Johdanto

Zabbix on avoimen Open Source valvontajärjestelmä, jota käytetään IT-infrastruktuurin tilan, suorituskyvyn ja saatavuuden reaaliaikaiseen seurantaan.

# Tehtävä 6.1 – Tutustu Zabbixiin

Tutki käyttöliittymän seuraavat osat:

### Hosts
Sisältää luettelon kaikista järjestelmään liitetyistä ja valvottavista laitteista. Laitteiden lisääminen, muokkaaminen, ryhmittely, tila-arviointi ja IP-osoitteiden tai muiden yhteystietojen hallinta.

### Templates
Mallipohjiin on määritelty valmiiksi tiettyjä laitteita koskevat mittarit.

### Monitoring
Reaaliaikaisten mittaustulosten tarkastelu, lokitietojen selaus, laitteiden aktiivisen tilan toteaminen ja poikkeamien nopea havainnointi.

### Dashboards
Visuaalisen yleisnäkymän järjestelmän kokonaistilasta. 

### Alerts
Hälytysrajojen asettaminen, aktiivisten hälytysten tarkastelu, kuittaus sekä ilmoituskanavien konffausta.

### Reports
Yhteenvetojen ja analyysien muodostamisen pidemmältä aikaväliltä.


# Tehtävä 6.5 – Valvontamittarien analyysi

### Cpu Utilization 
% Kertoo kuin paljon Prossun teho on käytetty, Ja kannattaa seurata koska sillä voidaan tietää että kone on kunossa, se voi mitata jos on tilaa skalaa ja myös jos pyörii extra paljon niin voi olla hyökkäys

### Availble Memory
GB, Kertoo kuin paljon muisti on vielä käytettävissä. Tarvii aina olla pieni katto että jos joku processi pyörii ylös niin ei vie kaikki muistit ja hidastaa clientti. Kertoo myös jos on hyökkäys tai turhia sovelluksia. 

### System Uptime
HH:MM:SS, Kertoo kuin kauan järjestelmä on ollu käynissä. Hyödyllinen tietää jos joku on esim käynistänyt järjestelmä uudestaan ilman lupaa tai jos tarvis rebootaa.

### Number of logged in users
Kpl, Kertoo kuin monta käyttäjää on sisällä järjestelmässä. Voidaan monitoroida millainen käyttö clientillä on. 

### Interface eth1: Bits received
bps, Monitoroin netti liikenteä. Pystyy katsomaan missä tarvii päivittää kaistaa mutta myös jos nousee oudosti niin voi olla hyökkäys. 

---


# Tehtävä 6.7 – Triggerien määrittäminen


## Triggeri 1

CPU-kuormitus >80%:

{web1:system.cpu.util.last()}>80 or {db1:system.cpu.util.last()}>80 or {branch-client:system.cpu.util.last()}>80

Warning

## Triggeri 2

Free disk space < 20 %
{web1:vfs.dev.util[sda].last()}<20 or {db1:vfs.dev.util[sda].last()}<20 or {branch-client:vfs.dev.util[sda].last()}<20

warning

---


# Tehtävä 6.10 – Kurssin työkalujen vertailu


### Tiedonkeruu:

SNMP: Kyselyt, ei agenttia. Aika simpeliä

Prometheus: Hakee dataa säänöllisin päätestä.

Zabbix: Agentti pohjasta tiedonkeruu



### Dashboardit:

SNMP: Ei ole oma

Prometheus: jotain pientä mutta haluat puskee sen eteenpäin

Zabbix: Kattavat omat joka on helppo muokata



### Hälytykset: 

SNMP: tosi vähän mahdollisuudet.

Prometheus. On hälytykset, ehkä haastava määrittää mutta toimii hyvin

Zabbix: monipuolisia triggers & Actionsit. Ja pystyy paritta ja and/or jne. 

### Käyttönotto:

SNMP: Helppo ja kevyt

Prometheus: Vähän haastava kun tarvii exporter

Zabbix: Vielä enemmän töitä kun tarvii servu jne

### Skaalautuvuus:

SNMP: Heikko

Prometheus: Erinomainen oikeassa miljöössä

Zabbix: Hyvä.

### Yrityskäyttö:

SNMP: Standari jossain laiteissa kuin esim kytkin

Prometheus: Konttiympäristöön

Zabbix: Kokovaltaisesti!


---

# Tehtävä 6.11 – Pohdinta

Vastaa seuraaviin kysymyksiin:

1. Mitä hyötyä keskitetystä valvonnasta on? :: 
    Löytää kaikki samasta paikkasta ja helpommin seurata ja luoda järjestelmiä ja triggerit.
    Pystyy myös helpommin seurata trendejä ja ennakoida. 
2. Mitkä mittarit ovat mielestäsi tärkeimpiä? :: 
    Kuin paljon jotain käytettään (CPU,MEMORY,Verkko) ja Virheiden määrä
3. Millaisista tilanteista ylläpitäjän pitäisi saada hälytys? :: 
    No jos on tosi outo käytös, Jos jotain on menossa kritiselle rajoille tai jotain on alhalla.
4. Missä tilanteissa käyttäisit Prometheusta? :: 
    Kontti ympäristössä grafanan kanssa
5. Missä tilanteissa käyttäisit Zabbixia? :: 
    Kun lois isompi pysyvämpi infra. 
6. Mitä valvontatoimintoja lisäisit tähän ympäristöön? :: 
    Milloin palvelut on online. Säänölliset testit. 

---