# Week1 report
## Verkon dokumentointi

*käytetty gemini oppimaan Markdown ja git workflow luomassa tämä tiedosto*

**Tehtävä 1.1 – Topologian kartoitus**

### Laiteiden Tarkoitus
- R1 on reititin työaseman laiteiden reiti verkkoon
- R2 on koko verko keskus kaikki tietä johtaa tänne
- R3 on  etätoimipisteen asiakaskone 
- client on työasema
- hyökkäjä testauksen 
- web1 verkkosivustojen järjestelijä
- db1 tietokanta
- branch client etätyöasema
- ansible automaation softa
- prometheus data scraper
- grafana data visualiser
- zabbix verkkovalvonta


**Tehtävä 1.3**

- Verkko 10.10.10.0/24 | Työasema verkko  | 10.10.10.1/24
- Verkko 10.10.20.0/24 | Palvelin (web ja db) | 10.10.20.1/24
- Verkko 10.10.30.0/24 | Etätyöverkko | 10.10.30.1/24
- Verkko 10.10.99.0/24 | Hallintoverkko | 10.10.99.1/24
- Verkko 10.255.12.0/30| R2 - R1 Verkko | 10.255.12.2/30
- Verkko 10.255.23.0/30| R2 - R3 Verkko | 10.255.23.1/30


