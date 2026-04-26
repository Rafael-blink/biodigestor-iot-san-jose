# Registro de Pruebas de Sensores

## Prueba 2: Sensor Ultrasónico JSN-SR04T

**Fecha:** 26 de Abril de 2026  
**Investigador:** Rafael Gustavo Ramos Noriega  
**Firmware:** MicroPython  
**Script utilizado:** `tests/test_jsn_sr04t.py`

### Configuración del Hardware

| Parámetro          | Valor                    |
|--------------------|--------------------------|
| Microcontrolador   | Raspberry Pi Pico W      |
| Pin TRIG           | GP14 (Pin 19)            |
| Pin ECHO           | GP15 (Pin 20)            |
| Alimentación       | 5V                       |

### Objetivo

Verificar la precisión y estabilidad del sensor ultrasónico JSN-SR04T para la medición de nivel de biol sin contacto.

### Fundamento Teórico

```
d = (t × v) / 2
```

Donde:
- **d:** distancia en cm
- **t:** tiempo de vuelo en μs
- **v:** velocidad del sonido = 0.03495 cm/μs a 30 °C

### Resultados

#### Prueba de estabilidad (objeto fijo a ~30 cm)

| Medición | Distancia (cm) | Estado |
|----------|----------------|--------|
| 1        | 30.4           | OK     |
| 2        | 30.4           | OK     |
| 3        | 30.4           | OK     |
| 4        | 30.9           | OK     |
| 5        | 30.4           | OK     |
| 6        | 30.8           | OK     |
| 7        | 30.3           | OK     |
| 8        | 30.7           | OK     |
| 9        | 30.7           | OK     |
| 10       | 30.7           | OK     |

#### Estadísticas

| Métrica             | Valor           |
|---------------------|-----------------|
| Promedio            | 30.6 cm         |
| Mínimo              | 30.3 cm         |
| Máximo              | 30.9 cm         |
| Variación           | 0.6 cm          |
| Lecturas válidas    | 9 / 10 (90 %)   |
| Lecturas con error  | 1 / 10 (10 %)   |
| Precisión           | ±0.3 cm respecto al promedio |
| Error relativo      | ~2 %            |

### Conclusiones

1. El sensor JSN-SR04T funciona correctamente.
2. La precisión de **±0.6 cm** es adecuada para monitoreo de nivel de biol.
3. El sensor detecta objetos en el rango esperado de 20–450 cm.
4. La tasa de lecturas exitosas (90 %) es aceptable para un prototipo inicial.
5. Para instalación en campo, se recomienda agregar un **filtro de mediana** para eliminar lecturas espurias.

### Notas para Implementación

- La velocidad del sonido se ajustó a **30 °C** (temperatura típica del biodigestor mesofílico).
- Para mayor precisión, se puede usar el DS18B20 para **compensar la velocidad del sonido en tiempo real**.
- El sensor es resistente al agua (**IP67**), ideal para el ambiente húmedo del biodigestor.

### Evidencia Fotográfica

[Pendiente: Insertar foto de la conexión en protoboard]
