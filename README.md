# 🌱 Sistema de Monitoreo IoT para Biodigestor Anaeróbico

[![Tech4Good](https://img.shields.io/badge/Tech4Good-Challenge%202025-blue?style=for-the-badge)](https://tech4goodchallenge.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python](https://img.shields.io/badge/MicroPython-3.4+-green.svg)](https://micropython.org/)
[![Hardware](https://img.shields.io/badge/Raspberry%20Pi-Pico%20W-red.svg)](https://www.raspberrypi.com/products/raspberry-pi-pico/)

## 📋 Descripción

Sistema de monitoreo IoT de código abierto para un biodigestor anaeróbico tubular instalado en la **Institución Educativa San José de Canalete**, Córdoba, Colombia. Este proyecto transforma residuos pecuarios y vegetales en biogás para generar energía eléctrica, garantizando la continuidad de las actividades académicas en una zona rural con suministro eléctrico inestable.

El sistema monitorea en tiempo real variables críticas del proceso de digestión anaeróbica: temperatura, presión del biogás, nivel de biol, concentración de metano y pH, utilizando una Raspberry Pi Pico W y sensores de bajo costo. Los datos se transmiten a una plataforma IoT para visualización y análisis remoto.

## 🎯 El Problema que Resolvemos

En el municipio de Canalete, Córdoba, la inestabilidad del suministro eléctrico afecta significativamente a las instituciones educativas rurales:
- ❌ Interrupción de clases por cortes de energía
- ❌ Imposibilidad de usar equipos pedagógicos esenciales
- ❌ Gestión inadecuada de residuos orgánicos que genera emisiones de metano

##  Nuestra Solución

Un sistema de biodigestión anaeróbica con monitoreo inteligente que:
- ✅ Genera energía eléctrica a partir de residuos pecuarios locales
- ✅ Monitorea variables críticas en tiempo real con IoT
- ✅ Capacita a la comunidad educativa en tecnologías sostenibles
- ✅ Promueve un modelo de economía circular replicable

##  Arquitectura del Sistema

```mermaid
graph TD
    A[Biodigestor Tubular] --> B[Sensores]
    B --> C[Raspberry Pi Pico W]
    C --> D[Procesamiento Local]
    D --> E[ThingSpeak IoT]
    D --> F[Control de Alarmas]
    E --> G[Dashboard Web]
    F --> H[LEDs de Estado]
    D --> I[Consola Serial]
    
    B1[DS18B20 Temperatura] --> B
    B2[JSN-SR04T Nivel Biol] --> B
    B3[MPX5100DP Presión] --> B
    B4[MQ-4 Metano] --> B

Componentes del Sistema

Microcontrolador	Raspberry Pi Pico W	
Temperatura	DS18B20	
Nivel	JSN-SR04T	
Presión	MPX5100DP	
Gas Metano	MQ-4	
ADC	ADS1115	
Alimentación	LM2596	

Instalación Rápida
Requisitos Previos
Raspberry Pi Pico W con firmware MicroPython

Thonny IDE instalado en tu computadora

Componentes listados en BOM.md

Configuración
Clona este repositorio

Abre src/config.py y configura:

Credenciales WiFi (SSID, PASSWORD)

API Key de ThingSpeak (THINGSPEAK_API_KEY)

Umbrales de tu biodigestor

Conecta los sensores siguiendo el diagrama en hardware/

Copia todos los archivos de src/ a tu Pico W

Ejecuta main.py

Variables Monitoreadas
Variable	Sensor	Rango Típico	Umbral Crítico
Temperatura	DS18B20	30-40°C	< 25°C o > 45°C
Nivel de Biol	JSN-SR04T	20-80 cm	> 90 cm (rebose)
Presión Biogás	MPX5100DP	0-5 kPa	> 10 kPa
Metano	MQ-4	< 1000 PPM	> 5000 PPM
pH (futuro)	Genérico	6.5-7.5	< 6.0 o > 8.0

Impacto Educativo y Social
Este proyecto incluye:

2 talleres teórico-prácticos para docentes, estudiantes y personal

Material didáctico: manuales, infografías y videos educativos

Plan de mantenimiento comunitario para sostenibilidad a largo plazo

Participación en eventos de apropiación social del conocimiento

Tech4Good Challenge 
Este proyecto participa en el Tech4Good Challenge, una iniciativa que busca soluciones tecnológicas innovadoras para problemas sociales y ambientales. Nuestro sistema aborda:

ODS 4: Educación de Calidad

ODS 7: Energía Asequible y No Contaminante

ODS 13: Acción por el Clima

👥 Equipo
Investigador Principal	Rafael Gustavo Ramos Noriega	Universidad Pontificia Bolivariana Ingeniería Electrónica
Directora	Ana Milena López López	Universidad Pontificia Bolivariana
Co-director	Carlos Andrés Marenco Porto	Universidad Pontificia Bolivariana

📄 Licencia
Este proyecto está bajo la Licencia MIT - vea el archivo LICENSE para más detalles.

El hardware es open source bajo CERN Open Hardware Licence Version 2 - Strongly Reciprocal (CERN-OHL-S-2.0).

📚 Documentación Adicional
Especificación Técnica

Guía de Ensamblaje

Guía de Implementación

Impacto Tech4Good

🤝 Cómo Contribuir
¡Las contribuciones son bienvenidas! Por favor lee CONTRIBUTING_ES.md para conocer nuestras pautas.

Contacto
Email: rafael.ramosn@upb.edu.co

Universidad: Pontificia Bolivariana seccional Montería
