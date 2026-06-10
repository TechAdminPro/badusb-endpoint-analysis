# 🔬 02 — Metodología y Entorno

## Entorno de trabajo

El proyecto se desarrolla sobre un equipo personal con Windows 11 actuando como sistema endpoint objetivo, simulando un puesto de trabajo básico en un entorno corporativo.

> Se valoró usar una máquina virtual para mayor aislamiento, pero se optó por el sistema principal por comodidad y disponibilidad, aplicando únicamente pruebas no destructivas.

---

## Componentes del entorno

| Elemento | Descripción |
|----------|-------------|
| Equipo endpoint | PC personal con Windows 11 Home (v25H2) |
| Sistema de protección | Microsoft Defender / Seguridad de Windows |
| Dispositivo de ataque simulado | Flipper Zero |
| Técnica utilizada | BadUSB / dispositivo HID |
| Tipo de prueba | No destructiva y en entorno controlado |
| Finalidad | Evaluar riesgo, respuesta del sistema y mitigación |

---

## Herramientas

### Flipper Zero

Dispositivo portátil multifunción orientado a investigación de seguridad y auditorías. En este proyecto se emplea su funcionalidad **BadUSB**, que permite al dispositivo ser reconocido por el sistema operativo como un teclado USB, automatizando secuencias de pulsaciones que el sistema interpreta como entrada legítima de usuario.

### BadUSB

Técnica que manipula el comportamiento de un dispositivo USB para que se presente ante el sistema operativo como un periférico de entrada (teclado/ratón) en lugar de una unidad de almacenamiento. La clave del riesgo está en que **los sistemas operativos confían en dispositivos HID sin verificación adicional**, permitiendo la ejecución de comandos automatizados sin validación del usuario.

### Microsoft Defender

Solución de seguridad integrada en Windows, empleada como mecanismo de defensa principal para observar si el sistema es capaz de bloquear o detectar la actividad generada por el BadUSB. El objetivo no es comparar Defender con EDRs empresariales, sino analizar si la **configuración estándar de Windows** es suficiente para detectar este tipo de amenaza.

---

## Ideas clave de partida

1. Los dispositivos HID son aceptados por defecto por todos los sistemas operativos
2. Un BadUSB puede simular un teclado y ejecutar acciones de forma automatizada
3. El ataque requiere acceso físico al equipo objetivo
4. Las soluciones antivirus tradicionales no siempre detectan estos comportamientos (no son ejecución de malware)
5. La defensa efectiva combina medidas técnicas y organizativas

---

## Riesgo analizado

> Posibilidad de que un atacante con acceso físico temporal conecte un HID malicioso y ejecute acciones automatizadas sin autorización.

Contextos de riesgo habituales:

- Oficinas con puestos desatendidos
- Aulas y bibliotecas
- Espacios de coworking
- Equipos compartidos
- Eventos y conferencias (salas de espera, stands, etc.)

---

## Escenario simulado

```
[Empleado deja PC desbloqueado]
         │
         ▼
[Atacante (técnico de red ficticio) conecta Flipper Zero]
         │
         ▼
[Windows reconoce el dispositivo como teclado USB]
         │
         ▼
[Se ejecuta el script automáticamente]
         │
         ▼
[Sistema analizado: ¿detecta? ¿bloquea? ¿permite?]
```

---

## Limitaciones del entorno

| Limitación | Impacto |
|------------|---------|
| Sin infraestructura corporativa real | El escenario es simulado, no un entorno empresarial real |
| Sin EDR avanzado | No se puede comparar con soluciones como CrowdStrike o SentinelOne |
| Un solo sistema operativo | Sin comparativa multi-OS |
| Equipo personal | No representativo de una flota empresarial gestionada |
| Pruebas no destructivas | No refleja el impacto real de un ataque malicioso |

Estas limitaciones no invalidan el proyecto — el objetivo es demostrar el riesgo y analizar el comportamiento del sistema en condiciones controladas. Se consideran **líneas de ampliación futura**.

---

[← Propuesta](./01-proposal.md) | [Siguiente: Ejecución →](./03-execution.md)
