# Sniff

---

## Tiivistelmät

### Karvinen 2025: Wireshark - Getting Started

Karvisen kirjoittamassa tutoriaalissa käydään läpi Wireshark snifferin asennus Linuxille ja perusasiat työkalun käytöstä.  Aluksi näytetään miten Wiresharkin voi ladata komentorivin kautta, ja mitä täytyy tehdä ennen kuin sitä voi käyttää.  Tutoriaali etenee vaiheittain ja käyttää kuvia havainnollistamaan prosessia, jotta lukija pystyy kaappaamaan  ja analysoimaan ensimmäiset pakettinsa.

### Karvinen 2025: Network Interface Names on Linux

Tässä oppimateriaalissa Karvinen käsittelee Linux käyttöjärjestelmän verkkoliittymien käytettyjä lyhenteitä.
Hän kertoo mihin nimeämiskäytännöt perustuvat, esimerkiksi en = Wired Ethernet ja wl = WLAN. Oppimateriaalissa katsotaan myös oman järjestelmän käyttöliittymien nimiä komennoilla ``ip a`` ja ```ìp route```.

--- 

## Asenna Linux virtuaalikoneeseen 

![image](https://github.com/user-attachments/assets/d80d5413-8c78-4b8f-a921-ef0bb80c597c) 

✅

---

## Internet yhteyden katkaisu

![image](https://github.com/user-attachments/assets/d579cc1d-6aa2-4e97-b0ee-2b1e4f159511)

--

![image](https://github.com/user-attachments/assets/256cb46c-1340-4ce8-8540-777660e4b136)

--- 

## Wireshark asennus ja kokeilu

Asensin Wiresharkin seuraamalla Karvisen oppimateriaalia "Wireshark - Getting Started". Ajoin komennot: 
```
sudo apt-get update
sudo apt-get install wireshark
```
"Should non-superusers be able to capture packets?" - Yes. 

![image](https://github.com/user-attachments/assets/e0cfccfe-06ee-4e77-a827-4aefde037cdc)  
(Karvinen, 2025) 

Seuraavaksi ajoin komennon  ```sudo adduser joelb wireshark```, jotta voin käyttää wiresharkkia tällä käyttäjällä. 

Nyt Wireshark on asennettu ja voin käynnistää GUI:n komennolla ```wireshark```. 

![image](https://github.com/user-attachments/assets/f1eddff4-3ba8-4d01-a0e0-fbc32174db3c)

Valitaan valikosta "any", sillä sen avulla voimme kaapata kaikista verkkoliittymistä (Karvinen, 2025). 

![image](https://github.com/user-attachments/assets/4ead8dd6-3e89-42a4-a85e-8bc30b4e19a3)

Kuvasta näkyy siepattua liikennettä. 

---

## TCP/IP-mallin neljä kerrosta

Kaappasin liikennettä hetken, kunnes sinne ilmestyi paketti jonka "Protocol" on TCP.  

![image](https://github.com/user-attachments/assets/4386281f-7513-4c45-8b6d-7ac51c33cf23)

Valitsin paketin ja klikkasin valikosta `Statistics` -> `Protocol Hierarchy`, joka avaa uuden ikkunan. 

![image](https://github.com/user-attachments/assets/7cbf28bd-793f-4f42-beeb-2633aa62f215)

Kuvankaappauksesta näkee TCP/IP-mallin neljä kerrosta: 

- Application layer : Transport Layer Security (TLS) 
- Transport layer : Transmission Control Protocol (TCP) 
- Internet layer : Internet Protocol Version 4 (IPv4)
- Link layer : Ethernet

---

## Mitäs tuli surffattua



--- 

## Lähteet

Karvinen Tero, Wireshark - Getting Started, luettavissa: https://terokarvinen.com/wireshark-getting-started/, luettu 31.3.2025

