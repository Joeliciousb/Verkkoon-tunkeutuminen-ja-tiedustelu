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


![image](https://github.com/user-attachments/assets/4bcbff6c-c1c7-4608-bb63-b1ea6bfff183)  

---

![image](https://github.com/user-attachments/assets/7e716c89-c974-44b4-957b-d0f468d29c6b)

Asennan `atool` nimisen työkalun kommennolla `sudo apt-get -y install atool wget libssl-dev libtool libusb-1.0-0-dev librtlsdr-dev rtl-sdr libsoapysdr-dev`

![image](https://github.com/user-attachments/assets/8ac1c48e-eef1-430d-b766-c54a0dfdc399)

Tämän jälkeen käynnistän koneen uudelleen. 

### Asennus

### Automaattinen analyysi

--- 

## 

## Lähteet

Karvinen Tero, Verkkoon tunkeutuminen ja tiedustelu, luettavissa: https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/, luettu 15.4.2025

Hubmartin 18.1.2019, Universal Radio Hacker SDR Tutorial on 433 MHz radio plugs , katsottavissa https://www.youtube.com/watch?v=sbqMqb6FVMY&t=199s, katsottu 15.4.2025

Cornelius 4.1.2022, Decode 433.92 MHz weather station data, luettavissa https://www.onetransistor.eu/2022/01/decode-433mhz-ask-signal.html, luettu 15.4.2025
