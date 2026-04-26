# Prueba 1: Sensor de Temperatura DS18B20

**Fecha:** 26 de Abril de 2026
**Investigador:** Rafael Gustavo Ramos Noriega
**Firmware:** MicroPython
**Script utilizado:** tests/test_ds18b20.py

## Configuración del Hardware
- Microcontrolador: Raspberry Pi Pico W
- Pin OneWire: GP17 (Pin 22)
- Alimentación: 3.3V desde el riel de la Pico W
- Resistencia pull-up: 4.7kΩ entre DATA y 3.3V
- Protocolo: OneWire (Maxim Integrated)

## Objetivo
Verificar la comunicación OneWire, la estabilidad de lecturas y la respuesta térmica del sensor DS18B20 para el monitoreo de temperatura en el biodigestor.

## Fundamento Teórico
El DS18B20 es un sensor de temperatura digital con resolución programable de 9 a 12 bits. Utiliza el protocolo OneWire, que permite comunicación bidireccional usando un solo cable de datos. Cada sensor tiene un ROM único de 64 bits grabado en fábrica, lo que permite conectar múltiples sensores en el mismo bus.

## Resultados

### Lecturas iniciales (sin contacto, 10 mediciones)
| Lectura | Temperatura (°C) |
|---------|------------------|
| 1 | 31.50 |
| 2 | 31.50 |
| 3 | 31.50 |
| 4 | 31.50 |
| 5 | 31.50 |
| 6 | 31.50 |
| 7 | 31.50 |
| 8 | 31.50 |
| 9 | 31.50 |
| 10 | 31.50 |

**Estabilidad:** Todas las lecturas idénticas (31.50°C). El sensor muestra estabilidad perfecta en condiciones constantes.

### Respuesta térmica (al tocar con los dedos)
| Tiempo | Temperatura (°C) | ΔT (°C) |
|--------|------------------|---------|
| t0 | 31.50 | 0.00 |
| t1 | 31.50 | 0.00 |
| t2 | 31.50 | 0.00 |
| t3 | 31.56 | +0.06 |
| t4 | 32.00 | +0.50 |
| t5 | 32.44 | +0.94 |
| t6 | 32.81 | +1.31 |
| t7 | 33.06 | +1.56 |
| t8 | 33.25 | +1.75 |
| t9 | 33.38 | +1.88 |
| t10 | 33.50 | +2.00 |
| t11 | 33.56 | +2.06 |

**Tiempo de respuesta:** 3 segundos para detectar cambio.
**Sensibilidad:** Detecta cambios de 0.06°C (resolución de 12 bits).
**ΔT máximo:** +2.06°C por calor corporal.

### Información del Sensor
- ROM: 285e5f4d06000075
- Resolución: 12 bits (0.0625°C por LSB)
- Rango: -55°C a +125°C
- Precisión: ±0.5°C (en rango -10°C a +85°C)

## Conclusiones
1. El sensor DS18B20 funciona correctamente y se comunica sin errores por OneWire.
2. La estabilidad de lecturas es excelente, con cero variación en condiciones estables.
3. La respuesta térmica es adecuada, detectando cambios sutiles de temperatura.
4. La precisión (±0.06°C) es más que suficiente para el monitoreo del biodigestor, que opera en rango mesofílico (30-40°C).
5. La resistencia pull-up de 4.7kΩ proporciona una comunicación OneWire estable.

## Referencias
- Maxim Integrated DS18B20 Datasheet
- MicroPython OneWire Library Documentation
