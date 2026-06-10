# 🛡️ 05 — Medidas de Mitigación

## Principio general

La solución más efectiva contra ataques BadUSB no es una única medida, sino una **estrategia multicapa** que combine controles técnicos y buenas prácticas organizativas. Ningún control por sí solo es suficiente.

---

## Medidas técnicas

### 1. Bloqueo automático de sesión

**¿Por qué?** El acceso físico a una sesión activa es el requisito indispensable de este tipo de ataque. Si la sesión está bloqueada, el script BadUSB no puede interactuar con el sistema de forma útil.

**Implementación**:
- GPO: `Computer Configuration > Windows Settings > Security Settings > Local Policies > Security Options > Interactive logon: Machine inactivity limit`
- Recomendado: bloqueo automático tras 1-3 minutos de inactividad
- Acceso directo: `Win + L` como hábito de usuario

---

### 2. Control y restricción de dispositivos USB

**¿Por qué?** Impedir la conexión de periféricos no autorizados elimina directamente el vector de entrada del ataque.

**Implementación**:

| Herramienta | Enfoque |
|-------------|---------|
| Group Policy (GPO) | `Computer Configuration > Administrative Templates > System > Device Installation > Device Installation Restrictions` |
| Microsoft Intune | Perfiles de restricción de dispositivos para endpoints gestionados |
| USB Guard (Linux) | Autorización por lista blanca de dispositivos |
| Soluciones EDR | CrowdStrike, SentinelOne, etc. incluyen control de dispositivos |

---

### 3. Restricción de herramientas sensibles

**¿Por qué?** Aunque en esta prueba solo se usó el Bloc de notas, un script malicioso real intentaría ejecutar PowerShell, CMD, o herramientas de administración del sistema.

**Implementación**:
- **AppLocker** o **Windows Defender Application Control (WDAC)** para restringir la ejecución de PowerShell y CMD a usuarios autorizados
- Política de ejecución de PowerShell: `Set-ExecutionPolicy RemoteSigned` o `AllSigned`
- Deshabilitar el acceso a `Win+R` en equipos de usuarios no técnicos mediante GPO

---

### 4. Soluciones EDR empresariales

**¿Por qué?** Microsoft Defender no detectó la actividad BadUSB. Un EDR avanzado monitoriza comportamientos, no solo firmas de malware, y puede detectar patrones anómalos incluso sin código malicioso.

**Opciones**:
- CrowdStrike Falcon
- SentinelOne
- Microsoft Defender for Endpoint (versión empresarial con EDR)
- Elastic Security

---

## Medidas organizativas

### 5. Concienciación y formación de usuarios

Esta es posiblemente la medida más importante en entornos reales. Los controles técnicos pueden ser bypasseados; un usuario consciente del riesgo es la primera línea de defensa.

**Puntos clave a comunicar**:
- Bloquear siempre la sesión al alejarse del equipo (`Win + L`)
- No conectar dispositivos USB desconocidos, aunque parezcan simples pendrives o teclados
- Reportar dispositivos físicos sospechosos encontrados cerca de equipos (USB dropping)
- Desconfiar de "técnicos" no esperados que solicitan conectar algo al equipo

---

### 6. Política de control de acceso físico

- Control de acceso a zonas con equipos sensibles (tarjetas, tornos, CCTV)
- Registro de visitas y acompañamiento de personal externo
- Inventario y monitorización de periféricos conectados
- Protocolos claros para reportar incidentes físicos

---

## Resumen ejecutivo

| Medida | Coste | Dificultad | Impacto |
|--------|-------|------------|---------|
| Bloqueo automático de sesión | Bajo | Bajo | Alto |
| Restricción PowerShell/CMD | Bajo | Medio | Alto |
| Control de dispositivos USB (GPO) | Bajo | Medio | Alto |
| EDR empresarial | Alto | Medio-alto | Muy alto |
| Formación y concienciación | Medio | Medio | Fundamental |
| Control de acceso físico | Variable | Variable | Alto |

---

[← Resultados](./04-results.md) | [← Inicio](../README.md)
