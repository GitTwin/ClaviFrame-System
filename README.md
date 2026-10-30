# ClaviFrame System

> **Archivio Ufficiale & Manifesto Tecnico**  
> *Hardware, Firmware e Sviluppo di un sistema di visualizzazione note da disporre direttamete sulla tastiera di un pianoforte modulare e con meccanica ripiegabile a fisarmonica.*

---

## 🚀 Panoramica del Progetto
Il **ClaviFrame System** è un'architettura hardware e software custom progettata per la gestione e la visualizzazione in tempo reale di flussi MIDI complessi attraverso una struttura fisica modulare (ripiegabile a fisarmonica) stampata in 3D. 

Questo repository funge da archivio storico ufficiale del progetto, documentando l'evoluzione dei moduli, i protocolli di comunicazione a basso livello e l'infrastruttura host dedicata.

---

## ⚙️ Specifiche Architetturali

### 1. Vettore Hardware
* **Unità di Elaborazione (MCU):** Raspberry Pi Pico 2 (`RP2350`).
* **Interfaccia Visiva:** Display LCD `480x480` per singolo modulo, con retroilluminazione stabilizzata (resistenze da 270 Ohm 1/4W) e mitigazione termica distribuita.
* **Bus di Comunicazione:** Linea bus parallela a 6 fili (`5V`, `12V`, `GND`, `TX`, `RX`, `Spare`). Linea *Spare* (`GPIO5`) dedicata alla sincronizzazione hardware e al clock inter-modulo.
* **Interfaccia Pedale:** Cablaggio su jack da 3.5 mm collegato al pin CTS del chip CH340G e chiuso a GND, con isolamento galvanico del bus.

### 2. Vettore Software & Host
* **Ambiente Host:** C++Builder 5 in esecuzione su Windows XP, con core bloccato su singola CPU tramite `SetThreadAffinityMask` (anti Hyper-Threading).
* **Core MIDI:** DLL in C++11 basata sulla libreria di Stuart Craig, con dump binario su MidiOut ed esposizione `extern C`.
* **Ottimizzazione delle Performance:** Gestione a 60 FPS con gestione dello LCD mediante tecnica a double buffer a dimensione fissa (480 righe) in RAM e gestione dell'interfaccia RGB666 con rotazione logica per DMA/PIO.

---

## 📺 Canale YouTube Ufficiale
Le dimostrazioni pratiche, i test dei 16 moduli fisicamente assemblati e gli approfondimenti tecnici sull'hardware sono pubblicati in azione sul canale ufficiale:

👉 **[Link al Canale YouTube del ClaviFrame System](#)** *(https://www.youtube.com/@ClaviFrameSystem)*

---

## 📜 Licenza
Questo progetto è rilasciato sotto licenza [MIT](LICENSE) - Consultare il file di licenza per i dettagli.
