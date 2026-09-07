# Week1 report
## Verkon dokumentointi

*käytetty gemini oppimaan Markdown ja git workflow luomassa tämä tiedosto*

### Johdanto
Tehtävän anto oli dokumentoida ja testaa verkon. Verkon rakenne on selvä neljään suuntaan. R2 toimii sydämenä koko operatioon ja neljä jalkaa ovat Etä työ verkko, työasema verkko, management verkko ja palvelin verkko. 

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


**Tehtävä 1.4**

1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
63: eth0@if64: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether 02:42:ac:14:14:06 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.20.20.6/24 brd 172.20.20.255 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:acff:fe14:1406/64 scope link
       valid_lft forever preferred_lft forever
92: eth1@if91: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9500 qdisc noqueue state UP group default
    link/ether aa:c1:ab:14:1c:27 brd ff:ff:ff:ff:ff:ff link-netnsid 1
    altname clab-o-05031180f95d8850
    inet 10.10.10.101/24 scope global eth1
       valid_lft forever preferred_lft forever
    inet6 fe80::a8c1:abff:fe14:1c27/64 scope link
       valid_lft forever preferred_lft forever


       root@client1:/# ip route
default via 10.10.10.1 dev eth1
10.10.10.0/24 dev eth1 proto kernel scope link src 10.10.10.101
172.20.20.0/24 dev eth0 proto kernel scope link src 172.20.20.6
root@client1:/#


root@client1:/# ping -c 4 10.10.20.101
PING 10.10.20.101 (10.10.20.101) 56(84) bytes of data.
64 bytes from 10.10.20.101: icmp_seq=1 ttl=62 time=0.099 ms
64 bytes from 10.10.20.101: icmp_seq=2 ttl=62 time=0.105 ms
64 bytes from 10.10.20.101: icmp_seq=3 ttl=62 time=0.081 ms
64 bytes from 10.10.20.101: icmp_seq=4 ttl=62 time=0.086 ms

--- 10.10.20.101 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 2997ms
rtt min/avg/max/mdev = 0.081/0.092/0.105/0.009 ms


root@client1:/# ping -c 4 10.10.30.101
PING 10.10.30.101 (10.10.30.101) 56(84) bytes of data.
64 bytes from 10.10.30.101: icmp_seq=1 ttl=61 time=0.155 ms
64 bytes from 10.10.30.101: icmp_seq=2 ttl=61 time=0.075 ms
64 bytes from 10.10.30.101: icmp_seq=3 ttl=61 time=0.072 ms
64 bytes from 10.10.30.101: icmp_seq=4 ttl=61 time=0.074 ms

--- 10.10.30.101 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 2997ms
rtt min/avg/max/mdev = 0.072/0.094/0.155/0.035 ms

root@client1:/# traceroute 10.10.30.101
traceroute to 10.10.30.101 (10.10.30.101), 30 hops max, 60 byte packets
 1  10.10.10.1 (10.10.10.1)  0.570 ms  0.464 ms  0.443 ms
 2  10.255.12.2 (10.255.12.2)  0.426 ms  0.393 ms  0.373 ms
 3  10.255.23.2 (10.255.23.2)  0.352 ms  0.317 ms  0.292 ms
 4  10.10.30.101 (10.10.30.101)  0.266 ms  0.225 ms  0.194 ms


- Verkko yhteydeyt löytyi.
- Eka ping kulkee R1-R2-Srv Br
- Toinen ping kulkee R1-R2-R3 joka traceroute näyttää


### Yhteenveto 
Dokumentaatiossa meni eniten aika verkkokaavion luomiseen koska tein se ensin ja selvitin kaikki ip't ja verkkon kun loin sen. Mutta se oli tosi arvokas lopputehtävään koska kaikki muut tehtävän kohdat tein sen perustella. Uskon että tämmönen visualinen kartaa helpottaa hahmottaa verkko tosi hyvin ja helpottaa reitien löytäminen. 
