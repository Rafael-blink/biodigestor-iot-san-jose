# Prueba 4: Sensor de Presión MPX5100DP

**Fecha:** 27 de Abril de 2026
**Investigador:** Rafael Gustavo Ramos Noriega
**Firmware:** MicroPython
**Script utilizado:** tests/test_mpx5100dp.py

## Configuración del Hardware
- Microcontrolador: Raspberry Pi Pico W
- ADC: ADS1115 canal A0 (I2C 0x48)
- Acondicionamiento: Divisor de voltaje R1=5.1kΩ, R2=10kΩ
- Factor de divisor: 1.51x (para reconstruir voltaje original)
- Alimentación del sensor: 5.0V

## Objetivo
Verificar la función de transferencia, el voltaje de offset y la respuesta dinámica del sensor de presión diferencial MPX5100DP.

## Fundamento Teórico
El MPX5100DP es un sensor piezorresistivo con compensación de temperatura. Su función de transferencia es:

```
Vout = Vs × (0.009 × P + 0.04) ± Error
P = ((Vout / Vs) - 0.04) / 0.009
```

Donde:
- Vout: voltaje de salida del sensor
- Vs: voltaje de alimentación (5.0V nominal)
- P: presión diferencial en kPa
- Error: ±2.5% máximo sobre el rango completo

El divisor de voltaje (5.1kΩ + 10kΩ) reduce la señal de 0-5V a 0-3.3V para proteger el ADC:

```
V_ADC = V_sensor × (R2 / (R1 + R2)) = V_sensor × 0.662
```

## Resultados

### Prueba de estabilidad a presión atmosférica (10 mediciones)
| Lectura | V_ADC (V) | V_Sensor (V) | Valor Crudo | Presión (kPa) |
|---------|-----------|--------------|-------------|---------------|
| 1 | 0.1543 | 0.2329 | 1234 | 0.732 |
| 2 | 0.1541 | 0.2327 | 1233 | 0.727 |
| 3 | 0.1541 | 0.2327 | 1233 | 0.727 |
| 4 | 0.1543 | 0.2329 | 1234 | 0.732 |
| 5 | 0.1540 | 0.2325 | 1232 | 0.723 |
| 6 | 0.1545 | 0.2333 | 1236 | 0.740 |
| 7 | 0.1541 | 0.2327 | 1233 | 0.727 |
| 8 | 0.1544 | 0.2331 | 1235 | 0.736 |
| 9 | 0.1545 | 0.2333 | 1236 | 0.740 |
| 10 | 0.1546 | 0.2335 | 1237 | 0.744 |

**Estadísticas:**
- V_ADC promedio: 0.1543V
- V_Sensor promedio: 0.2330V
- Presión promedio: 0.733 kPa
- Variación máxima: ±0.006 kPa

### Medición del voltaje de offset (10 mediciones)
| Medición | V_Sensor (V) |
|----------|--------------|
| 1 | 0.2335 |
| 2 | 0.2341 |
| 3 | 0.2339 |
| 4 | 0.2339 |
| 5 | 0.2339 |
| 6 | 0.2339 |
| 7 | 0.2337 |
| 8 | 0.2337 |
| 9 | 0.2342 |
| 10 | 0.2339 |

- **Offset promedio medido:** 0.2338 V
- **Offset esperado (datasheet):** Vs × 0.04 = 0.2000 V
- **Diferencia:** +0.0338 V (16.9% de error relativo)

### Respuesta a presión positiva (soplido suave)
Se aplicó presión mediante soplido en el puerto P1:

La presión respondió con picos de hasta 0.790 kPa durante la aplicación de presión, retornando a valores base (~0.73 kPa) al cesar el estímulo. Se observó que cuando el voltaje del sensor caía por debajo del umbral Vs × 0.04 (~0.2V), la fórmula de transferencia producía valores de 0.000 kPa como protección contra presiones negativas.

## Análisis

### Offset real vs teórico
La diferencia de +0.0338V (16.9%) entre el offset medido y el teórico se atribuye a:
1. Tolerancia del sensor: ±2.5% sobre el rango completo (100 kPa)
2. Tolerancia de las resistencias del divisor: ±5% típico
3. Variaciones en el voltaje de alimentación real (puede no ser exactamente 5.0V)

### Protección contra presión negativa
El sensor es diferencial y puede medir presiones negativas. Sin embargo, en la implementación actual, cuando V_sensor < Vs × 0.04, la presión se fuerza a 0.000 kPa para evitar valores negativos que no tendrían sentido físico en el biodigestor (la presión del biogás siempre es positiva o cero respecto a la atmosférica).

## Conclusiones
1. El sensor MPX5100DP funciona correctamente y responde a cambios de presión.
2. El offset medido (0.2338V) difiere del teórico (0.2000V) en +16.9%, lo cual es aceptable considerando las tolerancias del sistema.
3. La estabilidad de lecturas (±0.006 kPa) es excelente para el monitoreo de biodigestores de baja presión.
4. El divisor de voltaje (5.1kΩ + 10kΩ) protege adecuadamente el ADC, manteniendo las señales dentro del rango seguro de 0-3.3V.
5. Se recomienda recalibrar el offset una vez instalado el sensor en el biodigestor real.

## Recomendaciones para el sistema final
1. Implementar un filtro de mediana móvil (n=5) para suavizar fluctuaciones.
2. Realizar calibración de 2 puntos (0 kPa y presión conocida) para ajustar la función de transferencia.
3. Usar el voltaje de alimentación real medido (no asumir 5.0V exactos) para mayor precisión.

## Referencias
- NXP MPX5100DP Datasheet
- Principios de sensores piezorresistivos
- MicroPython I2C Documentation
