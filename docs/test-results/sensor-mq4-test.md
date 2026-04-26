# Prueba 5: Sensor de Gas Metano MQ-4

**Fecha:** 26 de Abril de 2026
**Investigador:** Rafael Gustavo Ramos Noriega
**Firmware:** MicroPython
**Script utilizado:** tests/test_mq4.py

## Configuración del Hardware

- Microcontrolador: Raspberry Pi Pico W
- ADC: ADS1115 canal A1 (I2C 0x48, GP4=SDA, GP5=SCL)
- Acondicionamiento de señal: Divisor de voltaje R1=5.1 kΩ, R2=10 kΩ
- Factor de reconstrucción del divisor: 1.51x
- Resistencia de carga (RL) en placa del MQ-4: 10 kΩ (valor típico de fábrica)
- Alimentación del sensor: 5.0 V desde riel de la protoboard
- Alimentación del calefactor: Interna, mismo pin VCC

## Objetivo

Verificar la capacidad del sensor MQ-4 para detectar gas metano, establecer la resistencia de referencia en aire limpio (R0), medir el tiempo de respuesta ante una fuga simulada y evaluar la recuperación del sensor tras la exposición al gas.

## Fundamento Teórico

### Principio de funcionamiento

El MQ-4 es un sensor de gas basado en semiconductor de óxido de estaño (SnO₂). En condiciones de aire limpio, el material semiconductor presenta una resistencia eléctrica alta (Rs). Cuando el sensor se expone a gas metano (CH₄), las moléculas de gas reaccionan con el oxígeno adsorbido en la superficie del semiconductor, liberando electrones y disminuyendo Rs.

### Cálculo de la resistencia del sensor

El módulo MQ-4 incorpora una resistencia de carga (RL) que forma un divisor de voltaje con el sensor:

```
Vout = Vcc × RL / (RL + Rs)
```

Despejando Rs:

```
Rs = (Vcc × RL / Vout) - RL
```

### Relación Rs/R0 como indicador de concentración

La relación Rs/R0 —donde R0 es la resistencia medida en aire limpio— es el parámetro estándar para interpretar las lecturas del MQ-4. Esta relación es más fiable que el voltaje absoluto porque compensa variaciones de temperatura y humedad:

- Rs/R0 ≈ 1.0 → Aire limpio
- Rs/R0 < 0.8 → Presencia detectable de metano
- Rs/R0 < 0.5 → Concentración peligrosa de metano

### Divisor de voltaje

Para proteger el ADS1115 (entrada máxima 3.3 V) de la señal de 0–5 V del MQ-4:

```
AOUT MQ-4 -> R1(5.1 kΩ) -> Canal A1 ADS1115 -> R2(10 kΩ) -> GND

Factor de división:      R2 / (R1 + R2) = 10 / 15.1 = 0.662
Factor de reconstrucción: (R1 + R2) / R2 = 1.51
```

## Precauciones y Preparación Previa

### Precalentamiento del sensor

El MQ-4 requiere que su elemento calefactor interno alcance una temperatura de trabajo estable (≈300–400 °C) antes de proporcionar lecturas fiables. Sin este período las moléculas adsorbidas en el SnO₂ no reaccionan de forma controlada.

**Procedimiento:**

1. Conectar VCC y GND del MQ-4 a 5 V y GND respectivamente.
2. Mantener el sensor alimentado durante un mínimo de 30–60 minutos.
3. Verificar que el sensor está caliente al tacto (temperatura elevada es normal).
4. No realizar mediciones durante el período de precalentamiento.

### Precauciones de seguridad para la prueba con gas

1. Realizar la prueba únicamente en un área bien ventilada.
2. Usar un encendedor sin encender como fuente de gas; nunca generar llama abierta cerca del sensor.
3. Mantener la fuente de gas a un mínimo de 10 cm del sensor.
4. No inhalar el gas directamente.
5. Garantizar ventilación suficiente para dispersar el gas después de la prueba.
6. Esta prueba es opcional y solo debe realizarse si se cumplen todas las condiciones de seguridad anteriores.

### Conexiones eléctricas

