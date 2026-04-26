# Prueba 3: Conversor ADC ADS1115

**Fecha:** 27 de Abril de 2026
**Investigador:** Rafael Gustavo Ramos Noriega
**Firmware:** MicroPython
**Script utilizado:** tests/test_ads1115.py (integrado en prueba MPX5100DP)

## Configuración del Hardware
- Microcontrolador: Raspberry Pi Pico W
- Bus I2C0: SDA=GP4 (Pin 6), SCL=GP5 (Pin 7)
- Frecuencia I2C: 100 kHz
- Dirección ADS1115: 0x48
- Alimentación: 5V

## Objetivo
Verificar la comunicación I2C con el ADS1115 y su capacidad para leer señales analógicas de los sensores de presión y metano.

## Fundamento Teórico
El ADS1115 es un conversor analógico-digital de 16 bits con 4 canales, que utiliza comunicación I2C. Ofrece una resolución de 0.125 mV por LSB cuando se configura con ganancia 1 (rango ±4.096V). Esto es 16 veces más preciso que el ADC interno de la Raspberry Pi Pico (12 bits).

## Resultados

### Escaneo del bus I2C
Dispositivos encontrados: 0x48
El ADS1115 fue detectado correctamente en su dirección predeterminada.

### Lecturas del canal A0 (sensor de presión conectado)
| Medición | V_ADC (V) | Valor Crudo |
|----------|-----------|--------------|
| 1 | 0.1543 | 1234 |
| 2 | 0.1541 | 1233 |
| 3 | 0.1541 | 1233 |
| 4 | 0.1543 | 1234 |
| 5 | 0.1540 | 1232 |
| 6 | 0.1545 | 1236 |
| 7 | 0.1541 | 1233 |
| 8 | 0.1544 | 1235 |
| 9 | 0.1545 | 1236 |
| 10 | 0.1546 | 1237 |

**Estabilidad del ADC:**
- Promedio: 0.1543V
- Variación: ±0.0003V
- Ruido pico a pico: 5 LSB (excelente para 16 bits)

## Conclusiones
1. El ADS1115 funciona correctamente y se comunica sin errores por I2C.
2. La estabilidad de lecturas es excelente, con variación de solo ±0.0003V.
3. La resolución de 16 bits proporciona 0.125mV por LSB, adecuado para sensores analógicos.
4. El uso del ADS1115 externo es necesario porque el ADC interno de la Pico W (12 bits, 0.8mV/LSB) no ofrece suficiente precisión para los sensores de presión y gas.
5. La comunicación I2C a 100 kHz es estable y no presenta errores.

## Referencias
- Texas Instruments ADS1115 Datasheet
- RP2040 Datasheet (Raspberry Pi)
