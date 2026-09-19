# 🔌 BadUSB Endpoint Analysis

> Análisis de vulnerabilidades en sistemas endpoint Windows frente a ataques físicos mediante dispositivos HID (BadUSB).  
> TFM — CEU Master en Ciberseguridad · 2026 · Carlos Munaiz Cossio

[![TFM](https://img.shields.io/badge/Tipo-TFM%20%2F%20Investigaci%C3%B3n-blue?style=flat-square)](.)
[![Platform](https://img.shields.io/badge/Target-Windows%2011-0078D4?style=flat-square&logo=windows)](.)
[![Device](https://img.shields.io/badge/Hardware-Flipper%20Zero-orange?style=flat-square)](.)
[![Status](https://img.shields.io/badge/Estado-Completado-brightgreen?style=flat-square)](.)
[![License](https://img.shields.io/badge/License-MIT-lightgrey?style=flat-square)](LICENSE)

---

## 📋 Descripción

Este proyecto documenta el análisis práctico de ataques físicos **BadUSB** sobre un sistema Windows 11, utilizando un **Flipper Zero** configurado como dispositivo HID (Human Interface Device). El objetivo es demostrar que la seguridad de los sistemas no depende únicamente de la protección frente a amenazas remotas — el **acceso físico y el control de periféricos** son vectores igual de críticos.

El trabajo evalúa la respuesta del sistema operativo y de **Microsoft Defender** ante la conexión de un dispositivo que simula ser un teclado USB legítimo, y propone medidas de mitigación técnicas y organizativas aplicables a entornos corporativos reales.

> ⚠️ **Aviso legal**: Todas las pruebas se realizaron en un entorno controlado y personal, con fines estrictamente académicos. No se ejecutó código malicioso ni se comprometió ningún sistema ajeno.

---

## 🗂️ Estructura del repositorio

```
badusb-endpoint-analysis/
│
├── README.md                  # Este archivo
├── LICENSE
│
├── docs/
│   ├── 01-proposal.md         # Propuesta y alcance del proyecto
│   ├── 02-methodology.md      # Metodología y entorno de pruebas
│   ├── 03-execution.md        # Ejecución del ataque paso a paso
│   ├── 04-results.md          # Resultados y análisis de seguridad
│   └── 05-mitigations.md      # Medidas de mitigación propuestas
│
├── scripts/
│   └── tfm_demo_simulacion.txt  # Script BadUSB utilizado en la prueba
│
└── assets/
    └── screenshots/           # Capturas del entorno y ejecución
        ├── 01-windows-info.png
        ├── 02-defender-active.png
        ├── 03-flipper-zero.png
        ├── 04-badusb-menu.png
        ├── 05-flipper-app-install.png
        ├── 06-script-created.png
        ├── 07-script-loaded-flipper.png
        ├── 08-script-selected.png
        ├── 09-system-initial-state.png
        ├── 10-notepad-result.png
        └── 11-defender-no-alert.png
```

---

## 🎯 Objetivos

| # | Objetivo | Estado |
|---|----------|--------|
| 1 | Diseñar un entorno controlado de pruebas | ✅ Completado |
| 2 | Configurar Flipper Zero en modo BadUSB | ✅ Completado |
| 3 | Ejecutar prueba no destructiva con HID | ✅ Completado |
| 4 | Analizar respuesta de Windows y Microsoft Defender | ✅ Completado |
| 5 | Elaborar recomendaciones técnicas y organizativas | ✅ Completado |
| 6 | Probar contra EDR corporativo avanzado | 🔜 Línea futura |
| 7 | Comparar en distintos sistemas operativos | 🔜 Línea futura |
| 8 | Pruebas en máquina virtual aislada | 🔜 Línea futura |

---

## 🧪 Entorno de pruebas

| Elemento | Detalle |
|----------|---------|
| Sistema objetivo | Windows 11 Home (v25H2, build 26200.8246) |
| Protección activa | Microsoft Defender / Seguridad de Windows |
| Dispositivo de ataque | Flipper Zero (modo BadUSB) |
| Técnica | HID Injection — simulación de teclado USB |
| Tipo de prueba | No destructiva, entorno controlado |

---

## ⚙️ El ataque: ¿cómo funciona?

```
┌─────────────────┐        USB         ┌──────────────────────┐
│   Flipper Zero  │ ─────────────────► │   Windows 11 (víctima) │
│  (modo BadUSB)  │  Reconocido como   │                      │
│                 │  teclado legítimo  │  Acepta HID sin      │
│  Script .txt    │                   │  verificación        │
│  cargado en SD  │  Ejecuta secuencia │                      │
└─────────────────┘  de pulsaciones   └──────────────────────┘
                                              │
                                              ▼
                                    Abre Run → Notepad
                                    Escribe mensaje automático
                                    Sin alerta de Defender ⚠️
```

El ataque **no explota ninguna vulnerabilidad tradicional**. Aprovecha la confianza automática que Windows deposita en los dispositivos HID (teclados, ratones). Cualquier dispositivo que se presente como teclado puede inyectar pulsaciones de forma inmediata.

---

## 📜 Script de prueba

El script utilizado es intencionalmente no destructivo. Su único propósito es abrir el Bloc de notas y escribir un mensaje informativo, demostrando que la automatización de acciones es posible.

```bash
# Ver script completo en: scripts/tfm_demo_simulacion.txt
DELAY 1000
GUI r
DELAY 500
STRING notepad
ENTER
DELAY 1000
STRING SIMULACION DE ATAQUE BADUSB
ENTER
STRING Este equipo ha aceptado un dispositivo HID externo.
...
```

---

## 🛡️ Resultados clave

| Elemento evaluado | Resultado |
|-------------------|-----------|
| Reconocimiento del Flipper Zero | ✅ Reconocido como dispositivo HID válido |
| Ejecución del script | ✅ Completada sin intervención del usuario |
| Apertura del Bloc de notas | ✅ Automática |
| Solicitud de confirmación | ❌ No se solicitó |
| Alerta de Microsoft Defender | ❌ Sin alerta observada |
| Impacto potencial | ⚠️ Crítico si se usa script malicioso |

**Conclusión**: Microsoft Defender no generó ninguna alerta porque el ataque no involucra archivos maliciosos — solo pulsaciones de teclado aparentemente legítimas. Este es precisamente el punto débil que explota BadUSB.

---

## 🔒 Medidas de mitigación

| Medida | Objetivo | Impacto esperado |
|--------|----------|-----------------|
| Bloqueo automático de sesión | Evitar acceso con sesión abierta | Alto — elimina la superficie de ataque principal |
| Restricción de PowerShell/CMD | Limitar herramientas sensibles | Alto — reduce impacto de ataques avanzados |
| Control de dispositivos USB (GPO/MDM) | Permitir solo periféricos autorizados | Alto — bloquea la conexión no autorizada |
| EDR corporativo | Detección de comportamientos anómalos | Muy alto — supera las limitaciones de Defender |
| Concienciación del usuario | Reducir errores humanos | Fundamental en entornos reales |

---

## 📚 Documentación completa

La documentación detallada del proyecto está disponible en la carpeta [`/docs`](./docs/):

- [`01-proposal.md`](./docs/01-proposal.md) — Propuesta, finalidad y alcance
- [`02-methodology.md`](./docs/02-methodology.md) — Entorno, herramientas y escenario
- [`03-execution.md`](./docs/03-execution.md) — Ejecución paso a paso con capturas
- [`04-results.md`](./docs/04-results.md) — Resultados y análisis de seguridad
- [`05-mitigations.md`](./docs/05-mitigations.md) — Medidas defensivas detalladas

---

## 🔮 Líneas de ampliación futura

- [ ] Repetir el ataque frente a un **EDR empresarial** (CrowdStrike, SentinelOne, etc.)
- [ ] Comparar el comportamiento en **Linux y macOS**
- [ ] Ejecutar las pruebas en una **máquina virtual aislada** (VirtualBox/VMware)
- [ ] Implementar un sistema de **monitorización y alertas** con SIEM
- [ ] Aplicar y documentar **políticas de control USB corporativas** via Group Policy / Intune

---

## 👨‍💻 Autor

**Carlos Munaiz Cossio**  
Ciberseguridad · 2026  
[![LinkedIn](https://img.shields.io/badge/LinkedIn-carlos--munaiz-0A66C2?style=flat-square&logo=linkedin)](https://www.linkedin.com/in/carlos-munaiz/)
[![GitHub](https://img.shields.io/badge/GitHub-TechAdminPro-181717?style=flat-square&logo=github)](https://github.com/TechAdminPro)

---

## 📄 Licencia

Este proyecto se distribuye bajo licencia [MIT](LICENSE). Puedes usarlo con fines educativos y de investigación, siempre dentro del marco legal y con los permisos correspondientes.
