# ⚖️ HX711 Load Cell & RPM Measurement (STM32F103)

![MCU](https://img.shields.io/badge/MCU-STM32F103C8T6-03234B?logo=stmicroelectronics&logoColor=white)
![ADC](https://img.shields.io/badge/ADC-HX711%2024--bit-2E7D32)
![IDE](https://img.shields.io/badge/IDE-STM32CubeIDE-03234B)
![Language](https://img.shields.io/badge/Language-C-555555?logo=c)

<a id="english"></a>**🇬🇧 English** · [🇻🇳 Tiếng Việt](#tieng-viet)

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

<a id="tieng-viet"></a>

## 🇻🇳 Tiếng Việt

[🇬🇧 English](#english) · **🇻🇳 Tiếng Việt**

Đo khối lượng bằng **loadcell + ADC 24-bit HX711** trên **STM32F103C8T6**, kèm bộ **đo tốc độ quay (RPM)** bằng cách đếm xung. Có thể dùng làm bàn đo lực đẩy hoặc mô-men cho động cơ và cánh quạt.

### ✨ Tính năng

- **Driver HX711 bit-bang:** đọc 24-bit với thời gian µs lấy từ TIM2, có timeout 200 ms nếu cảm biến chưa sẵn sàng.
- **Lấy trung bình:** mỗi lần đo là trung bình của 50 mẫu để giảm nhiễu.
- **Hiệu chuẩn 2 điểm:** trừ bì (tare) và nhân hệ số tỉ lệ từ một vật mẫu đã biết khối lượng. Kết quả tính bằng **miligam**.
- **Đo RPM:** đếm xung trên PA0 (ngắt EXTI), TIM3 quy đổi mỗi giây: `RPM = số xung/giây × 60`.

Bảng nối chân: xem phần tiếng Anh ở trên.

### ⚙️ Hiệu chuẩn

Các hằng số nằm trong `HX711/Core/Src/main.c`:

1. Đọc giá trị thô khi không tải và gán vào `tare`.
2. Đặt vật mẫu, đọc `(raw − tare)` và gán vào `knownHX711`. Gán khối lượng vật mẫu (mg) vào `knownOriginal`.
3. `weight = (average − tare) × knownOriginal / knownHX711` (mg).

### 🚀 Hướng dẫn sử dụng

1. Mở thư mục `HX711` bằng **STM32CubeIDE**.
2. Build và nạp bằng ST-Link.
3. Theo dõi biến `weight` (mg) và `rpm` trong **Live Expressions** khi debug.

---

<p align="center">Made by <a href="https://github.com/TuanLinh05">Vu Tuan Linh</a> · HCMUT</p>
