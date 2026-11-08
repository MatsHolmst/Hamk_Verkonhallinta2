# Wireshark tehtävä

## Osa1

Kun olin käynyt sivulla niin etsin eka tcp syn ja painoin dest.address ja painoin apply as filter niin tuli:
ip.dst == 52.85.154.27  

Vastaus minulle tän kerran oli Amazon.com United States. Kartta toiminto ei toiminnut koska kartta tekijän (carto) API ei toimminut enää wiresharkin kanssa mutta löysin workarounding missä tallensin kartan html ja kävin htmlssä vaihtamassa kartta sivu pohjalle. 
https://server.arcgisonline.com/ArcGIS/rest/services/World_Street_Map/MapServer/tile/{z}/{y}/{x}


## Osa2:

1) Mitkä olivat learn.hamk.fi DNS-kyselyn vastauksen IP osoite ja authoratiiviset DNS palvelimet? Paljon on DNS kyselyn TTL aika? Termistöön liittyvä artikkeli

Käytin eka: dns.qry.name == "learn.hamk.fi". Sieltä löytyi että ip oli 195.148.239.84 / hamk.fi: type SOA, class IN, mname ns1.hamk.fi
TTL oli 64

2) TÄHÄN OTIN TUKI TEKOÄLYSTÄ: 
Käytetty protokolla on TLS 1.3
Client Hello: selain ehdotti 16 salaussarjaa:
Cipher Suite: Reserved (GREASE) (0x3a3a)
Cipher Suite: TLS_AES_128_GCM_SHA256 (0x1301)
Cipher Suite: TLS_AES_256_GCM_SHA384 (0x1302)
Cipher Suite: TLS_CHACHA20_POLY1305_SHA256 (0x1303)
Cipher Suite: TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256 (0xc02b)
Cipher Suite: TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (0xc02f)
Cipher Suite: TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384 (0xc02c)
Cipher Suite: TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (0xc030)
Cipher Suite: TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256 (0xcca9)
Cipher Suite: TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256 (0xcca8)
Cipher Suite: TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA (0xc013)
Cipher Suite: TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA (0xc014)
Cipher Suite: TLS_RSA_WITH_AES_128_GCM_SHA256 (0x009c)
Cipher Suite: TLS_RSA_WITH_AES_256_GCM_SHA384 (0x009d)
Cipher Suite: TLS_RSA_WITH_AES_128_CBC_SHA (0x002f)
Cipher Suite: TLS_RSA_WITH_AES_256_CBC_SHA (0x0035)

Server Hello: Cipher Suite: TLS_AES_128_GCM_SHA256 (0x1301)



## Osa

Filtteri joka käytin oli ihan vaan http sieltä tuli vastaan niin pieni määrä vastauksia niin silmäilin vähän niitä kunnes löysin:
Authorization: Basic TWF0czoxMjM0\r\n
    Credentials: Mats:1234

Se löyty:
692	23:15:47.297409400	4.688374400	localhost	localhost	HTTP	887	823	GET /index.html.txt HTTP/1.1 

Lisäsin repon report imagen myös pari näytön kaapauksia ja se haku tiedosto. 

