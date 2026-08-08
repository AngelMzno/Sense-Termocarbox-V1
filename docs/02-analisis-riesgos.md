# Análisis de riesgos preliminar — Sense Termocarboxiterapia (S Carbox)

**Documento:** RM-001 | **Revisión:** 00 | **Metodología:** Análisis de peligros basado en ISO 14971 (adaptado, pendiente de formalización completa)

| ID | Peligro | Causa posible | Efecto | Mitigación (requisito derivado) |
|----|---------|---------------|--------|----------------------------------|
| R-01 | Sobre-temperatura del gas administrado | Falla en control PWM/resistencia, sensor fuera de rango, lazo de control sin corte | Molestia/daño térmico al paciente, pérdida de propiedades terapéuticas del gas | RS-01: corte automático por sobre-temperatura |
| R-02 | Fuga de gas en el trayecto | Conexión floja, manguera dañada, fitting mal sellado | Interrupción del suministro durante sesión, tanque se vacía antes de lo esperado | RS-02: detección de fuga de gas |
| R-03 | Dosificación sin gas real presente | Tanque vacío, obstrucción en línea, electroválvula activa pero sin flujo | Sesión incompleta sin que el operador lo note ("dosis fantasma") | RS-03: confirmación de flujo/gas real |
| R-04 | Electroválvula atascada abierta | Falla mecánica o eléctrica del actuador | Suministro continuo no controlado de gas | RS-04: detección de electroválvula atascada abierta |

## Notas
- Este análisis es preliminar, elaborado durante la fase de definición de requisitos (Bloque 0).
- Debe formalizarse con metodología completa ISO 14971 (severidad, probabilidad, nivel de riesgo, verificación de mitigación) antes de avanzar a diseño de detalle (Bloque 2 en adelante).
- Este documento es la entrada de riesgos referenciada en docs/01-requisitos.md conforme a NOM-241-SSA1, Cláusula 8.3.2.3.
