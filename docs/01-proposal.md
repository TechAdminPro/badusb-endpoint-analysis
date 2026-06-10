# 📋 01 — Propuesta del Proyecto

## Resumen

Análisis del riesgo que presentan los ataques físicos mediante dispositivos HID en entornos corporativos simulados. Se utiliza un **Flipper Zero** configurado en modo BadUSB para simular el comportamiento de un teclado USB y ejecutar acciones automatizadas sobre un equipo Windows 11.

El trabajo evalúa cómo responde un sistema endpoint ante la conexión de un dispositivo físico aparentemente legítimo, identificando las debilidades asociadas a la **confianza automática** que los sistemas operativos depositan en periféricos de entrada USB.

---

## Finalidad

Demostrar que la seguridad de los sistemas no depende únicamente de la protección frente a amenazas remotas (malware, phishing, exploits de red), sino también del **control del acceso físico** y de los dispositivos conectados al equipo.

Los ataques con HID maliciosos son especialmente relevantes porque:

- No requieren explotar ninguna vulnerabilidad del sistema operativo
- Aprovechan una funcionalidad legítima: la aceptación de dispositivos de entrada
- Son difícilmente detectables por soluciones antivirus tradicionales
- Pueden ejecutarse en segundos

---

## Alcance

| Elemento | Detalle |
|----------|---------|
| Sistema objetivo | Equipo personal Windows 11 (simulando puesto corporativo) |
| Dispositivo atacante | Flipper Zero en modo BadUSB |
| Defensa analizada | Microsoft Defender / Seguridad de Windows |
| Tipo de pruebas | No destructivas, orientativas y educacionales |
| Infraestructura | Entorno controlado personal (no corporativo real) |

---

## Justificación

La ciberseguridad suele asociarse principalmente a amenazas remotas, pero el acceso físico a un equipo sigue siendo uno de los vectores de ataque más críticos y, en muchas ocasiones, el menos valorado.

Este proyecto no se limita a documentar una técnica ofensiva, sino que:

1. Analiza el riesgo real
2. Estudia sus causas técnicas y organizativas
3. Propone medidas de protección aplicables en entornos reales

---

## Resultado esperado

- Evaluación práctica del comportamiento de Windows 11 frente a un ataque HID físico
- Documentación de las pruebas, resultados y diferencias pre/post ataque
- Serie de recomendaciones para reducir el riesgo: controles técnicos, restricciones de uso, buenas prácticas y medidas de concienciación

---

[← Inicio](../README.md) | [Siguiente: Metodología →](./02-methodology.md)
