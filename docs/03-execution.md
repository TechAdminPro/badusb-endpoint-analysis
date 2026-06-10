# ⚙️ 03 — Ejecución del Ataque

## Diseño de la prueba

La prueba consiste en utilizar el Flipper Zero como dispositivo HID, simulando el comportamiento de un teclado USB. Al conectarse al equipo Windows, el dispositivo ejecuta una secuencia previamente definida de pulsaciones.

El script diseñado abre el Bloc de notas de Windows y escribe un mensaje informativo, permitiendo comprobar visualmente que el sistema acepta entradas del dispositivo **sin ejecutar código malicioso ni modificar configuraciones críticas**.

---

## Paso 1 — Creación del script BadUSB

El script se crea en formato `.txt` compatible con la sintaxis DuckyScript del Flipper Zero. Contiene una cadena de instrucciones que abren el menú de ejecución de Windows, inician el Bloc de notas y escriben automáticamente el mensaje.

```
DELAY 1000       # Espera 1 segundo tras conectar
GUI r            # Abre el cuadro de diálogo Ejecutar (Win+R)
DELAY 500
STRING notepad   # Escribe "notepad"
ENTER            # Confirma
DELAY 1000       # Espera a que abra
STRING SIMULACION DE ATAQUE BADUSB
ENTER
...
```

> 📄 Script completo disponible en: [`/scripts/tfm_demo_simulacion.txt`](../scripts/tfm_demo_simulacion.txt)

**Captura de referencia**: `assets/screenshots/06-script-created.png`

---

## Paso 2 — Carga del script en el Flipper Zero

1. Conectar el Flipper Zero al equipo mediante USB
2. Abrir la aplicación de escritorio de Flipper
3. Acceder al explorador de archivos del dispositivo
4. Localizar la carpeta `SD Card/badusb/`
5. Copiar el archivo del script
6. Desconectar el Flipper del equipo

Tras el proceso, el script queda disponible desde el menú BadUSB del Flipper, listo para ejecución.

**Captura de referencia**: `assets/screenshots/07-script-loaded-flipper.png`

---

## Paso 3 — Ejecución inicial (sin medidas adicionales)

**Condiciones**:
- Sesión de Windows iniciada y **desbloqueada** ← *condición crítica*
- Flipper Zero conectado al puerto USB del equipo objetivo
- Sin ninguna restricción adicional activa

**Procedimiento**:
1. Mantener la sesión de Windows iniciada y desbloqueada
2. Conectar el Flipper Zero al equipo
3. Acceder en el Flipper al menú `BadUSB`
4. Seleccionar el script y ejecutarlo

**Lo que ocurre**:

```
Flipper conectado
      │
      ▼
Windows detecta nuevo dispositivo HID (teclado)
      │
      ▼
Script se ejecuta automáticamente
      │
      ├─► GUI+R → Abre cuadro "Ejecutar"
      ├─► "notepad" + ENTER → Abre Bloc de notas
      └─► Escribe el mensaje completo
```

**Captura de referencia**: `assets/screenshots/10-notepad-result.png`

---

## Paso 4 — Comprobación de seguridad

Tras la ejecución, se revisa:

- Estado de **Seguridad de Windows**
- **Historial de protección** de Microsoft Defender

**Resultado**: No se observó ninguna alerta en Microsoft Defender. Al tratarse de pulsaciones de teclado simuladas (sin descarga de archivos ni ejecución de malware), el sistema no identificó la actividad como amenaza.

> Este es el punto clave del análisis: **el ataque pasa desapercibido para las defensas estándar** porque no se parece a ninguna amenaza tradicional.

**Captura de referencia**: `assets/screenshots/11-defender-no-alert.png`

---

[← Metodología](./02-methodology.md) | [Siguiente: Resultados →](./04-results.md)
