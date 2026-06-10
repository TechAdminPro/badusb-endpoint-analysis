# 📊 04 — Resultados y Análisis

## Tabla de resultados

| Elemento evaluado | Resultado observado |
|-------------------|---------------------|
| Reconocimiento del Flipper Zero | ✅ Windows lo reconoció como dispositivo HID válido |
| Ejecución del script BadUSB | ✅ El script se ejecutó correctamente |
| Apertura del Bloc de notas | ✅ Se realizó de forma automática |
| Escritura del mensaje | ✅ Se completó automáticamente |
| Solicitud de confirmación al usuario | ❌ No se solicitó ninguna |
| Detección por Microsoft Defender | ❌ No se observó alerta clara |
| Impacto potencial con script malicioso | ⚠️ Crítico |

---

## Análisis

### ¿Por qué funciona el ataque?

El sistema Windows confía de forma automática en los dispositivos HID. Esta confianza es necesaria para el funcionamiento normal del sistema (sin ella, ni los teclados ni los ratones físicos funcionarían), pero también representa una superficie de ataque cuando el dispositivo conectado no es lo que parece.

El Flipper Zero, al presentarse como un teclado USB, recibe el mismo nivel de confianza que cualquier periférico legítimo. No hay ningún mecanismo en la configuración estándar de Windows que verifique la autenticidad o el origen de un dispositivo HID.

### ¿Por qué no lo detecta Microsoft Defender?

Microsoft Defender está optimizado para detectar:

- Archivos maliciosos (malware, ransomware)
- Descargas sospechosas
- Comportamientos anómalos de procesos

Un BadUSB no hace ninguna de esas cosas. Simplemente introduce pulsaciones de teclado que el sistema interpreta como acciones legítimas del usuario. No hay ningún archivo malicioso, no hay ningún proceso sospechoso — por eso la detección es prácticamente nula con herramientas estándar.

### Condiciones necesarias para el ataque

El ataque requiere la confluencia de tres factores:

```
    Acceso físico          Sesión              Sin controles
    al equipo       +   desbloqueada    +     USB activos
         │                   │                    │
         └───────────────────┴────────────────────┘
                             │
                             ▼
                    ATAQUE VIABLE EN SEGUNDOS
```

Eliminar cualquiera de estos tres factores reduce significativamente el riesgo.

---

## Conclusión del análisis

> Los ataques físicos mediante dispositivos HID representan una **amenaza realista y subestimada** en sistemas endpoint. El hecho de que la prueba no generara alertas evidencia que las defensas estándar no están diseñadas para este vector de ataque.

La seguridad no puede depender exclusivamente de soluciones antivirus. Requiere una estrategia multicapa que incluya controles técnicos, políticas de uso y formación de usuarios.

---

[← Ejecución](./03-execution.md) | [Siguiente: Mitigaciones →](./05-mitigations.md)
