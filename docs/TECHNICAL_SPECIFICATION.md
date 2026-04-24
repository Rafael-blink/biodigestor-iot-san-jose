# 📐 Especificación Técnica del Sistema

## Sistema de Monitoreo IoT para Biodigestor Anaeróbico

### 1. Descripción General

Sistema embebido basado en Raspberry Pi Pico W para el monitoreo continuo de variables críticas en un biodigestor anaeróbico tubular. El sistema adquiere datos de temperatura, nivel de biol, presión de biogás y concentración de metano, los procesa localmente y los transmite a una plataforma IoT para visualización remota.

### 2. Especificaciones de Hardware

#### 2.1. Microcontrolador
- **Modelo:** Raspberry Pi Pico W
- **Microcontrolador:** RP2040
- **Arquitectura:** Dual-core ARM Cortex-M0+ @ 133 MHz
- **Memoria:** 264 KB SRAM, 2 MB Flash
- **Conectividad:** WiFi 802.11n (2.4 GHz)
- **GPIO:** 26 pines multifunción
- **Comunicación:** 2× I2C, 2× SPI, 2× UART
- **ADC Interno:** 3 pines (no utilizados por baja resolución)
- **Voltaje de Operación:** 3.3V (regulado internamente)

#### 2.2. Sensores

| Sensor | Principio | Rango | Precisión | Comunicación |
|--------|-----------|-------|-----------|--------------|
| DS18B20 | Termorresistencia digital | -55°C a +125°C | ±0.5°C | OneWire |
| JSN-SR04T | Ultrasónico (40 kHz) | 20-450 cm | ±1 cm | Digital (Trig/Echo) |
| MPX5100DP | Piezorresistivo diferencial | 0-100 kPa (0-14.5 Psi) | ±2.5% | Analógico (0-5V) |
| MQ-4 | Semiconductor (SnO2) | 200-10000 PPM CH4 | ±10% | Analógico (0-5V) |

#### 2.3. Módulos Auxiliares
- **ADC ADS1115:** Conversión analógica-digital de 16 bits, 4 canales, I2C
- **LM2596:** Regulador DC/DC step-down, 3A máx, ajustable
- **Diodo 1N4007:** Protección contra polaridad inversa, 1000V, 1A

### 3. Mapa de Pines

| Pin Pico W | GPIO | Función | Conectado a |
|------------|------|---------|-------------|
| Pin 1 | GP0 | I2C0 SDA | ADS1115 SDA |
| Pin 2 | GP1 | I2C0 SCL | ADS1115 SCL |
| Pin 4 | GP3 | OneWire | DS18B20 DATA |
| Pin 6 | GP4 | Digital Out | JSN-SR04T TRIG |
| Pin 7 | GP5 | Digital In | JSN-SR04T ECHO |
| Pin 36 | 3V3(OUT) | Alimentación | VCC sensores 3.3V |
| Pin 38 | GND | Tierra común | Todos los GND |
| Pin 40 | VBUS | Entrada 5V | Salida LM2596 |

### 4. Especificaciones de Software

- **Firmware:** MicroPython v1.22+
- **Librerías principales:** machine, onewire, ds18x20, time, network, urequests
- **Frecuencia de muestreo:** 2 segundos (local), 20 segundos (nube)
- **Protocolo IoT:** HTTP GET a ThingSpeak API

### 5. Máquina de Estados


| Estado | Condición | Acción |
|--------|-----------|--------|
| NORMAL | Todas las variables en rango óptimo | LED integrado apagado |
| ADVERTENCIA | 1 variable fuera de rango | LED parpadeo lento (1 Hz) |
| PELIGRO | Metano > 5000 PPM o Presión > 10 kPa | LED parpadeo rápido (5 Hz) |

### 6. Umbrales de Referencia (Literatura)

Basado en los estudios de Li et al. (2025), Ahmed et al. (2025) y Pérez-Suárez et al. (2021):

| Variable | Óptimo | Advertencia | Peligro |
|----------|--------|-------------|---------|
| Temperatura | 30-40°C | 25-30°C o 40-45°C | <25°C o >45°C |
| pH | 6.5-7.5 | 6.0-6.5 o 7.5-8.0 | <6.0 o >8.0 |
| Presión | 0-5 kPa | 5-8 kPa | >10 kPa |
| Metano | <1000 PPM | 1000-5000 PPM | >5000 PPM |

### 7. Referencias Técnicas
- RP2040 Datasheet: https://datasheets.raspberrypi.com/rp2040/rp2040-datasheet.pdf
- ADS1115 Datasheet: https://www.ti.com/lit/ds/symlink/ads1115.pdf
- MPX5100DP Datasheet: https://www.nxp.com/docs/en/data-sheet/MPX5100.pdf
