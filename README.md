# 🌐 Industrial IIoT Gateway: Integrare OPC UA și MQTT în Node-RED

Acest proiect a fost realizat în cadrul laboratoarelor de specialitate din anul 4 (Politehnica) și reprezintă o soluție de tip **IIoT Gateway**. Rolul său este de a prelua date din echipamente industriale (via OPC UA) și de a le transpună într-un format accesibil pentru monitorizare de la distanță (via MQTT).

> [!IMPORTANT]
> **Context Laborator:** Aceasta este versiunea originală care a funcționat cu succes pe aparatura de laborator (PLC Siemens, server OPC UA și senzori industriali). Proiectul demonstrează managementul fluxului de date între protocoale industriale (OT) și protocoale IoT (IT).

---

## 📊 Arhitectura și Logică

Proiectul este structurat în trei fluxuri principale, fiecare având un rol specific în procesarea telemetriei:

### 1. Extracția Dinamică (OPC UA Browser)
În loc să adaug manual fiecare adresă, am implementat un flux care scanează serverul:
* **Nodul Extract NodeID:** Un script JavaScript care filtrează tag-urile valide și salvează automat NodeID-urile în memoria locală a flow-ului (`flow context`). 
* Această abordare face sistemul mult mai flexibil la schimbările de pe serverul OPC UA.

### 2. Puntea de Date (OPC UA -> MQTT)
Sistemul se abonează la variabilele critice ale motorului din laborator:
* **Filtrare Potențiometru (`ns=4;i=81`):** Datele sunt scalate și trimise pe topicul MQTT `senzor/potentiometru`.
* **Filtrare Avarii Motor (`ns=4;i=61`):** Starea bitului de avarie este monitorizată și trimisă pe topicul `senzor/cuvant_avarii_motor`.

### 3. Monitorizare și Dashboard
Interfața HMI este realizată cu **Node-RED Dashboard** și oferă:
* Vizualizarea poziției potențiometrului printr-un indicator de tip **Gauge (Donut)**.
* Afișarea stării avariilor într-un câmp de tip **Text dinamic**.

---

## 🎮 Interactivitate și Control Manual

Spre deosebire de un sistem complet automatizat, am ales să implementez controlul manual al sesiunilor pentru a face demonstrația mai interesantă și pentru a simula controlul asupra fluxului de date:

* **Conectare/Deconectare Broker:** Conexiunea la serverul MQTT nu este automată; am creat noduri de tip `Inject` (butoane) care permit pornirea sau oprirea manuală a legăturii cu brokerul.
* **Abonare/Dezabonare (Subscribe/Unsubscribe):** Utilizatorul poate decide momentul în care dorește să înceapă recepția datelor de la senzori. 
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

---
*Proiect realizat pentru demonstrarea integrării sistemelor industriale cu tehnologii de tip Cloud/IoT.*
