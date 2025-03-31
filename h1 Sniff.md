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

![image](https://github.com/user-attachments/assets/a93c88ab-a2ec-4f48-919a-0ac58ed22b5a)

Nopealla silmäyksellä huomaa, että liikenne tapahtuu useamman verkon välillä. Kaappaus kesti kokonaisuudessaan reilut 7 sekunttia ja paketteja on yhteensä 283. 

Kaksi laitetta samassa aliverkossa, joissa ne lähettävät DNS paketteja toisilleen. Laite osoitteessa x.x.x.1 on todennäköisesti reititin ja laite osoitteessa x.x.x.7 on jokin laite verkossa 

![image](https://github.com/user-attachments/assets/072d78e1-cfe1-48aa-bc2f-e2aff92aed4e)

Yksi laite sijaitsee osoitteessa 216.58.210.164, jonka liikenne on QUIC tyyppistä.  

![image](https://github.com/user-attachments/assets/fef43f62-c350-4043-b4db-131d5a7d92df)

Laitteet osoitteissa 139.162.131.217 ja 135.181.139.209 lähettää TCP ja TLS paketteja.  

![image](https://github.com/user-attachments/assets/0721e7f1-16cf-46aa-831c-311acf72eff2)

---

##  Mitä selainta käyttäjä käyttää

Lähdin tutkimaan eri paketteja. Aluksi ajattelin, että löytäisin selaimen tiedot kätevästi DNS paketista, mutta kun olin tutkinut muutaman paketin tajusin olevani väärässä.   
Seuraavaksi yritin tutkia endpointteja, mutta en löytänyt mitään. Yritin etsiä netistä tietoa hakusanoilla "wireshark browser identification", "how to figure out browser from .pcap file" yms.,  
mutta en löytänyt mitään järkevää. Päädyin kysymään ChatGPT:ltä apua.

![image](https://github.com/user-attachments/assets/4949c397-e781-46af-8ff5-5a350b03fba9)

ChatGPT:n avulla lähdin etsimään JA3 tunnusta TLS Client hello paketista. 

![image](https://github.com/user-attachments/assets/534ab636-6591-4de7-a9bb-d2ba5b3808c5) 

![image](https://github.com/user-attachments/assets/cd76f479-3e71-40a5-bfcf-3d32ed939296)

Löysin JA3 tunnuksen, jonka jälkeen hain netistä "JA3 search" ja päädyin sivulle JA3 Zone, josta voi hakea tietoa.

![image](https://github.com/user-attachments/assets/f3f9b9b5-9748-4720-8f7b-a0ee730f4bbe)

Kuvasta käy ilmi, että selaimena käytettiin Firefoxia. 

--- 

## Minkä merkkinen verkkokortti käyttäjällä on

Verkkokortin etsimistä varten aloin tutkia MAC osoitteita. Paketin Ethernet tiedoissa lukee src ja dst MAC osoitteet. 

![image](https://github.com/user-attachments/assets/4e26fa28-e9a3-4316-90a7-76d289854252)

Seuraavaksi menin MAC Adress database verkkosivulle, jotta voin hakea tietoa MAC osoitteista. 

![image](https://github.com/user-attachments/assets/b7ebad49-35bf-4147-86b8-885b9cd8f1ae)

Verkkokortin merkki on RealtekU

![image](https://github.com/user-attachments/assets/080f59bc-de81-4ef2-b260-4c772a12fbfd)

---

## Millä weppipalvelimilla käyttäjä on surffaillut

Ensimmäinen ajatus oli tutkia DNS paketteja, joten asetan filtteriksi ```dns```. 

![image](https://github.com/user-attachments/assets/395fb549-f87e-4a1c-a32b-78be52f9e1b3)

Kuten kuvasta näkyy, käyttäjä on surffaillut googlessa, terokarvinen.com, commentero.terokarvinen.com, 
goatcounter.netlify.com, terokarvinen.goatcounter.com

---

## Analyysi omasta liikenteestä

Kaappasin kolme pakettia, joita aion analysoida.

![image](https://github.com/user-attachments/assets/df5c3c65-ce2f-41cc-a9fc-c54bae3af7c2)


--- 

## Lähteet

Karvinen Tero, Verkkoon tunkeutuminen ja tiedustelu, luettavissa: https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/, luettu 31.3.2025

Karvinen Tero, Wireshark - Getting Started, luettavissa: https://terokarvinen.com/wireshark-getting-started/, luettu 31.3.2025

Tehtävässä "Mitä selainta käyttäjä käyttää" on hyödynnetty ChatGPT 4.0-kielimallia. Syötteenä käytettiin: "I have a .pcap file and I want to find out what browser was used when sending these packages. There is no http packages, only tls."

https://mac.lc/address/52:54:00:2f:e1:e5
