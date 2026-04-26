# Registro de Pruebas de Sensores

## Prueba 1: Sensor de Temperatura DS18B20

**Fecha:** 26 de Abril de 2026  
**Investigador:** Rafael Gustavo Ramos Noriega  
**Firmware:** MicroPython  
**Script utilizado:** `tests/test_ds18b20.py`

### Configuración del Hardware

| Parámetro          | Valor                            |
|--------------------|----------------------------------|
| Microcontrolador   | Raspberry Pi Pico W              |
| Pin OneWire        | GP17 (Pin 22)                    |
| Alimentación       | 3.3V                             |
| Resistencia pull-up| 4.7 kΩ entre DATA y 3.3V         |

### Objetivo

Verificar la comunicación OneWire, la estabilidad de lecturas y la respuesta térmica del sensor DS18B20.

### Resultados

#### Lecturas iniciales (sin contacto)

| Lectura | Temperatura (°C) |
|---------|------------------|
| 1       | 31.50            |
| 2       | 31.50            |
| 3       | 31.50            |
| 4       | 31.50            |
| 5       | 31.50            |
| 6       | 31.50            |
| 7       | 31.50            |
| 8       | 31.50            |
| 9       | 31.50            |
| 10      | 31.50            |

**Estabilidad:** Todas las lecturas idénticas. Sensor perfectamente estable.

#### Respuesta térmica (al tocar con los dedos)

| Tiempo | Temperatura (°C) | ΔT (°C) |
|--------|------------------|---------|
| t0     | 31.50            | 0.00    |
| t1     | 31.50            | 0.00    |
| t2     | 31.50            | 0.00    |
| t3     | 31.56            | +0.06   |
| t4     | 32.00            | +0.50   |
| t5     | 32.44            | +0.94   |
| t6     | 32.81            | +1.31   |
| t7     | 33.06            | +1.56   |
| t8     | 33.25            | +1.75   |
| t9     | 33.38            | +1.88   |
| t10    | 33.50            | +2.00   |
| t11    | 33.56            | +2.06   |

#### Métricas de respuesta térmica

| Métrica                  | Valor                        |
|--------------------------|------------------------------|
| Tiempo de respuesta      | ~3 segundos para detectar cambio |
| Sensibilidad             | 0.06 °C                      |
| ΔT máximo observado      | +2.06 °C (calor corporal)    |

### ROM del Sensor

```
285e5f4d06000075
```

Este identificador único permite la trazabilidad del sensor en futuras calibraciones.

### Conclusiones

1. El sensor DS18B20 **funciona correctamente**.
2. La comunicación OneWire es estable con la resistencia pull-up de 4.7 kΩ.
3. El sensor responde adecuadamente a cambios de temperatura.
4. La precisión de **±0.06 °C** es suficiente para el monitoreo del biodigestor (rango mesofílico: 30–40 °C).

### Evidencia Fotográfica

[Pendiente: Insertar foto de la conexión en protoboard]