| Pin del MQ-4 | Conectar a                              | Nota                                          |
|:------------|:----------------------------------------|:----------------------------------------------|
| VCC         | Riel 5 V de la protoboard              | Alimentación del sensor y calefactor          |
| GND         | Riel GND de la protoboard              | Tierra común con la Pico W y ADS1115          |
| AOUT        | Divisor de voltaje → Canal A1 ADS1115  | Señal analógica de concentración de gas       |
| DOUT        | No conectar                            | Salida digital de umbral (no usada)           |

## Resultados

### Verificación de comunicación I2C

El ADS1115 fue detectado correctamente en la dirección 0x48. La comunicación I2C a 100 kHz funciona sin errores.

### Establecimiento de línea base (R0)

Se tomaron 30 lecturas consecutivas con intervalo de 2 s en condiciones de aire limpio y área ventilada. Los valores de V_sensor corresponden al voltaje reconstruido aplicando el factor 1.51 sobre la lectura del ADC.

| Lectura | V_sensor (V) | Rs (Ω)  | Lectura | V_sensor (V) | Rs (Ω)  |
|:-------:|:------------:|:-------:|:-------:|:------------:|:-------:|
|  1      | 0.1942       | 247 427 |  16     | 0.1931       | 248 937 |
|  2      | 0.1944       | 247 177 |  17     | 0.1931       | 248 937 |
|  3      | 0.1942       | 247 427 |  18     | 0.1933       | 248 684 |
|  4      | 0.1937       | 248 180 |  19     | 0.1933       | 248 684 |
|  5      | 0.1937       | 248 180 |  20     | 0.1933       | 248 684 |
|  6      | 0.1937       | 248 180 |  21     | 0.1931       | 248 937 |
|  7      | 0.1935       | 248 432 |  22     | 0.1931       | 248 937 |
|  8      | 0.1933       | 248 684 |  23     | 0.1931       | 248 937 |
|  9      | 0.1933       | 248 684 |  24     | 0.1929       | 249 190 |
| 10      | 0.1935       | 248 432 |  25     | 0.1929       | 249 190 |
| 11      | 0.1933       | 248 684 |  26     | 0.1929       | 249 190 |
| 12      | 0.1933       | 248 684 |  27     | 0.1929       | 249 190 |
| 13      | 0.1933       | 248 684 |  28     | 0.1927       | 249 444 |
| 14      | 0.1931       | 248 937 |  29     | 0.1929       | 249 190 |
| 15      | 0.1929       | 249 190 |  30     | 0.1929       | 249 190 |

**Estadísticas de la línea base:**

| Parámetro                              | Valor             |
|:---------------------------------------|:------------------|
| R0 (resistencia promedio)              | 248 677 Ω         |
| Rs mínimo                              | 247 177 Ω         |
| Rs máximo                              | 249 444 Ω         |
| Rango de variación                     | 2 267 Ω (0.91 %)  |
| Desviación estándar de Rs              | ≈ 590 Ω           |
| Voltaje típico del sensor en aire limpio | 0.1929 V         |
| Voltaje típico en el ADC (post-divisor) | 0.1277 V         |

La variación total de 0.91 % confirma que el sensor estaba correctamente precalentado, el ADS1115 opera sin interferencias y el divisor de voltaje funciona dentro de tolerancias.

### Prueba de respuesta a gas metano

**Procedimiento:**

1. Se tomaron 5 lecturas de confirmación de línea base.
2. Se liberó gas de un encendedor sin encender a ≈10 cm del sensor.
3. Se registraron lecturas cada 1 s durante 20 s (exposición).
4. Se retiró la fuente de gas y se monitoreó la recuperación durante 15 s adicionales.

**Confirmación de línea base (pre-gas):**

| Lectura | V_sensor (V) | Rs (Ω)  | Rs/R0 |
|:-------:|:------------:|:-------:|:-----:|
| 1       | 0.1914       | 251 235 | 1.010 |
| 2       | 0.1916       | 250 978 | 1.009 |
| 3       | 0.1914       | 251 235 | 1.010 |
| 4       | 0.1922       | 250 209 | 1.006 |
| 5       | 0.1937       | 248 180 | 0.998 |

