# Aaltoja harjaamassa

## Tiivistelmät

### Hubmartin 2019: Universal Radio Hacker SDR Tutorial on 433 MHz radio plugs 

Hubmartin esittelee Universal Radio Hacker-työkalun käyttöä ja radiolähetysten analysointia

Aluksi hän asettaa "spectrum analyzer" välilehdeltä taajuuden 433 MHz.

Tämän jälkeen hän nauhoittaa lähetteen ja tallentaa sen.

Hän avaa nauhoitetun tiedoston, valitsee Modulation kentästä "ASK" ja painaa "autodetect parameters"

Ohjelma onnistuu kääntämään viestin biteiksi.

### Cornelius 2022: Decode 433.92 MHz weather station data

Corneliuksen julkaisemassa tutoriaalissa hän kertoo miten hän käänteismallinsi Lidlistä ostetun sääaseman. 

Aluksi hän käytti `rtl_443` työkalua, jonka avulla hän sai luettua säätietoa, sekä myös laitteen mallin. 

Tämän jälkeen hän käytti Universal Radio Hacker työkalua, joss hän tutki signaalia manuaalisesti. Hän sai selville, että signaali on ASK moduloitua, joten nyt hän voi rakentaa toimivan järjestelmän itse.

--- 

## WebSDR

Haetaan netistä "websdr". Muistan "enschede" sivun oppitunnilta, joten mennään kuuntelemaan sinne.

![image](https://github.com/user-attachments/assets/ac378427-dc5f-414b-9f90-b0c3c3709ea5)

Painetaan "Start audio". Äänet ovat täysillä oletuksena :).

