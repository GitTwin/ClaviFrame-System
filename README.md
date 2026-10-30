# ClaviFrame System

> **Official Archive & Technical Manifesto**  
> *Hardware, Firmware, and Development of a modular note-visualization system placed directly on a piano keyboard, featuring an accordion-style foldable mechanical design.*

---

## 🚀 Project Overview
The **ClaviFrame System** is a custom hardware and software architecture designed for the real-time management and visualization of complex MIDI streams through a 3D-printed modular physical structure (accordion-foldable). 

This repository serves as the official historical archive of the project, documenting the evolution of the modules, low-level communication protocols, and the dedicated host infrastructure.

---

## ⚙️ Architectural Specifications

### 1. Hardware Vector
* **Processing Unit (MCU):** Raspberry Pi Pico 2 (`RP2350`).
* **Visual Interface:** `480x480` LCD display per single module, featuring stabilized backlighting (270 Ohm 1/4W resistors) and distributed thermal mitigation.
* **Communication Bus:** 6-wire parallel bus line (`5V`, `12V`, `GND`, `TX`, `RX`, `Spare`). *Spare* line (`GPIO5`) dedicated to hardware synchronization and inter-module clock.
* **Pedal Interface:** 3.5 mm jack wiring connected to the CTS pin of the CH340G chip and pulled to GND, with galvanic bus isolation.

### 2. Software & Host Vector
* **Host Environment:** C++Builder 5 running on Windows XP, with core locking on a single CPU via `SetThreadAffinityMask` (anti Hyper-Threading).
* **Core MIDI:** C++11 DLL based on Stuart Craig's library, featuring binary dump to MidiOut and `extern C` exposure.
* **Performance Optimization:** 60 FPS management utilizing a fixed-size double-buffer technique (480 rows) in RAM for the LCD, and RGB666 interface handling with logical rotation for DMA/PIO.

---

## 📺 Official YouTube Channel
Practical demonstrations, tests of the 16 physically assembled modules, and deep dives into the hardware are published in action on the official channel:

👉 **[Official YouTube Channel - ClaviFrame System](https://www.youtube.com/@ClaviFrameSystem)**

---

## 📜 License
This project is released under the [MIT License](LICENSE) - See the license file for details.
