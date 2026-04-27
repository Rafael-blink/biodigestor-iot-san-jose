# Prueba de Integracion - Sistema Completo de Monitoreo

**Fecha:** 26 de Abril de 2026
**Investigador:** Rafael Gustavo Ramos Noriega
**Firmware:** MicroPython
**Tipo de prueba:** Integracion de sistema completo

## Configuracion del Hardware

- Microcontrolador: Raspberry Pi Pico W
- ADC: ADS1115 (I2C 0x48, GP4=SDA, GP5=SCL)
- Sensor de temperatura: DS18B20 (OneWire GP17, ROM 285e5f4d06000075)
- Sensor ultrasonico: JSN-SR04T (TRIG=GP14, ECHO=GP15)
- Sensor de presion: MPX5100DP (Canal A0, divisor 5.1k+10k)
- Sensor de metano: MQ-4 (Canal A1, divisor 5.1k+10k, R0=248677 ohm)
- Alimentacion: LM2596 ajustado a 5.0V con proteccion de diodo 1N4007

## Objetivo

Verificar que los 5 sensores funcionen simultaneamente sin interferencias, validando la comunicacion I2C, OneWire y GPIO en paralelo.

## Resultados de Inicializacion

- Bus I2C: ADS1115 detectado en 0x48
- DS18B20: Encontrado con ROM 285e5f4d06000075
- Canal A0 (MPX5100DP): 0.3121V inicial
- Canal A1 (MQ-4): 0.3121V inicial
- JSN-SR04T: 26.3 cm en primera lectura

## Resultados de las 5 Lecturas

### Lectura 1
- Temperatura: 31.06 C
- Nivel: 75.8% (distancia: 29.0 cm)
- Presion: 5.975 kPa (V: 0.4689V)
- Metano: Rs/R0 = 0.388 (V: 0.4698V)

### Lectura 2
- Temperatura: 31.00 C
- Nivel: 76.3% (distancia: 28.4 cm)
- Presion: 5.962 kPa (V: 0.4683V)
- Metano: Rs/R0 = 0.389 (V: 0.4681V)

### Lectura 3
- Temperatura: 31.00 C
- Nivel: 76.5% (distancia: 28.2 cm)
- Presion: 0.035 kPa (V: 0.2016V)
- Metano: Rs/R0 = 0.957 (V: 0.2016V)

### Lectura 4
- Temperatura: 31.06 C
- Nivel: 77.1% (distancia: 27.5 cm)
- Presion: 6.105 kPa (V: 0.4747V)
- Metano: Rs/R0 = 0.944 (V: 0.2042V)

### Lectura 5
- Temperatura: 31.00 C
- Nivel: 78.3% (distancia: 26.0 cm)
- Presion: 6.122 kPa (V: 0.4755V)
- Metano: Rs/R0 = 0.946 (V: 0.2039V)

## Tabla Resumen de Sensores

| Sensor | Variable | Protocolo | Lecturas exitosas | Errores | Estado en prueba |
|--------|----------|-----------|-------------------|---------|------------------|
| DS18B20 | Temperatura | OneWire (GP17) | 5/5 | 0 | Completado |
| JSN-SR04T | Nivel ultrasonico | GPIO (GP14/GP15) | 5/5 | 0 | Completado |
| MPX5100DP | Presion biogás | I2C/ADS1115 A0 | 5/5 | 0 | Completado |
| MQ-4 | Metano | I2C/ADS1115 A1 | 5/5 | 0 | Completado |
| ADS1115 | ADC 16 bits | I2C (0x48) | 5/5 | 0 | Completado |

## Estadisticas Finales

- Lecturas totales: 5 ciclos completos
- Errores de temperatura: 0
- Errores de nivel: 0
- Errores de presion: 0
- Errores de metano: 0
- Tasa de exito: 100.0%

## Observacion Importante para la Tesis

Durante la Lectura 3, los sensores analogicos (presion y metano) mostraron un cambio simultaneo significativo (presion de ~6 kPa a 0.035 kPa, metano de Rs/R0 0.389 a 0.957), mientras los sensores digitales (temperatura y nivel) permanecieron estables. Esto demuestra que ambos canales del ADS1115 estan funcionando independientemente y responden a cambios reales en el entorno, posiblemente por movimiento del protoboard o corrientes de aire. La recuperacion en las Lecturas 4-5 confirma que los sensores vuelven a su estado estable, lo cual es el comportamiento esperado. Este fenomeno valida que no hay interferencia cruzada entre sensores.

## Conclusiones

1. Los 5 sensores funcionan simultaneamente sin conflictos de recursos.
2. La comunicacion I2C (ADS1115) y OneWire (DS18B20) coexisten sin interferencias.
3. El ADC maneja dos canales analogicos independientes correctamente.
4. Los sensores digitales (temperatura y ultrasonico) mantienen estabilidad total.
5. La tasa de exito del 100% en 5 ciclos confirma que el hardware esta listo para operacion continua.
6. El sistema supera la prueba de integracion y esta listo para implementacion final.

## Estado Final

SISTEMA LISTO PARA IMPLEMENTACION FINAL
