# ⚖️ HX711 Load Cell & RPM Measurement (STM32F103)

![MCU](https://img.shields.io/badge/MCU-STM32F103C8T6-03234B?logo=stmicroelectronics&logoColor=white)
![ADC](https://img.shields.io/badge/ADC-HX711%2024--bit-2E7D32)
![IDE](https://img.shields.io/badge/IDE-STM32CubeIDE-03234B)
![Language](https://img.shields.io/badge/Language-C-555555?logo=c)

Weight measurement with a **load cell + HX711 24-bit ADC** on an **STM32F103C8T6**, plus a pulse-counting **RPM meter**. Useful as a thrust/torque test bench for motors and propellers.

---

## ✨ Features

- **Bit-banged HX711 driver:** 24-bit read with microsecond timing from TIM2 and a 200 ms timeout if the sensor is not ready.
- **Averaging:** each reading is the mean of 50 samples to reduce noise.
- **Two-point calibration:** tare offset + scale factor from a known reference mass, output in **milligrams**.
- **RPM measurement:** pulses on PA0 (EXTI) are counted and converted every second by TIM3: `RPM = pulses/s × 60`.

## 🔧 Hardware

| Function | STM32F103 pin | Connected to |
| :-- | :-- | :-- |
| HX711 DOUT | PB8 (input) | HX711 `DT` |
| HX711 SCK | PB9 (output) | HX711 `SCK` |
| Speed sensor | PA0 (EXTI, rising edge) | Hall / optical sensor output |
| µs delay timer | TIM2 | – |
| 1 s time base | TIM3 (interrupt) | – |

## ⚙️ Calibration

Calibration constants are in `HX711/Core/Src/main.c`:

```c
uint32_t tare         = 8645755;  // raw HX711 value with no load
float    knownOriginal = 74000;   // reference mass in mg
float    knownHX711    = 30108;   // (raw − tare) measured with the reference mass
```

1. Read the raw value with no load and put it in `tare`.
2. Place a known mass, read `(raw − tare)` and put it in `knownHX711`; put the mass (mg) in `knownOriginal`.
3. `weight = (average − tare) × knownOriginal / knownHX711` (mg).

## 🚀 Getting started

1. Open the `HX711` folder in **STM32CubeIDE** (*File → Import → Existing Projects into Workspace*).
2. Build and flash with an ST-Link.
3. Watch the `weight` (mg) and `rpm` variables in **Live Expressions** while debugging.

---

<p align="center">Made by <a href="https://github.com/TuanLinh05">Vu Tuan Linh</a> · HCMUT</p>
