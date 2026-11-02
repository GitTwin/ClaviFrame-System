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
* **Host Pc Environment:** C++Builder 5 running on Windows XP, with core locking on a single CPU via `SetThreadAffinityMask` (anti Hyper-Threading).
* **Core MIDI:** C++11 DLL based on Stuart Craig's library, featuring binary dump to MidiOut and `extern C` exposure.
* **Performance Optimization:** 60 FPS management utilizing a fixed-size double-buffer technique (480 rows) in RAM for the LCD, and RGB666 interface handling with logical rotation for DMA/PIO.

---

## 📁 Technical Documentation

For an in-depth analysis of the individual modules and firmware, please refer to the dedicated guides in the `docs` folder:
* [Incipit, Origin Problem & Architecture Overview](docs/cf_episode_00.md) - *The initial technical manifesto, project genesis, hardware constraints (Raspberry Pi Pico 2 / RP2350), and architectural choices for the C++Builder 5 host on Windows XP.*
<!-- 
* [Program Pc Host Specifications](docs/CfPc_specs.md)*
* [16-Module Specifications](docs/CfModule_specs.md)*
* [Communication Protocols & Bus Layout](docs/protocols.md)*
-->
---

## 📺 Official YouTube Channel

* <img src="https://yt3.ggpht.com/fLU9FasrhSwZtXxpJuSyPmQh1pYS1tXrZbnYzEx_uKByCx20HWTzgBOkS9TwcaH-Mn58IaVULyU=s600-c-k-c0x00ffffff-no-rj-rp-mo" width="60" align="absmiddle"> **[ClaviFrame System](https://www.youtube.com/@ClaviFrameSystem)**  
  Practical demonstrations, tests of the 16 physically assembled modules, and deep dives into the hardware are published in action on our official channel.
  
  Depending on the level of detail you are looking for, you can follow two video paths:
    * **[Features & Demos](https://www.youtube.com/@ClaviFrameSystem/playlists)**  
      *Practical demonstrations of the system in action, MIDI stream management, and visual feedback on the piano keyboard.*
      
    * **[The Making Of... (Coming Soon)](https://www.youtube.com/@ClaviFrameSystem/playlists).**  
      *The technical series dedicated to firmware development (Raspberry Pi Pico 2), the C++Builder architecture on Windows XP, and hardware assembly.*
---

## 📜 License
This project is released under the [MIT License](LICENSE) - See the license file for details.
