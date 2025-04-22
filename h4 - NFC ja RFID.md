# NFC ja RFID

## Near-field communication (NFC)

Hyödyntää RFID-tekniikkaa laitteiden tunnistamiseen ja tiedonsiirtoon hyvin lyhyille etäisyyksille (muutamia senttimetrejä). NFC-laite voi toimia sekä lukijana, että tunnisteena (esim. älypuhelin voi lukea tai esittää NFC-tunnisteen). 
Kaksisuuntainen tiedonsiirto erottaa NFC:n perinteisestä RFID:stä. 

Yleisiä: 

- Matkapuhelimissa
  - NFC yhteyden avulla voidaan parittaa kaksi laitetta, jonka jälkeen data voidaan siirtää turvallisesti esimerkiksi bluetooth-yhteyden avulla
- Pankkikorteissa
  - NFC muotoista tiedonsiirtoa käytetään maksukortin, maksutägin ja maksuterminaalin välisessä kontaktittomassa tiedonsiirrossa
- Lähimaksaminen puhelimella
  - Maksusovellukset käyttävät `NFC Card Emulation`- tilaa, jossa matkapuhelin esittää korttia ja maksupääte toimii lukijana

## Käytössäni olevia RFID tuotteita ja niiden suojaus

- Pankkikortit
  - NFC
- Passi
  - Basic Access Control, eli sirua ei voi lukea ilman, että lukee tietoja fyysisestä passista. Näiden tietojen avulla lukija luo avaimen, jonka avulla muodostetaan yhteys siruun.
- Apple Pay
  - NFC

Käyttämäni RFID-tuotteet ovat riittävän hyvin suojattu urkintaa vastaan. 

---

## APDU komento

APDU - Application Protocol Data Units

- Älykorttien ja lukijan välinen kommunikointi
- ISO/IEC 7816

APDU komennot jaetaan kahteen osaan: C-APDU (Command APDU) ja R-APDU (Response APDU)

### C-APDU

Komento, jonka lukija lähettää kortille

Rakenne: 

- Pakollinen header
  - 4 tavua
  - sisältää 4 osaa
    - CLA
      - Class Byte, Määrittelee komennon tyypin
    - INS
      - Instruction Byte, Määrittelee tarkan komennon
    - P1 & P2
      - Parameter Byte, Määrittelee mahdolliset parametrit komennolle
- Lc
  - Datakentän pituus (jos datakenttä olemassa) 
- Data
  - Dataa, `0 - 65 535 tavua`
- Le
  - Expected Length, määrittelee vastauksen koon tavuissa

### R-APDU

Komento, jonka kortti lähettää lukijalle vastauksena C-APDU komentoon

Rakenne: 

- Data
  - Response Data, `0 - 65 536 tavua`
- SW1 & SW2
  - Komennon prosessointi status, heksadesimaali arvo
 
---

## RFID hakkerointi uutinen

### Hardware Backdoor Discovered in RFID Cards Used in Hotels and Offices Worldwide

22.8.2024 Ravie Lakshmananin kirjoittamassa artikkelissa, joka julkaistiin The Hacker News -verkkosivustolla, kerrotaan takaporttihaavoittuvuudesta, joka on löydetty MIFARE Classic FM11RF08S -korttimallista.
Haavoittuvuuden seurauksena hyökkääjä voi murtaa kaikki avaimet, vaikka ne olisivat yksilöityjä. Kortit voidaan kloonata, mikäli niihin saadaan vain muutaman minuutin fyysinen yhteys.
Kyseisiä kortteja käytetään laajasti hotelleissa ja toimistoissa Yhdysvalloissa, Euroopassa ja Intiassa.
Riskinä on myös mahdollinen toimitusketjuhyökkäys, mikä tarkoittaa tilannetta, jossa kaikki kortit ovat jo valmiiksi saastuneita ennen kuin ne edes saapuvat hotelleihin.

--- 

## Lähteet

Wikipedia, Smart Card Application Protocol Data Unit, luettavissa https://en.wikipedia.org/wiki/Smart_card_application_protocol_data_unit, luettu 22.4.2025

Wikipedia, Near-field communication, luettavissa https://fi.wikipedia.org/wiki/Near-field_communication, luettu 22.4.2025

The Hacker News, Ravie Lakshmanan 22.8.2024, Hardware Backdoor Discovered in RFID Cards Used in Hotels and Offices Worldwide, luettavissa https://thehackernews.com/2024/08/hardware-backdoor-discovered-in-rfid.html, luettu 22.4.2025
