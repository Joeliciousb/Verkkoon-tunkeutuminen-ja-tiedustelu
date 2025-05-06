#  Laboratorio- ja simulaatioympäristöt hyökkäyksissä

## Mininet

### Asennus

Latasin kurssin sivuilta annetun `mininet-lab.ova.xz` xz pakatin tiedoston

![image](https://github.com/user-attachments/assets/f19dbfe4-44b9-427b-8a36-07769492f9f7)

Purin pakkauksen, jonka jälkeen minulla oli `mininet-lab.ova` tiedosto. 

![image](https://github.com/user-attachments/assets/3851f33f-2114-4989-9649-300728bc1c31)

Avasin `VMware Workstation` ohjelman ja siiryn valikosta `open` osioon. 

![Screenshot 2025-05-05 145407](https://github.com/user-attachments/assets/853897ad-9bb7-474a-8337-2e7cc121322d)

Valitsen juuri puretun `mininet-lab.ova` tiedoston ja avaan sen. Ohjelma ladata lutkuttaa hetken. 

![Screenshot 2025-05-05 150750](https://github.com/user-attachments/assets/bd5f74a3-a447-47e2-bc6e-65a9315446f9)

Latauksen jälkeen muokkaan vielä network asetuksista `NAT` asetuksen päälle

![Screenshot 2025-05-05 150958](https://github.com/user-attachments/assets/4b8deb06-9897-4362-99f2-64bc60f55c21)

Käynnistän virtuaalikoneen ja testaan yhteyden verkkoon

![Screenshot 2025-05-05 151104](https://github.com/user-attachments/assets/05be4a1f-bdf8-459a-99f5-348952b61f85)

Mininetin voi käynnistää komennolla `sudo mn`. Testaan vielä pingata mininetin luomia laitteita. 

![Screenshot 2025-05-05 151223](https://github.com/user-attachments/assets/17fb019e-5375-46f3-bad5-6a7b727dc9ef)

Kaikki näyttää toimivan. 

### Tehtävät

#### 01 - Network-Security-Lab

#### Task 1

Aluksi piti kloonata `https://github.com/ssam246/Network-Security-Lab` virtuaalikoneelle komennolla `git clone https://github.com/ssam246/Network-Security-Lab`, jonka jälkeen siirrytään uuteen hakemistoon `cd Network-Security-Lab/scripts`. 

![image](https://github.com/user-attachments/assets/c635b502-96e9-4e24-969d-9cd37d9daaef)

Harjoituksen voi aloittaa ajamalla komennon `sudo python hub_topo.py`

![image](https://github.com/user-attachments/assets/37759e32-7895-4b50-81e1-cc015a5946f1)

Avataan uudet terminaalinäkymät eri hosteille. Komennon `xterm h1 h2` pitäisi toimia. 

![image](https://github.com/user-attachments/assets/85eae00c-5fd7-4e12-9ede-e47c1a09f070)

Tehtävänannon mukaan: 

![image](https://github.com/user-attachments/assets/cb361568-ec16-49f7-9775-34b48dd457ab)

![image](https://github.com/user-attachments/assets/952b1200-28c7-4eef-b03b-8e2bd4631a45)

#### Task 2

Tehtävänanto

![image](https://github.com/user-attachments/assets/694a50b5-adcc-410c-9fb1-0ee69995a56b)

![image](https://github.com/user-attachments/assets/a8a13a62-2dbe-4a56-9a76-a198f4359aac)

#### Task 3

Tehtävänanto 

![image](https://github.com/user-attachments/assets/b56ababb-db45-4649-b064-d1d888612673)

![image](https://github.com/user-attachments/assets/948a575c-2eb3-4828-8f48-180a50919ac0)

---

### 02 - DDoS Simulation in a Software Defined Network

Ajetaan komennot
```
git clone https://github.com/santhisenan/SDN_DDoS_Simulation.git
cd SDN_DDoS_SImulation
```

![image](https://github.com/user-attachments/assets/d1a58792-307d-4155-a6a0-662a421c8002)

Tämän jälkeen ajetaan `sudo python tree_topology.py`

![image](https://github.com/user-attachments/assets/1edb4f80-6557-4745-8f38-08380925f0d2)

---

## evilginx2

En alkanut asentelemaan ainakaan vielä, mutta evilginx2 on man-in-the-middle hyökkäystyökalu, jota käytetään phishing-hyökkäyksiin. 

- reverse proxy
- uhri luulee kirjautuneensa oikealle sivulle, mutta evilginx2 kaappaa tiedot välistä ja varastaa session cookien

## TCP SYN-flood

Loin mininet-verkon komennolla `sudo mn --topo=singe,3`
- 3 hostia
  - h1 hyökkää h2 http palvelinta
  - h2 pyörittää http palvelinta
  - h3 pingaa h2, jotta nähdään pysyykö h2 mukana
- 1 switch

Tämän jälkeen avasin jokaiselle hostille oman xterm ikkunan. host1 xterm ikkunassa asensin hping3 työkalun, jonka avulla suoritan hyökkäyksen. `sudo apt-get install hping3`

host2 xterm ikkunnassa käynnistin http palvelimen portissa 80: `python3 -m http.server 80`

host3 xterm ikkunassa aloin pingata h2. `ping h2`

![image](https://github.com/user-attachments/assets/9a7dd466-0f43-4614-a130-b1a9272cfd1f)

Tässä kuvassa hyökkäys on jo käynnissä ja nähdään, miten h3 > h2 ping vastausaika heittelee

![image](https://github.com/user-attachments/assets/39cd3917-0991-4ca3-8ae9-0c146848008b)

`26.9 ms` oli suurin viive tässä kokeilussa

![image](https://github.com/user-attachments/assets/530f0693-1c5c-4346-b811-2e3ed1a94111)

## Lähteet

Kurssin moodle-sivut

GitHub, ssam246, Network-Security-Lab, luettavissa https://github.com/ssam246/Network-Security-Lab, luettu 5.5.2025

GitHub, santhisenan, DDoS Simulation in a Software Defined Network, luettavissa https://github.com/santhisenan/SDN_DDoS_Simulation, luettu 5.5.2025

GitHub, kgretzky, evilginx2, luettavissa https://github.com/kgretzky/evilginx2, luettu 6.5.2025

Firewall.cx, How to Perform TCP SYN Flood DoS Attack & Detect it with Wireshark - Kali Linux hping3, luettavissa https://www.firewall.cx/tools-tips-reviews/network-protocol-analyzers/performing-tcp-syn-flood-attack-and-detecting-it-with-wireshark.html#how-to-perform-syn-flood-attack, luettu 6.5.2025

