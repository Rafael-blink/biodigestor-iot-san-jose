# Guía de Contribución

Gracias por su interés en contribuir al Sistema de Monitoreo IoT para Biodigestores de la Institución Educativa San José de Canalete. Este documento define las pautas para asegurar contribuciones consistentes, reproducibles y alineadas con los objetivos técnicos y académicos del proyecto.

---

## Código de Conducta

Este repositorio se rige por el archivo [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md). Se espera que todas las interacciones se mantengan dentro de un marco de respeto, profesionalismo y colaboración.

---

## Formas de Contribuir

### Reporte de Errores

Antes de crear un *issue*, verifique que no exista un reporte previo en la sección de [Issues](https://github.com/TU-USUARIO/biodigestor-iot-san-jose/issues). Al crear uno nuevo, incluya:

- Versión de MicroPython utilizada
- Descripción detallada del problema
- Pasos para reproducir el error
- Resultado esperado versus resultado observado
- Evidencia adjunta (logs o capturas de consola serial)

### Sugerencias de Mejora

Las propuestas de nuevas funcionalidades deben:

- Estar justificadas técnicamente
- Aportar valor al sistema en su contexto de uso
- Considerar las limitaciones de hardware de la Raspberry Pi Pico W

### Pull Requests

1. Realizar un *fork* del repositorio.
2. Crear una rama con nombre descriptivo según el tipo de cambio:

   ```
   feature/nombre-funcionalidad
   fix/descripcion-error
   docs/actualizacion
   ```

3. Implementar los cambios con código claro y debidamente documentado.
4. Actualizar la documentación afectada por el cambio.
5. Validar el funcionamiento antes de enviar.
6. Enviar el *pull request* con una descripción técnica del cambio y su justificación.

---

## Estándares de Código

### Python / MicroPython

| Aspecto | Convención |
|---------|------------|
| Estilo general | PEP 8 (adaptado al entorno embebido) |
| Variables y funciones | `snake_case` |
| Clases | `PascalCase` |
| Comentarios y docstrings | Español, con términos técnicos en inglés |
| Docstrings | Obligatorios en todas las funciones |
| Gestión de recursos | Optimización para las limitaciones de la Pico W |

### Documentación

- El README principal debe mantenerse en español e inglés.
- La documentación técnica se redacta en español.
- Todo cambio funcional debe reflejarse en la documentación correspondiente.

---

## Pruebas

Se recomienda validar los cambios en hardware real (Raspberry Pi Pico W). En caso de utilizar simulación o entorno alternativo, incluya:

- Descripción del entorno de prueba utilizado
- Evidencia del comportamiento observado (logs, capturas de consola serial)

---

## Nota Académica

Este proyecto prioriza el rigor técnico, la reproducibilidad, la claridad pedagógica y la aplicabilidad en contextos rurales con recursos limitados. Las contribuciones deben mantenerse alineadas con estos principios.

---

## Contacto

Para consultas sobre el proceso de contribución, abra un *issue* con la etiqueta `question` o comuníquese a [rafael.ramosn@upb.edu.co](mailto:rafael.ramosn@upb.edu.co).
