# ClaviFrame System — Incipit and Origin Problem

> **Technical & Historical Background**  
> *This document serves as the foundational narrative and technical background for Episode 00 of the ClaviFrame System project, exploring the roots, design choices, and architectural constraints of the hardware-software stack.*

---

## Introduction: Roots and Vision

Welcome to this journey into the heart of the **ClaviFrame System**.

If you stumbled upon this, forget prepackaged solutions, infinite clouds, and wasted computing power. Here, we talk about low-level engineering, hardware recovery, bare metal, and code written with purpose.

I grew up around the 1980s, an era when you opened up a machine with a screwdriver, looked at the traces, and counted every single byte. That way of understanding technology—where every resource is precious and every hardware limit becomes a creative stimulus—never died; it just evolved. ClaviFrame was born from exactly this philosophy: combining a passion for keyboards and music with the harsh reality of minimal hardware, leveraging modern microcontrollers, 3D printing, and workshop ingenuity.

---

## 1. The Origin Problem and Desiderata

Watching music videos on YouTube, I was completely mesmerized by Rousseau's channel: the geometric harmony of notes falling from above and the clean execution are hypnotic. So I asked myself: *why not try to build something like that?* Maybe structured in a way to help compensate for my limited piano-playing skills.

Naturally, the approach was the usual one: no bloated hardware or expensive PCs, but using the absolute minimum resources necessary, squeezed to the bone to keep costs close to zero while utilizing what was already available in my workshop.

Essentially, I needed a display capable of covering the entire span of a keyboard and software capable of parsing a MIDI file, turning note events into perfectly synchronized vertical columns. I already had small 4-inch LCDs lying around with a $480\times480$ resolution and an RGB control bus: the choice was forced. For local control, I turned to the Raspberry Pi Pico 2, powered by the mighty RP2350, while for the command station, I dusted off my old ASUS netbook running Windows XP.

Let's add a fundamental requirement: modularity. The system had to be portable and adaptable even to instruments with fewer keys. To cover a standard 88-key piano keyboard with my 4-inch modules, I needed 16 of them, turning the assembly into a single, massive logical display of $7920 \times 480$ pixels, factoring in the natural mechanical gaps between consecutive modules.

All the mechanics were designed with my trusty old SketchUp Make 2017 CAD and 3D printed with a glorious, contemporary R2EVO printer. *Dulcis in fundo*: the PC control software had to remain anchored to the old Win32 APIs, with the GUI built on good old Borland C++Builder 5.

With these premises and constraints, the challenge was open, crazy enough, and terribly fun.

---

## 2. Hardware, Constraints, and Module Intelligence

Analyzing the project, I realized right away that with a host of very limited capabilities like a netbook, the workload had to be carefully distributed. The goal was to give individual actuators complete autonomy in managing their display output.

On the other hand, managing a total screen area of roughly $8000 \times 480$ pixels in RGB666 format would have been impossible for a PC of that era: the bandwidth at 60 FPS would have choked any direct solution. The only way out was to make the individual LCD modules "intelligent," sending images in a compact, encoded format that reduces the entire dynamic to a few lightweight messages per frame.

Looking at the real needs of the application, I was able to drastically slash complexity:

* **Color Space:** Instead of the $2^{18}$ colors of an RGB666 interface, I opted for $2^6$ colors ($64$ shades), simplifying both physical wiring and firmware management down to an **RGB222** interface.

* **Double Buffering and Dual-Core:** Switching to RGB222 freed up enough RAM on the Raspberry Pi Pico 2 (RP2350) to implement *Double Buffering* (Front and Back) — the exact technique used in 80s games to eliminate tearing. Leveraging the two cores of the RP2350, **Core 0** continuously writes the Front Buffer to the LCD, while **Core 1** talks to the PC and prepares the Back Buffer.

* **Visual Optimization:** Taking advantage of the fact that notes fall from above, most of the image is already on screen and just needs to be "scrolled/rotated," a classic trick from 80s platform games.

On the mechanical and power front, the modules had to sit as close as possible to minimize the blind gap while allowing the ClaviFrame to fold like an accordion. Finally, current draw was carefully analyzed to distribute loads and keep the parallel bus wire gauge thin.

---

## 3. Communication, Firmware, and System Architecture

For data transmission and control messaging between the PC and the modules, I opted for a **115200 baud serial line** in a strictly **Master/Slave** configuration. The modules respond and claim the bus one at a time, only when interrogated by the PC, staying in high impedance the rest of the time.

Inside the module firmware, I had to implement two key features:

1. **IAP (In-Application Programming):** To update module software directly from the PC without physically messing with each individual board.

2. **Flash as EEPROM:** A portion of the Flash memory is treated as EEPROM to store local runtime parameters, such as the module's unique bus address and the first piano key assigned to it.

Of course, geometric complications were inevitable: the physical width of real piano keys is not an integer submultiple of the LCD display width. This forced me to handle software partitioning of individual keys straddling two adjacent modules.

**The final result? The ClaviFrame System consists of three macro-elements:**

* **CfHost:** The control software running on the ASUS EeePC 1001PX netbook with Windows XP.

* **CfBox:** The junction box housing the USB/Serial adapter, the 5V to 12V Step-Up converter for LCD backlight, and the 5V power distribution for the Pico 2s.

* **CfModule (x16):** The sixteen intelligent modules that bring notes to life across the entire keyboard layout.

---

## 4. Closing & Development Outlook

We have reached the end of this foundational analysis of the ClaviFrame System architecture. You won't find commercial promises, prepackaged solutions, or cookie-cutter code here: there is only raw, unadulterated passion for low-level engineering, hardware recovery, and the desire to push a Raspberry Pi Pico 2 and a 15-year-old netbook to their absolute limits.

If you want to dive deeper into the code or follow the practical demonstrations, check out our [Official YouTube Channel](https://www.youtube.com/@ClaviFrameSystem) and explore the rest of the repository documentation.
