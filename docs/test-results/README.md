# Registro de Pruebas de Sensores

## Proyecto: Sistema de Monitoreo IoT para Biodigestor Anaeróbico
## Institución: Educativa San José de Canalete, Córdoba
## Investigador: Rafael Gustavo Ramos Noriega
## Tech4Good Challenge 2025

### Sensores Probados

| Sensor | Variable | Fecha | Estado | Precisión | Archivo |
|--------|----------|-------|--------|-----------|---------|
| DS18B20 | Temperatura | 26 Abr 2026 | Completado | ±0.06°C | <a>sensor-ds18b20-test.md</a> |
| JSN-SR04T | Nivel ultrasónico | 26 Abr 2026 | Completado | ±0.6 cm | <a>sensor-jsn-sr04t-test.md</a> |
| ADS1115 | ADC 16 bits | 27 Abr 2026 | Completado | 16 bits | <a>sensor-ads1115-test.md</a> |
| MPX5100DP | Presión biogás | 27 Abr 2026 | Completado | ±0.01 kPa | <a>sensor-mpx5100dp-test.md</a> |
| MQ-4 | Metano | 26 Abr 2026 | Completado | Cualitativo (Rs/R0) | [sensor-mq4-test.md](sensor-mq4-test.md) |
| Sistema completo | Integracion de 5 sensores | 26 Abr 2026 | Completado | 100% exito | [integration-test.md](integration-test.md) |

### Resumen de Resultados

Todas las pruebas han sido completadas exitosamente. Los cinco sensores responden según las especificaciones de sus datasheets con ligeras variaciones atribuibles a tolerancias de fabricación y condiciones ambientales.

### Siguientes Pasos
1. Integracion de los cinco sensores en el firmware principal
2. Integracion con Azure IoT Central
3. Prueba de campo en la Institucion Educativa San Jose de Canalete
