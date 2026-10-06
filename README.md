# LoRa USB Dongle

USB-C LoRa dongle for the 868 MHz band. It plugs into a PC and shows up as a serial port, with an SMA connector for the antenna.

## Hardware
- USB-C receptacle + USBLC6-2SC6 ESD protection
- VBUS polyfuse + AP2112K-3.3 LDO
- CP2102N USB-UART bridge
- STM32WLE5CCUx (LDO only, no SMPS), 32 MHz crystal, SWD header, reset button, status LEDs
- RF chain based on ST's STDES-WL5U2ILH: BALFHB-WL-06D3 IPD + BGS12WN6 RF switch + SMA jack

## Status
Schematic done (ERC clean). PCB layout in progress.

## Tools
KiCad 10