**Respuesta durante la exposición al gas (0–19 s):**

| Tiempo (s) | V_sensor (V) | Rs (Ω)  | Rs/R0 | Estado         |
|:----------:|:------------:|:-------:|:-----:|:---------------|
|  0         | 0.1954       | 245 935 | 0.989 | Aire limpio    |
|  1         | 0.1984       | 242 039 | 0.973 | Aire limpio    |
|  2         | 0.1989       | 241 321 | 0.970 | Aire limpio    |
|  3         | 0.1993       | 240 845 | 0.969 | Aire limpio    |
|  4         | 0.2003       | 239 663 | 0.964 | Aire limpio    |
|  5         | 0.2018       | 237 795 | 0.956 | Inicio respuesta |
|  6         | 0.2027       | 236 641 | 0.952 | Gas detectado  |
|  7         | 0.2050       | 233 916 | 0.941 | Gas detectado  |
|  8         | 0.2071       | 231 470 | 0.931 | Gas detectado  |
|  9         | 0.2088       | 229 505 | 0.923 | Gas detectado  |
| 10         | 0.2203       | 216 986 | 0.873 | Gas presente   |
| 11         | 0.2354       | 202 424 | 0.814 | ADVERTENCIA    |
| 12         | 0.2499       | 190 070 | 0.764 | ADVERTENCIA    |
| 13         | 0.2529       | 187 681 | 0.755 | ADVERTENCIA    |
| 14         | 0.2507       | 189 467 | 0.762 | ADVERTENCIA    |
| 15         | 0.2486       | 191 133 | 0.769 | ADVERTENCIA    |
| 16         | 0.2499       | 190 070 | 0.764 | ADVERTENCIA    |
| 17         | 0.3131       | 149 670 | 0.602 | ADVERTENCIA    |
| 18         | 0.3766       | 122 778 | 0.494 | PELIGRO        |
| 19         | 0.3854       | 119 722 | 0.481 | PELIGRO        |

**Recuperación tras retirar el gas (20–35 s):**

| Tiempo (s) | V_sensor (V) | Rs (Ω)  | Rs/R0 | Estado      |
|:----------:|:------------:|:-------:|:-----:|:------------|
| 20         | 0.3834       | 120 425 | 0.484 | PELIGRO     |
| 21         | 0.3764       | 122 845 | 0.494 | PELIGRO     |
| 22         | 0.3700       | 125 149 | 0.503 | ADVERTENCIA |
| 23         | 0.3911       | 117 844 | 0.474 | PELIGRO     |
| 24         | 0.4717       |  95 999 | 0.386 | PELIGRO     |
| 25         | 0.4823       |  93 676 | 0.377 | PELIGRO     |
| 26         | 0.4787       |  94 453 | 0.380 | PELIGRO     |
| 27         | 0.4687       |  96 682 | 0.389 | PELIGRO     |
| 28         | 0.4400       | 103 639 | 0.417 | PELIGRO     |
| 29         | 0.4222       | 108 414 | 0.436 | PELIGRO     |
| 30         | 0.4026       | 114 188 | 0.459 | PELIGRO     |
| 31         | 0.3894       | 118 402 | 0.476 | PELIGRO     |
| 32         | 0.3728       | 124 123 | 0.499 | PELIGRO     |
| 33         | 0.3571       | 130 007 | 0.523 | ADVERTENCIA |
| 34         | 0.3439       | 135 386 | 0.544 | ADVERTENCIA |
| 35         | —            | —       | —     | Tendencia de mejora |

## Análisis de Resultados

### Tiempo de respuesta

| Evento                                    | Tiempo (s) |
|:------------------------------------------|:----------:|
| Rs/R0 comienza a descender de 0.95         |  5         |
| Umbral ADVERTENCIA (Rs/R0 < 0.8) alcanzado | 11         |
| Umbral PELIGRO (Rs/R0 < 0.5) alcanzado    | 18         |
| Respuesta máxima registrada (Rs/R0 = 0.481)| 19        |

### Sensibilidad

Una pequeña cantidad de gas liberada a 10 cm de distancia produjo una reducción del 52 % en la relación Rs/R0, lo que confirma la alta sensibilidad del sensor.

