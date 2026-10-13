# LoRa USB Dongle

<p align="center">
  <img src="https://img.shields.io/badge/Status-Work%20in%20progress-yellow" alt="Status">  <img src="https://img.shields.io/badge/KiCad-10.0-blue?logo=kicad&logoColor=white" alt="KiCad 10.0">
  <img src="https://img.shields.io/badge/Hardware%20Rev-v1.0-brightgreen" alt="Hardware Rev">
  <img src="https://img.shields.io/badge/License-CERN--OHL--P-orange" alt="License">
</p>

A compact USB-C LoRa dongle for the **868 MHz** EU band, built around the **STM32WLE5**.
Plug it into a PC and it enumerates as a standard **USB serial port** (CP2102N, driver-free on most OSes); the antenna connects through an **SMA** jack.


<img width="600" alt="Top view" src="https://github.com/user-attachments/assets/29c8bbf9-87e6-45f2-b0b9-3b5891f112a1" />

## Block diagram

```
USB-C ──► ESD (USBLC6-2SC6) ──► CP2102N ──UART──► STM32WLE5CCUx ──► IPD ──► RF switch ──► SMA
  │                                                (BALFHB-WL-06D3)  (BGS12WN6)
  └─► Polyfuse ──► AP2112K-3.3 LDO ──► 3V3 rail
```

## Hardware

| Block | Part | Notes |
|---|---|---|
| Connector | USB-C receptacle (USB 2.0) | 5.1 kΩ CC pull-downs, device mode |
| ESD protection | USBLC6-2SC6 | On D+/D− and VBUS |
| Input protection | Polyfuse on VBUS | |
| Power | AP2112K-3.3 | 3.3 V LDO, 600 mA |
| USB-UART bridge | CP2102N (QFN28) | TXD/RXD to the MCU UART |
| MCU + radio | STM32WLE5CCUx | Cortex-M4 + sub-GHz LoRa/(G)FSK radio, LDO mode (SMPS not used) |
| Clock | 32 MHz crystal | HSE for the radio |
| RF front-end | BALFHB-WL-06D3 + BGS12WN6 | Integrated balun/matching/harmonic filter IPD + SPDT TX/RX switch |
| Antenna | SMA jack | 50 Ω |

### RF chain

The RF section follows ST's **STDES-WL5U2ILH** reference design:

- **TX**: `RFO_LP` → series DC-block cap → IPD (harmonic filtering) → switch → SMA
- **RX**: SMA → switch → IPD (balun) → `RFI_P` / `RFI_N` (differential LNA input)
- The switch is driven by an MCU GPIO; the radio is half-duplex, so TX and RX never run at the same time.

Layout guidelines applied:
- The chip ↔ IPD section is kept as short and symmetric as possible (complex, non-50 Ω impedances).
- Everything after the IPD is a 50 Ω grounded coplanar waveguide (GCPW) with via stitching.
- Solid ground plane under the whole RF area, dedicated GND vias on every IPD ground bump.


## Tools

- **KiCad 10** — schematic and PCB

