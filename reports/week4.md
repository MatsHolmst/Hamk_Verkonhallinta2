# Johdanto

Infrastructure as Code on tapaa hallita ja otaan käytöön laiteita. Se toimii silleen että olet luonut koodi tiedostoja joka hoitaa asioita. Silloin sinä et tarvitsee käsin naputella sisään kaikki konfiguraatiot ja asennukset. Se varmistaa myös että asennus on joka kerta samalainen ja skaalaus on paljon nopempi ja helpompi. 

# Inventory

## Tehtävä 4.1 
### Mitä laitteita inventory sisältää:
Inventorysta löytyi kaikki laiteet johon Ansible voi koskea ja niiden SSH salasanat että niitä voidaan tehdää muutoksia: 
### Miten ryhmät on muodostettu:
On olemassa erilaiset ryhmätykset. Oli laitetyypit kuin Reittimet ja käyttäjä koneet. Oli ryhmiä sovelluksen tarpeiden mukaan esim semmoset joka tarvii node exporter että voidaan valvoa. Ja oli myös oma ryhmä palvelilimeen. Eli ryhmätykset oli tehty monen eri tarpeiden mukaan. 
### Mitä hyötyä ryhmistä on:
No jos on hyvin ryhmitetty niin on helpompii valita ryhmä riipuen tarpeiden mukaan ja ei tarvii jokainen kone valitsee itse. Skaalauksessaa on myös hyötyä koska koneet voi liittyä nopeasti ryhmään ja saada samat asennukset kun jo olevat laitteet. 

# Esimerkkiplaybookit

## Tehtävä 4.2
### Mitä playbook tekee:
Se on vähän kun tähdätty skripti, Sille määritetään tietty ryhmä tai host inventorysta ja sit se ajaa kommenot pelikirjassa niille laiteille. Tämä playbook pingasi kaikki laiteet listaalla.
### Mitä tuloksista voidaan päätellä:
Että kaikki laiteeti saatin yhteyttä paitsi 5 joka oli management dockerit johon ei pitänytkään saada pingi. 
## Tehtävä 4.3 
Tutki molemmat tiedostot ja vastaa:

### mitä moduuleja playbookeissa käytetään (esim. `apt`, `copy`, `service`, `get_url`, `unarchive`):
apt, file, get_url, unarchive, copy, shell, uri, debug, service.

### miten muuttujia (`vars`) hyödynnetään:
vars snmp_community: "The string public is the universal industry standard for read-only access. It allows a monitoring tool to read device statistics, but prevents it from changing configurations." google vastaus
vars: node_exporter_version: "1.9.1" määrittää mitä versio tullaan käytämään. 

### miten `handlers`-lohkoa käytetään SNMP-playbookissa
Se käytetään käynistämään snmpd uudestaan kun on tehty muutoksia config tiedostossa. Se tapahtuu vaan kerran lopussa

 
### miksi Node Exporter -playbook ei tarvitse handleria:
Koska ei ole config tiedosto. 

## Tehtävä 4.5
Kerää tiedot:
"ansible_distribution": "Ubuntu",
"ansible_distribution_file_parsed": true,
"ansible_distribution_file_path": "/etc/os-release","ansible_distribution_file_variety": "Debian",
"ansible_distribution_major_version": "24",
"ansible_distribution_release": "noble",
"ansible_distribution_version": "24.04",
"ansible_memtotal_mb": 7901,
"ansible_processor_vcpus": 16

Ip't on kaikki eri mutta esim client1 = IPV4: 10.10.10.101



## Oma playbook
### Web-Server asennus

- name: Asenna web-palvelin web1-koneelle
  hosts: web1
  become: yes

  tasks:
    - name: Päivitä pakettilista
      apt:
        update_cache: yes
        
    - name: Asenna nginx
      apt:
        name: nginx
        state: present

    - name: Luo index.html, joka kertoo palvelimen nimen
      copy:
        dest: /var/www/html/index.html
        mode: "0644"
        content: |
          <!DOCTYPE html>
          <html lang="fi">
          <head>
            <meta charset="UTF-8">
            <title>{{ inventory_hostname }}</title>
          </head>
          <body>
            <h1(otan tän pois rapportin takia)>Tervetuloa palvelimelle {{ inventory_hostname }}</h1>
            <p>Tämä sivu on luotu Ansiblella.</p>
          </body>
          </html>
          

    - name: Käynnistä Nginx
      command: /usr/sbin/nginx
      args:
        creates: /run/nginx.pid
        

    - name: Verify Webserver responds
      uri:
        url: http://localhost
        status_code: 200
      register: result

    - name: Show verification result
      debug:
        msg: "Nginx running on {{ inventory_hostname }}"

### OUTPUT:

PLAY [Asenna web-palvelin web1-koneelle] *************************************************************************************************

TASK [Päivitä pakettilista] **************************************************************************************************************
ok: [web1]

TASK [Asenna nginx] **********************************************************************************************************************
ok: [web1]

TASK [Luo index.html, joka kertoo palvelimen nimen] **************************************************************************************
ok: [web1]

TASK [Käynnistä Nginx] *******************************************************************************************************************
changed: [web1]

TASK [Verify Webserver responds] *********************************************************************************************************
ok: [web1]

TASK [Show verification result] **********************************************************************************************************
ok: [web1] => {
    "msg": "Nginx running on web1"
}

PLAY RECAP *******************************************************************************************************************************
web1                       : ok=6    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0


### Recap:
Vähän säädön kautta sain sen toimimaan. Mutta tässä vaiheessa pakko myöntää että käytiin Gemini avuksi. Kun yritin käyttää Service module niin se antoi minulle vaan: "msg": "Service is in unknown state", "status": {}}" ja en löytänyt ratkaisu sille niin käytin koodi joka AI antoi mulle. HUOM. Ainostaan startti modulli on  suoraan AI'stä muut on itse kirjoitettu / kopiotu esimerkistä. 

# Vertailu

Automaatio voi olla hankallaa oppia mutta oli aika selvä missä meni väärin ja oli suht helppoa debugga netin kanssa. Muuten se on paljon helpompaa saada toteutettu samalaiset järjestelmät monikertaistettu. Mutta jos haluat omalatuinen laite niin on ehkä helpompaa nakutta se suoraan. 

# Yhteenveto

Playbookin rakenne ja mistä löydän moduuleja. 
Playbookien debuggaminen
Inventoryn käyttö tarkoitus ja millä tavalla se on rakennettu.
Opin myös käyttämään Vscode tähän tarkoitukseen.  ja oli paljon helpompaa kun suoraan shellissä. 

