# 📘 Piezo DAQ Front-End for ESP32
**High-speed piezoelectric data acquisition front-end featuring OPA320, mid-rail biasing, RC anti-alias filtering, and a 16-bit 1 MSPS SAR ADC (MCP33131D-10).  
Designed for vibration, impact, and acoustic emission (AE) analysis using the ESP32.**

---

## 🔧 Features
- 16-bit, 1 MSPS SAR ADC (MCP33131D-10)
- Low-noise OPA320 amplifier
- Selectable gain (SW2)
- Clean mid-rail VCM reference (1.65 V)
- Piezo protection (BAT54S)
- Anti-aliasing filter (22Ω + 1.7nF)
- High-speed SPI interface for ESP32
- KiCad hardware + ESP32 firmware included

---

## 📚 Documentation
| File | Description |
|------|-------------|
| `docs/theory-of-operation.md` | System architecture and analog front-end theory |
| `docs/block-diagram.png` | High-level signal flow |
| `docs/wiring-guide.png` | ESP32 connection diagram |
| `docs/gain-settings.md` | Amplifier gain modes and switch mapping |

---

## 🚀 Getting Started

### 1. Hardware Setup
Connect the module to ESP32:

| ADC Pin | ESP32 Pin |
|---------|-----------|
| SCLK | GPIO 18 |
| SDO | GPIO 19 |
| CS  | GPIO 5 |
| 3.3V | 3V3 |
| GND | GND |

### 2. Firmware Example (Basic SPI Read)
```cpp
#include <Arduino.h>
#include <SPI.h>

#define PIN_CS   5
#define PIN_SCLK 18
#define PIN_MISO 19

SPIClass ADC_SPI(HSPI);

void setup() {
    Serial.begin(115200);
    ADC_SPI.begin(PIN_SCLK, PIN_MISO, -1, PIN_CS);
    pinMode(PIN_CS, OUTPUT);
    digitalWrite(PIN_CS, HIGH);
}

uint16_t readADC() {
    digitalWrite(PIN_CS, LOW);
    uint16_t value = ADC_SPI.transfer16(0x0000);
    digitalWrite(PIN_CS, HIGH);
    return value;
}

void loop() {
    Serial.println(readADC());
}
