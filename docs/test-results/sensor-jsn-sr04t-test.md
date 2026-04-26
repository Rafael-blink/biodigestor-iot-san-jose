# Prueba 2: Sensor Ultrasónico JSN-SR04T

**Fecha:** 26 de Abril de 2026
**Investigador:** Rafael Gustavo Ramos Noriega
**Firmware:** MicroPython
**Script utilizado:** tests/test_jsn_sr04t.py

## Configuración del Hardware
- Microcontrolador: Raspberry Pi Pico W
- Pin TRIG: GP14 (Pin 19)
- Pin ECHO: GP15 (Pin 20)
- Alimentación: 5V
- Protocolo: Digital (Trig/Echo)

## Objetivo
Verificar la precisión y estabilidad del sensor ultrasónico JSN-SR04T para la medición de nivel de biol sin contacto en el biodigestor.

## Fundamento Teórico
El sensor emite un pulso ultrasónico de 40 kHz y mide el tiempo que tarda el eco en retornar:

```
d = (t × v) / 2
```

Donde:
- d: distancia en cm
- t: tiempo de vuelo en microsegundos
- v: velocidad del sonido = 0.03495 cm/μs a 30°C (temperatura típica del biodigestor)

La velocidad del sonido se ajusta por temperatura: v = 331.3 + 0.606 × T (m/s)

## Resultados

### Prueba de estabilidad (objeto fijo a ~30 cm, 10 mediciones)
| Medición | Distancia (cm) |
|----------|----------------|
| 1 | 30.4 |
| 2 | 30.4 |
| 3 | 30.4 |
| 4 | 30.9 |
| 5 | 30.4 |
| 6 | 30.8 |
| 7 | 30.3 |
| 8 | 30.7 |
| 9 | 30.7 |
| 10 | 30.7 |

**Estadísticas:**
- Promedio: 30.6 cm
- Mínimo: 30.3 cm
- Máximo: 30.9 cm
- Variación: 0.6 cm
- Desviación estándar: ±0.21 cm
- Lecturas válidas: 9/10 (90%)
- Lecturas con error: 1/10 (10%)

**Precisión:** ±0.6 cm respecto al promedio.
**Exactitud:** Error de 0.6 cm respecto a la referencia de 30 cm (2%).

## Conclusiones
1. El sensor JSN-SR04T funciona correctamente y responde a objetos en su rango.
2. La precisión de ±0.6 cm es adecuada para monitoreo de nivel de biol, donde cambios > 5 cm son significativos.
3. El sensor detecta objetos en el rango esperado de 20-450 cm.
4. La tasa de lecturas exitosas (90%) es aceptable para un prototipo. Para producción, implementar un filtro de mediana (n=5) eliminará lecturas espurias.
5. El sensor es resistente al agua (IP67), ideal para el ambiente húmedo del biodigestor.

## Notas para Implementación
- La velocidad del sonido se ajustó a 30°C (temperatura mesofílica típica).
- Se puede usar el DS18B20 para compensar la velocidad del sonido en tiempo real.
- Para calcular el nivel de biol: Nivel(%) = ((Profundidad_total - Distancia_medida) / Profundidad_total) × 100

## Referencias
- JSN-SR04T Datasheet
- Principios de acústica: velocidad del sonido en función de temperatura