![image](https://github.com/user-attachments/assets/c631a0a4-b01f-413c-b138-7f63d2a5077f)

Lasketaan volumea ja vaihdetaan AM modulaatioon. Klikkailen vesiputousmallista kohtia, jossa näyttää tapahtuvan jotain. 

![image](https://github.com/user-attachments/assets/cc243ce2-39d2-439c-80ef-a105e1e925b0)

Vesiputoismallissa voi myös zoomata sisään, jotta näkee monia kanavia. Klikataan 

![image](https://github.com/user-attachments/assets/44d1488d-62c3-4f52-9b7c-4911f944d802)

Jään kuuntelemaan taajuutta `15.245 MHz` (AM modulaatio).

Aallonpituus kaavalla: `aallonpituus (m) = 300 / taajuus (MHz)` > `300 / 15.245 ≃ 19.68 m`

---

## rtl_443

### Asennus

Mennään `rtl_443/releases` GitHub sivulle ja valitaan uusi release (25.02) 

![image](https://github.com/user-attachments/assets/4bcbff6c-c1c7-4608-bb63-b1ea6bfff183)  

---

Ladataan sopiva .zip paketti. Minulle `rtl_433-rtlsdr-openssl3-Linux-amd64-25.02.zip`

![image](https://github.com/user-attachments/assets/7e716c89-c974-44b4-957b-d0f468d29c6b)

Asennan `atool` nimisen työkalun kommennolla `sudo apt-get -y install atool wget libssl-dev libtool libusb-1.0-0-dev librtlsdr-dev rtl-sdr libsoapysdr-dev`

Asennuksen jälkeen käynnistän koneeni uudelleen. 

![image](https://github.com/user-attachments/assets/8ac1c48e-eef1-430d-b766-c54a0dfdc399)

Puretaan ladattu .zip tiedosto komennolla `aunpack rtl*.zip`

![image](https://github.com/user-attachments/assets/b612b889-9718-459c-82ee-aae663607ddd)

---

### Automaattinen analyysi

Ladataan analysoitava tiedosto

![image](https://github.com/user-attachments/assets/d92600a2-183b-4f37-855b-8b8d9200445c)

Ajetaan komento `./rtl_433 -r Converted_433.92M_2000k.cs8`

![image](https://github.com/user-attachments/assets/bfc90280-72df-402e-aaea-04928e9f312b)

Analysointi: 
- Monta id kenttään, kaikkien arvo sama: `8785315`
- Kolmea eri model arvoa: `KlikAanKlikUit-Switch`, `Proove-Security` & `Nexa-Security House Code: 8785315 (House Code sama kuin id arvot)`
  - Arvot vaihtuvat tietyssä järjestyksessä: `Klik > Proove > Nexa`
- Joukossa liikkuu myös `Group Call`, `State` ja `Unit` arvoja, mutta en tiedä mitä nämä tarkoittavat ja mitä niistä pitäisi olla mieltä

--- 

## Too complex 16

Ladataan `.complex16s` tiedosto

![image](https://github.com/user-attachments/assets/e7395c02-a2f4-4cb0-a6d0-893fad85946f)

Tiedoston filetype on `.complex16s`, joka pitää muuttaa `.cs8`. 

- Tiedoston nimessä on tarvittavat tiedot
  - center frequency = 433.92 Mhz
  - Sample rate = 2 MSps
 
Muokataan tiedoston nimi oikeaan muotoon

![image](https://github.com/user-attachments/assets/d15bac6a-efab-4577-a953-551f3f8f2aba)

Nyt voidaan käyttää rtl_433 työkalua tiedostoon

![image](https://github.com/user-attachments/assets/69d93d45-56c3-4795-929d-37919a7d2a91)

Tiedosto näyttää identtiseltä aiemmassa tehtävän tiedostoon.

---

## URH

### Asennus

![image](https://github.com/user-attachments/assets/daf0f39a-9ba0-4a6b-a0ba-1b33cb34203e)

En päässyt eteenpäin asennuksen kanssa. Kokeilin laitailla urh Python Package Indexin ohjeiden mukaan, mutta tämäkään ei toiminut.

![image](https://github.com/user-attachments/assets/988bce86-7972-4891-834d-3c32420fc9ae)

Netistä ei löydy juuri mitään apua, joten jätän tämän tähän. 

Tässä välissä palautin tämän raportin ja aloin vertaisarvioimaan muiden raportteja. Arvioin Hobittisen raporttia, jossa hän oli onnistunut lataamaan urh ohjelman tavalla, joten en ollut kokeillut. 

Hobitti latasi urh snapd työkalun avulla: 

```
sudo apt update
sudo apt install snapd
sudo snap install snapd
sudo snap install urh
```
![image](https://github.com/user-attachments/assets/f1c8bd68-db74-4bf6-acd1-b355faaa42f6)

Tämän jälkeen hän ajoi komennon ```snap run urh```

![image](https://github.com/user-attachments/assets/2bf7c5fa-e36f-4d95-9952-b5d03ce27592)

![image](https://github.com/user-attachments/assets/e1a7b318-f64c-4e1a-a994-c40f89d0008a)

![image](https://github.com/user-attachments/assets/f63d7d64-0efc-48fa-ab00-6d5c346a6b87)

### Yleiskuva

Ladataan tiedosto

![image](https://github.com/user-attachments/assets/48c374de-3090-4818-9178-21c9ad1f5d02)

Avataan ladattu tiedosto urh ohjelmassa

![image](https://github.com/user-attachments/assets/76edd069-9997-4a39-b2a2-9eada40bfc2f)

Jos painaa "information" kuvaketta aukeaa ikkuna, josta nähdään kesto

![image](https://github.com/user-attachments/assets/62d06035-a344-4cc7-8e93-de8907b4a9aa)

Jos painetaan "play" kuvaketta aukeaa ikkuna, josta nähdään lisää tietoa

![image](https://github.com/user-attachments/assets/a35a379d-7c58-4650-b4f1-8fe0df2bc3a3)

Analyysi: 
- Taajuus: 433.92 MHz
- Näytteenottotaajuus: 1 MSps
- Kesto: 5.49 s
- Näytteessä näyttää olevan kolme signaalia

### Bittistä

Modulaatio on ASK (Amplitude Shifting Key), ja bitit saa näkyviin painamalla "autodetect parameters"

![image](https://github.com/user-attachments/assets/c04b8712-5bc3-4961-a00b-1822fea37d44)

Näytteenottotaajuus: `2 MSps`
Näytteitä: `500`

Kaava: `raakabitin pituus = näytteiden määrä / näytteenottotaajuus` > `500 / 2 000 000 = 0.00025 s = 250µs`

Raakabitin pituus: 250 µs

Wikipediasta löytyy monta mielenkiintoista faktaa tästä, mutta oma lemppari: 

`"3.33564095 microseconds – the time taken by light to travel one kilometre in a vacuum."`

Eli tyhjiössä valo kulkee noin 75 kilometriä 250 mikrosekunnissa.

---

## Lähteet

Karvinen Tero, Verkkoon tunkeutuminen ja tiedustelu, luettavissa: https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/, luettu 15.4.2025

Hubmartin 18.1.2019, Universal Radio Hacker SDR Tutorial on 433 MHz radio plugs , katsottavissa https://www.youtube.com/watch?v=sbqMqb6FVMY&t=199s, katsottu 15.4.2025

Cornelius 4.1.2022, Decode 433.92 MHz weather station data, luettavissa https://www.onetransistor.eu/2022/01/decode-433mhz-ask-signal.html, luettu 15.4.2025

Merbanan, rtl_443, luettavissa https://github.com/merbanan/rtl_433/releases/tag/25.02, luettu 15.4.2025

Python Package Index, urh, luettavissa: https://pypi.org/project/urh/2.3.0/, luettu 16.4.2025

Hobittinen, Aaltojaharjaamassa, luettavissa: https://github.com/hobittinen/verkkoon_tunkeutuminen/blob/main/h3-Aaltojaharjaamassa.md, luettu 16.4.2025

Wikipedia, Microsecond, luettavissa: https://en.wikipedia.org/wiki/Microsecond, luettu 16.4.2025
