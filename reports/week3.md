# Week3 report
## Prometheus käytönotto

### Johdanto

Monitoroinnin perusajatus on kerätä dataa, analysoida ja visualisoida sitä sekä sen avulla ylläpitää järjestelmiä. Mitä enemmän dataa meillä on järjestelmästä, sitä helpompi sitä on ylläpitää. Kyse ei ole pelkästään datan vilkuilusta, vaan automatisoidusta tavasta kerätä tietoa esimerkiksi CPU-kuormituksesta, vasteajoista tai virhekoodeista.

Näitä tietoja voidaan tarkastella graafisesti, ja niiden perusteella voidaan tunnistaa esimerkiksi erilaisia kaavoja, jotka auttavat ennaltaehkäisemään virheitä tai nopeuttamaan vianmääritystä. Järjestelmään voidaan myös luoda hälytyksiä poikkeustilanteiden varalta, jotta niihin voidaan reagoida nopeasti.

Myös proaktiivisuus on tärkeää: resurssien skaalausta ja tehotarpeita voidaan suunnitella hyvin, ja järjestelmän luotettavuus voidaan varmistaa.

Prometheus on avoimen lähdekoodin erittäin suosittu monitorointi- ja hälytystyökalu, jonka avulla dataa voidaan kerätä ja selata helposti PromQL-kyselykielen avulla. Kyselykielellä voidaan näyttää dataa sekä laskea esimerkiksi keskiarvoja tai prosentteja.

### Ympäristön rakentaminen

Asensimme ohjeiden mukaan Node Exporterin, joka toimii tiedonkerääjänä sellaisille tiedoille, joita Prometheus ei pysty suoraan lukemaan. Prometheus lukee Node Exporterin keräämän datan. Tämän jälkeen otettiin käyttöön Grafana, joka lukee Prometheuksen dataa, jotta voimme määrittää sen visuaalisen esitystavan eikä pelkkien numerorivien lukeminen ole tarpeen.

Tässä kohtaa minulla oli hieman ongelmia Node Exporterin zip-tiedoston avaamisessa web1-kontissa, mutta tekoälyn kanssa tehdyn vianmäärityksen (troubleshooting) jälkeen ja katsottuani videoltasi kikan, jossa Node Exporter asennetaan Ansiblella kaikille tarvittaville konteille, sain sen lopulta toimimaan.

### Mittareiden kerääminen

Alun perin oli tarkoitus tarkastella vain web1-konttia, mutta kun asensin sen kaikille konteille, Grafana täyttyi useilla streameillä. Tämä ei kuitenkaan ollut ongelma, sillä testeissä oikeat piikit nousi esiin ja ohjeissa määriteltyjen mittareiden löytäminen oli helppoa. Kyseiset mittarit olivat CPU usage %, Memory Usage %, Disk Usage %, Network Receive ja Network Transmit. Kun käynnistin ympäristön uudelleen ja käytössä oli pelkkä web1-kontin Node Exporter, Grafanan graafien tarkastelu oli vieläkin helpompaa.

### Prometheus-kyselyt

(node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes) / node_memory_MemTotal_bytes * 100 = Memory Usage % 

100 - (avg by(instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100) = Cpu usage % 


rate(node_network_receive_bytes_total[5m]) = Network receive




### Grafana-näkymät
Katso kuvakaapaus.

Selvästi näkyy milloin CPU on lähtenyt tekemään töitä. Ja sit riipuu miten patternit katsotaan koska näkymä pystyi vaihtaamaan aika tarkaa niin miten 35% vaikkuttaa 5tunnilla verattu 5 minutilla voi olla aika iso ero,. 

### Snmp vs Prometheus

|          **Ominaisuus**         	|                                        **Snmp**                                        	|                                                          **Prometheus**                                                          	|
|:-------------------------------:	|:--------------------------------------------------------------------------------------:	|:--------------------------------------------------------------------------------------------------------------------------------:	|
| **Tiedonkeruu**                 	| Kysellään laitteilta yksitellen tai laitteet itse ilmoittavat ongelmista.              	| käy säännöllisesti hakemassa valmista mittaritietoa sovellusten ja palvelimien verkkosivuilta.                                   	|
| **Käyttöönotto**                	| Toimii suoraan monissa verkkolaitteissa, mutta asetukset pitää usein kirjoittaa käsin. 	| Vaatii yleensä pienen apuohjelman (kuten Node Exporterin) palvelimelle, mutta pilvessä se löytää uudet laitteet automaattisesti. 	|
| **Mittarien määrä**             	| Perustuu kiinteään listaan.                                                            	| Todella tehokas, pystyy seuraamaan miljoonia eri asioita ja pilkkomaan niitä tarkkoihin osi                                      	|
| **Visualisointi**               	| Vanhoja ja vähän kankeita valvontanäkymiä                                              	| Erittäin siistit, modernit ja elävät kuvaajat Grafaanan avulla                                                                   	|
| **Hälytysmahdollisuudet**       	| Vähän kömpelöt ja vanhanaikaiset ilmoitukset.                                          	| Osaa ryhmitellä hälytykset, hiljentää turhat ja lähettää oikeat viestit suoraan esim. Slackiin tai puhelimeen.                   	|
| **Soveltus pilviympäristöihin** 	| Perinteisiin, kiinteisiin yritysverkkoihin (reitittimet, kytkimet, vanhat palvelimet). 	| Nykyaikaisiin pilvipalveluihin, dockereihin ja jatkuvasti muuttuviin ohjelmistoihin.                                             	|

### Havaintoja monitoroinnista
Tei se johto päätöksen että järjestelmä kestää suht pitkään ennen kun se asentuu neutraalille. Voisin kuvitella että on aika monta processejä jokaisessa bootissa joka tuotaa tietynlaisen stressi järjestelmään ja ei voidaan ikinä tehdä johto päätöksiä ekat minuteihen perustella. 

### Pohdinta
Mitä opit tehtävän aikana?
Opin perusteet work flow node-e prometheusksen grafanaan. Opin että on olemassa oma kieli siihen dataan läpikäymiseen ja opin että usein tehdään valmiita kaavoja joka lasketaan ei vaan katsotaan raaka dataa.
Mitkä ovat Prometheuksen vahvuudet?
Helppo käytöisyys, Open source mutta myös sitä että sen pysty helposti inteigroida eri järjestelmii ja voi esim lähettää viestiä jos on ongelma tai näyttää graafisesti grafaanalla dataa.

Mitä haasteita tai rajoituksia ratkaisussa on?
Palomuurit ja nat verkoissa voi olla om haaste saada läpi dataa. 


### Tekoäly selvennys.
Olen käyttänyt google gemini ongelmaselvennys tilanteessa node exporterin kanssa ja myös korjamaan minun huono suomenkieli. 