# Lempiväri: violetti

## Tiivistelmät

### Pyramid of Pain

Pyramid of Pain -malli kuvaa eri tasoja, joilla voidaan torjua tietoturvahyökkäyksiä, sekä torjunnan vaikutusta hyökkääjän toimintaan. Pyramidin alaosassa on tekijät joiden torjunta on helppoa, mutta ne eivät aiheuta merkittävää vaikutusta hyökkääjän toimintaan. Pyramidin huipulla on ne tekijät joiden torjuminen on todella vaikeaa, mutta jos torjuminen onnistuu, hyökkäystä on todella vaikea jatkaa. 

![image](https://github.com/user-attachments/assets/40fd19b4-8927-4450-bafe-d3005eaf060a)

### Diamond Model

Diamond Model on analysointimalli, jonka avulla hahmotetaan hyökkäystä ja sen osia. Malli sisältää neljä osaa: 
- Hyökkääjä (Adversary)
- Infrastruktuuri (Infrastructure)
- Kyvykkyys (Capability)
- Kohde (Target)

Mallin avulla voidaan kartoittaa miten hyökkäyksen eri osat / toimijat ovat vuorovaikutuksessa keskenään, esimerkiksi mitä resursseja hyökkääjä käyttää ja mihin kohteeseen. 

![image](https://github.com/user-attachments/assets/b2b7c5fe-e738-4cd0-b965-8ad9e074502e)

---

## Apache2

Apache2 oli jo asennettu

![image](https://github.com/user-attachments/assets/07c36561-480a-4bdc-84ad-980cf98338e2)

Mennään osoitteeseen `http://localhost`

![image](https://github.com/user-attachments/assets/cc097ece-d08a-4ee3-9cf5-f52b5efc7486)

Siirrytään oikeaan hakemistoon

![image](https://github.com/user-attachments/assets/8fbbb705-e2b0-4251-a09e-26a3bf5621b6)

`sudo tail` kommennolla saa tulostettua tiedoston viimeiset 10 riviä

![image](https://github.com/user-attachments/assets/1d6778a3-1336-4528-a520-94e442a7df47)

Analyysi:
- `127.0.0.1` - Loopback osoite
- `[07/Apr/2025:09:27:39 +0100]` - Aikaleima
- `GET / HTTP/1.1` - Selain pyytää sivua `/` HTTP/1.1 protokollalla
- `200` - HTTP statuskoodi
- `3383` - Vastaus oli 3383 tavua pitkä
- `Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0` - User-Agent

--- 

## Nmapped ja skriptit

Porttiskannataan kommennolla `sudo nmap -T4 -vv -A -p 80 localhost`

![image](https://github.com/user-attachments/assets/5b659129-ac48-4d07-89f7-0697a6168957)
![image](https://github.com/user-attachments/assets/07a4942d-bf46-4421-b5c3-e2c2557951f0)

#### Analyysi

- Portti 80 on auki HTTP pyynnöille
- Versio: Apache httpd 2.4.63 ((Debian))

#### Päälle olleet skriptit

- `http-methods`
  - `Supported Methods`
- `http-server-header`
- `http-title`
- `OS-scan`

---

## Jäljet lokeissa

Haetaan lokit

![image](https://github.com/user-attachments/assets/c64010b2-e40a-4c88-afe9-f58ff1d8f43e)

Kuten kuvasta näkyy, lokissa on pyyntöjä nmap scripting engineltä. User-Agent: `Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/book/nse.html)`

Esimerkiksi `OPTIONS` tiedustelee tuettuja HTTP metodeja, joka nähtii aiemman tehtävän tulosteessa

Jos lokitiedosto on suuri, voidaan käyttää komentoa `sudo grep -i nmap access.log`

![image](https://github.com/user-attachments/assets/d84abf2a-ee8a-4521-9fd9-48e287f1c21e)

---

## Wire sharking

Valitaan loopback ja aletaan kuuntelemaan

![image](https://github.com/user-attachments/assets/4ca8e55c-1c37-4628-88be-1e7415e5e1a3)

Uusi porttiskannaus

![image](https://github.com/user-attachments/assets/b89f6203-de12-4705-a958-346df8702423)

Wireshark kaappasin pajon paketteja

![image](https://github.com/user-attachments/assets/186aa11b-0777-48e9-9ff3-b07a25afe62b)

Lisätään filtteri `frame contains nmap`, jotta näemme paketit josta olemme kiinnostuneita

![image](https://github.com/user-attachments/assets/890256bc-cb1a-4fcc-85eb-ab3371fa12da)

Nyt nähdään paketit, joita nmap lähetti

Paketeissa on `OPTIONS`, `POST`, `GET`-metodeja sekä muita hieman harvinaisempia

---

## Net grep

Avataan yksi terminaali ja ajetaan komento `sudo ngrep -d lo -i nmap`

![image](https://github.com/user-attachments/assets/09204b25-15a9-4dae-9603-95cb882b80ed)

Avataan toinen terminaali, jossa ajetaan porttiskannaus

![image](https://github.com/user-attachments/assets/e72e7185-a5a1-445d-8d11-89711ae8e151)

Nähdään, että ensimmäiseen terminaaliin tulee paljon liikennettä, jossa on sana "nmap". 

![image](https://github.com/user-attachments/assets/0b3f8c03-de5f-4e0c-b4f2-4f516883776f)

---

## Agentti ja pienemmät jäljet

nmap:n user-agentin voi vaihtaa lisäämällä `--script-args http.useragent=""` joka muuttaa defaultin user-agentin haluamakseen. 

![image](https://github.com/user-attachments/assets/9f9dfcfd-a5a1-423d-b0bc-7f43a6ca50bc)

![image](https://github.com/user-attachments/assets/da0870ca-72f0-4cd0-b69d-a2b821907e1e)

Kun mennään katsomaan apachen lokeja huomataan, että User-Agent on nyt `Mozilla/5.0 (X11; Linux i686; rv:137.0) Gecko/20100101 Firefox/137.0` eikä tyypillinen Nmap Scripting Engine. 

Jos joku katsoo tiedostoa kommennolla `sudo grep -i nmap access.log` suurin osa riveistä on hävinnyt, mutta porttiskannaus jätti silti jälkensä.

`GET /nmaplowercheck1744103772 HTTP/1.1` Tämä on varmaan jokin skripti, joka ajetaan aina porttiskannauksen yhteydessä

---

## LoWeR cHeCk

Navigoidaan hakemistoon `usr/share/nmap/`

![image](https://github.com/user-attachments/assets/20cc53b3-048f-42ec-81af-128110e972d9)

Kävin aluksi katsomassa `/scripts` kansiosta, mutta siellä ei ole .lua tiedostoja

![image](https://github.com/user-attachments/assets/808de804-0ebc-49f3-8638-7b4efc5aa2cc)

`http.lua` näyttää lupaavalta

Avataan tiedosto `sudoedit http.lua`

![image](https://github.com/user-attachments/assets/4bffe37e-1b86-4d05-9e07-4d79bd198604)

Haetaan nimellä `lowercheck`, koska se luki rivissä jota ei saatu piiloon:
`127.0.0.1 - - [08/Apr/2025:10:16:12 +0100] "GET /nmaplowercheck1744103772 HTTP/1.1" 404 451 "-" "Mozilla/5.0 (X11; Linux i686; rv:137.0) Gecko/20100101 Firefox/137.0"`

![image](https://github.com/user-attachments/assets/d830c689-2ced-474d-b176-a7e2aca80eb4)

Testasin muokata merkkijonoa `"/nmaplowercheck" > "/lowercheck"`

![image](https://github.com/user-attachments/assets/3fa25591-dfbe-4787-9922-5c27c39e749a)

Ajoin uuden porttiskannauksen, huom. aikaleima: `2025-04-08 11:04 BST`

![image](https://github.com/user-attachments/assets/59ec0b4b-09c1-416d-b7fd-fa7e30c8dc4d)

ja kun haetaan Apachen lokeista kommennolla `sudo grep -i nmap access.log`

![image](https://github.com/user-attachments/assets/ce6ab6b9-6514-4d62-8833-bcb91dc3dd79)

Nähdään, että viimeisin rivi on kirjattu eri porttiskannauksen yhteydessä, joten muokkaus toimi ja nyt porttiskannauksesta ei jää mitään jälkeä jos hakee `grep -i nmap` kommennolla

---

## Lähteet

Karvinen Tero, Verkkoon tunkeutuminen ja tiedustelu, luettavissa: https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/, luettu 7.4.2025

Apache, Log Files, luettavissa: https://httpd.apache.org/docs/2.4/en/logs.html, luettu 7.4.2025

Detect-respond.blogspot, What is the Diamond Model of Intrusion Analysis?, luettavissa http://detect-respond.blogspot.com/2013/03/the-pyramid-of-pain.html, luettu 8.4.2025

TEAMT5, What is the Diamond Model of Intrusion Analysis? Why Does It Matter?, luettavissa https://teamt5.org/en/posts/what-is-diamond-model-of-intrusion-analysis/, luettu 8.4.2025

ECCouncil, Cybersecurity Exchange, Diamond Model Intrusion Analysis, luettavissa https://www.eccouncil.org/cybersecurity-exchange/ethical-hacking/diamond-model-intrusion-analysis/, luettu 8.4.2025
