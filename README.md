# ProtonFlex: Multi-Channel EMG Muscle Activation Game

A rhythm game for your muscles. ProtonFlex reads muscle activity from surface EMG sensors, shows which muscles are firing on a live display, and challenges you to match a sequence of flexes like "bicep, forearm, forearm, thigh." Each attempt is scored on timing and signal strength and ranked against other players.

Built by the Proton Flexers team for Purdue ECE 36200 (Microprocessor Systems & Interfacing), Spring 2026.

<!-- Add a photo or short GIF of the system running here.  -->
<!-- ![ProtonFlex demo](media/demo.gif) -->

## Why

ProtonFlex was designed with people recovering from nerve damage or living with other neuromuscular conditions in mind. Rehab exercises are repetitive, and it's hard to tell whether you're activating the right muscle. By turning muscle training into a guided, scored game, ProtonFlex gives immediate feedback on how well each muscle is activating, helps users build control, and keeps them motivated. Logged sessions make it possible to track progress over time.

## Hardware

- **Microcontroller:** RP2350 on the Purdue "Proton" board
- **Sensors:** MyoWare 2.0 surface EMG sensors (multiple channels)
- **Display:** 2.2" ILI9341 TFT (SPI)
- **Storage:** SD card (SPI) for timestamped session logs

## How it works

- **Sampling:** timer interrupts trigger ADC reads from each EMG channel at a fixed rate.
- **Display:** per-electrode activation is drawn on the TFT. Display updates are streamed with DMA so sampling never pauses for screen refreshes.
- **Separate SPI buses:** the display runs on SPI0 and the SD card on SPI1, so neither blocks the other.
- **Logging:** sessions are written to the SD card with FatFS, with timestamps for later review.
- **Game:** a random flex sequence is shown; the player's EMG response is scored on timing and amplitude, then ranked.

## Development notes

- Firmware is written in C using the Raspberry Pi Pico SDK, built with PlatformIO and a custom platform fork with Proton/RP2350 support.
- The RP2350's RTC API differs from the original Pico's, so parts of the FatFS port had to be excluded and stubbed to build.
- The storage pipeline was tested on simulated, timestamped data before real sensors were connected, which isolated a fault to the SD driver early.

## Building

<!-- Fill in the exact steps team used -->
1. Install [PlatformIO](https://platformio.org/).
2. Clone this repo and open it in VS Code with the PlatformIO extension.
3. Build and upload to the Proton board.

## My contributions

<!-- Edit this to reflect exactly what you did. -->
- Project ideation and overall system design
- Wiring and integration of the peripherals (EMG sensors, display, SD card) to the microcontroller
- Parts of the firmware [specify which]
- Team coordination: schedule, check-ins, and meetings

## Team

Built by the Proton Flexers team ([Kirtan Patel, Michael Zhang, Arnav Vermula]) for Purdue ECE 36200. Original repository: [Vemulk05/Proton-Flex](https://github.com/Vemulk05/Proton-Flex).