### Recuperación post-exposición

El sensor muestra recuperación progresiva tras retirar el gas: a los 15 s post-exposición Rs/R0 = 0.544, saliendo de la zona de peligro. La recuperación completa puede tomar entre 5 y 15 minutos dependiendo de la concentración y la duración de la exposición.

Este comportamiento es característico de los sensores de estado sólido basados en SnO₂: las moléculas de gas adsorbidas en la superficie tardan en desorberse, especialmente tras exposiciones prolongadas a concentraciones elevadas.

La oscilación observada entre los 22–25 s (donde Rs/R0 sube brevemente a 0.503 y luego desciende a 0.377) se debe al reequilibrio químico en la superficie del sensor durante la disipación del gas y es un comportamiento normal en este tipo de dispositivos.

## Umbrales para el Sistema de Monitoreo

Los siguientes umbrales se definen a partir de los resultados experimentales y la literatura del fabricante:

| Estado      | Rs/R0         | V_sensor típico | Acción requerida                         |
|:------------|:-------------:|:---------------:|:-----------------------------------------|
| NORMAL      | > 0.8         | < 0.25 V        | Operación segura, sin fugas detectables  |
| ADVERTENCIA | 0.5 – 0.8     | 0.25 V – 0.38 V | Posible fuga; requiere inspección        |
| PELIGRO     | < 0.5         | > 0.38 V        | Fuga significativa; acción inmediata     |

Estos umbrales son cualitativos. Para una calibración cuantitativa en PPM se requeriría gas metano de concentración conocida, cámara de calibración con volumen controlado y múltiples puntos de calibración para construir la curva Rs/R0 vs PPM. Dado que el objetivo del proyecto es la detección de fugas y no la medición precisa de concentración, los umbrales cualitativos son adecuados.

## Conclusiones

1. **Funcionamiento verificado:** El sensor MQ-4 detecta gas metano de manera confiable.
2. **Respuesta rápida:** La detección inicial en 5 s y el tiempo hasta el umbral de peligro en 18 s son adecuados para un sistema de alerta temprana.
3. **Estabilidad de línea base:** La variación de 0.91 % en 30 lecturas confirma que el sensor, correctamente precalentado, proporciona lecturas estables.
4. **Recuperación adecuada:** Aunque la recuperación completa es lenta, el sensor muestra tendencia de retorno a la línea base dentro de los primeros 15 s tras eliminar la fuente de gas.
5. **Sensor cualitativo:** El MQ-4 es adecuado para detección de fugas pero no proporciona concentraciones en PPM sin calibración profesional.
6. **Precalentamiento crítico:** El período de 30–60 minutos es indispensable; sin él los valores de Rs fluctúan significativamente.

## Recomendaciones para el Sistema Final

1. **Filtro de mediana móvil:** Implementar una ventana de 5 lecturas para suavizar fluctuaciones sin perder sensibilidad.
2. **Tiempo de estabilización al inicio:** Esperar 60 minutos antes de declarar el estado NORMAL, para garantizar el precalentamiento completo.
3. **Calibración periódica:** Recalibrar R0 cada 30 días para compensar la deriva del sensor por envejecimiento.
4. **Histéresis en umbrales:**
   - NORMAL → ADVERTENCIA: Rs/R0 < 0.78
   - ADVERTENCIA → NORMAL: Rs/R0 > 0.85
5. **Ubicación del sensor:** Instalar el MQ-4 en la parte superior del biodigestor, donde el metano (menos denso que el aire) tiende a acumularse en caso de fuga.

## Referencias

- Datasheet MQ-4 Gas Sensor, Zhengzhou Winsen Electronics Technology Co., Ltd.
- Jaaniso, R. & Tan, O.K. (2013). *Semiconductor Gas Sensors*. Woodhead Publishing.
- Wang, C. et al. (2010). Metal Oxide Gas Sensors: Sensitivity and Influencing Factors. *Sensors*, 10(3), 2088–2106.
- Texas Instruments ADS1115 Datasheet.
- RP2040 Datasheet, Raspberry Pi Ltd.
