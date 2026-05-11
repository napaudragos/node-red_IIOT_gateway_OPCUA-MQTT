# 🌐 Industrial IIoT Gateway: Integrare OPC UA și MQTT în Node-RED

Acest proiect a fost dezvoltat în cadrul laboratoarelor de specialitate (Anul 4, Politehnica) și servește drept punte de legătură (Gateway) între protocoalele industriale clasice și tehnologiile moderne de tip IoT.

> [!IMPORTANT]
> **Notă Laborator:** Aceasta este versiunea finală care a funcționat perfect cu aparatura reală din laborator (PLC, senzori industriali și server OPC UA).

---

## 📋 Prezentare Generală

Obiectivul principal al proiectului este extragerea datelor de telemetrie de la un motor industrial și un potențiometru de proces folosind protocolul **OPC UA**, procesarea acestora și retransmiterea lor via **MQTT** către un dashboard de monitorizare în timp real.

### Componente Principale:
1. **Data Acquisition (OPC UA):** Scanarea automată a tag-urilor și abonarea (Subscribe) la variabilele de proces.
2. **Data Bridge (MQTT):** Conversia datelor industriale în mesaje ușoare pentru mediul cloud/IT.
3. **Interfață HMI:** Vizualizarea stării motorului și a valorilor citite prin Node-RED Dashboard.

---

## 🛠️ Detalii Tehnice & Logică

### 1. Extracția Datelor (OPC UA)
Am implementat o logică de tip **Browser** care extrage automat NodeID-urile valide de pe server. Nodul de funcție `Extract NodeID` filtrează tag-urile de tip "Null" și salvează lista în memoria locală (`flow context`) pentru a fi utilizată de nodul de **Subscribe**.
* **Endpoint Lab:** `opc.tcp://192.168.50.96:4840`

### 2. Procesarea și Filtrarea (JavaScript)
Datele brute primite prin OPC UA sunt trecute prin noduri de tip `Function` pentru curățare și rutare:
* **Filtrare Potențiometru:** Identifică ID-ul specific (`ns=4;i=81`) și scalează valoarea pentru a fi trimisă pe topic-ul MQTT `senzor/potentiometru`.
* **Filtrare Avarii Motor:** Monitorizează registrul de avarii (`ns=4;i=61`) și trimite starea către topic-ul `senzor/cuvant_avarii_motor`.

### 3. Monitorizarea (Dashboard)
Sistemul dispune de o interfață grafică ce conține:
* **Indicator de tip Donut (Gauge):** Pentru afișarea poziției potențiometrului (0-100%).
* **Câmp Text Dinamic:** Pentru afișarea în clar a cuvântului de avarie primit de la motor.

---

## 📂 Structura Repozitoriului

* `flows.json`: Fișierul sursă care conține întreaga logică a nodurilor.
* `Screenshot_Flow.png`: Captură de ecran cu arhitectura logică a proiectului.

---

## 🚀 Cum se utilizează

1. Asigurați-vă că aveți **Node-RED** instalat.
2. Instalați următoarele librării din *Manage Palette*:
   * `node-red-dashboard`
   * `node-red-contrib-opcua`
3. Importați fișierul `flows.json` în editorul Node-RED.
4. Apăsați butonul **Deploy**.
5. Accesați dashboard-ul la adresa: `http://localhost:1880/ui`.

---
*Realizat ca proiect de laborator pentru demonstrarea competențelor în Industrial Internet of Things (IIoT).*
