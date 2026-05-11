# 🌐 Industrial IIoT Gateway: OPC UA & MQTT

Soluție de monitorizare realizată în Node-RED (Proiect Lab, Anul 4 Poli) pentru integrarea datelor industriale (**OPC UA**) în fluxuri IoT (**MQTT**). 

> [!NOTE]
> **Hardware Lab:** Versiune testată și funcțională pe echipamente reale (PLC Siemens, senzori industriali), demonstrând convergența dintre mediul industrial (OT) și cel informatic (IT).
---

## 📊 Arhitectura și Logică

Proiectul este structurat în trei fluxuri principale, fiecare având un rol specific în procesarea telemetriei:

### 1. Extracția Dinamică (OPC UA Browser)
În loc să adaug manual fiecare adresă, am implementat un flux care scanează serverul:
* **Nodul Extract NodeID:** Un script JavaScript care filtrează tag-urile valide și salvează automat NodeID-urile în memoria locală a flow-ului (`flow context`). 
* Această abordare face sistemul mult mai flexibil la schimbările de pe serverul OPC UA.
<img width="831" height="72" alt="image" src="https://github.com/user-attachments/assets/84222bed-ad6d-4c70-bd8c-7eb01159ddb1" />

### 2. Puntea de Date (OPC UA -> MQTT)
Sistemul se abonează la variabilele critice ale motorului din laborator:
* **Filtrare Potențiometru (`ns=4;i=81`):** Datele sunt scalate și trimise pe topicul MQTT `senzor/potentiometru`.
* **Filtrare Avarii Motor (`ns=4;i=61`):** Starea bitului de avarie este monitorizată și trimisă pe topicul `senzor/cuvant_avarii_motor`.
<img width="1134" height="224" alt="image" src="https://github.com/user-attachments/assets/9043d276-1aea-4916-b58e-5c6aaba0fb7a" />

### 3. Monitorizare și Dashboard
Interfața HMI este realizată cu **Node-RED Dashboard** și oferă:
* Vizualizarea poziției potențiometrului printr-un indicator de tip **Gauge (Donut)**.
* Afișarea stării avariilor într-un câmp de tip **Text dinamic**.
<img width="1243" height="198" alt="image" src="https://github.com/user-attachments/assets/3238ee80-75f4-40e0-9ce3-8d19ed910743" />

---

## 🎮 Interactivitate și Control Manual

Spre deosebire de un sistem complet automatizat, am ales să implementez controlul manual al sesiunilor pentru a face demonstrația mai interesantă și pentru a simula controlul asupra fluxului de date:

* **Conectare/Deconectare Broker:** Conexiunea la serverul MQTT nu este automată; am creat noduri de tip `Inject` (butoane) care permit pornirea sau oprirea manuală a legăturii cu brokerul.
  <img width="705" height="209" alt="image" src="https://github.com/user-attachments/assets/cbb6888c-a03e-4aae-ab41-a99f8d14dedd" />
* **Abonare/Dezabonare (Subscribe/Unsubscribe):** Utilizatorul poate decide momentul în care dorește să înceapă recepția datelor de la senzori.
  <img width="449" height="143" alt="image" src="https://github.com/user-attachments/assets/4e8f988c-45ae-4937-927a-dc771f3a1eb8" />
* *De ce?* Această logică este utilă în scenarii de mentenanță sau debugging, permițând controlul precis asupra traficului de rețea.

---

## 🛠️ Specificații Tehnice

* **Protocoale:** OPC UA (Industrial), MQTT (IoT).
* **Limbaje:** JavaScript (în nodurile de funcție Node-RED).
* **Hardware Interfațat:** PLC Industrial (Server OPC UA), Motor cu convertizor, Potențiometru de proces.

---

## 🚀 Instalare și Rulare

1. Instalați **Node-RED** local.
2. Din meniul *Manage Palette*, instalați următoarele librării necesare:
   * `node-red-dashboard`
   * `node-red-contrib-opcua`
3. Importați fișierul `flows.json` în spațiul de lucru.
4. Apăsați **Deploy**.
5. Accesați Dashboard-ul la adresa: `http://localhost:1880/ui`.


