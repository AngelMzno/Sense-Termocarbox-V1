# Requisitos del sistema — Sense Termocarboxiterapia (S Carbox)

**Documento:** REQ-001 | **Revisión:** 00 | **Estado:** Borrador — Fase de definición (Bloque 0)

## Uso previsto
Dispositivo médico para administración subcutánea controlada de CO2 grado médico con fines de termocarboxiterapia estética/terapéutica, aplicado por personal capacitado mediante aguja e infiltración de dosis controlada por tiempo/volumen.

## Clasificación de riesgo (preliminar)
Clase II — dispositivo introducido al organismo (vía aguja) de forma transitoria (permanencia menor a 30 días por aplicación). Sujeto a confirmación formal por el área regulatoria de JADASA antes de someter a registro.

## Normas y disposiciones aplicables (preliminar)
- NOM-241-SSA1-2021/2025 — Buenas Prácticas de Fabricación de Dispositivos Médicos
- NOM-137-SSA1-2008 — Etiquetado de dispositivos médicos
- NOM-240-SSA1-2012 — Instalación y operación de la tecnovigilancia
- Normas eléctricas/seguridad aplicables a equipo médico (por determinar en Bloque 1)

## Descripción funcional del equipo
Sistema de dosificación de CO2 médico grado terapéutico para termocarboxiterapia. Componentes principales: tanque CO2 (CGA940, 3kg/4.5L) con regulador de presión mecánico (calibración manual única), cámara de calentamiento con resistencia, electroválvula de dosificación, pantalla táctil DWIN 7", salida hacia línea de muestreo con filtro de jeringa 0.22 µm.

## Requisitos funcionales
- RF-01: Suministro dosificado de CO2, calibrado a 4 L/min de salida.
- RF-02: Calentamiento del gas hasta un máximo de 31°C ±1°C, medido lo más cerca posible del punto de entrega al paciente. Criterio de aceptación: verificación por medición directa comparada contra referencia trazable. Mecanismo de sensor (hotend externo vs. roscado en contacto con el gas) a definir en Bloque 1.
- RF-03: Purga de aire en las líneas al iniciar cada sesión, antes de la etapa de calentamiento. Deseable: aprovechar la purga para detectar disponibilidad de gas en el tanque.
- RF-04: Activación de dosis por pedal (tipo disparo: un pisón aplica la dosis total programada) o botón en pantalla.
- RF-05: Botón de reinicio de sesión (detiene dosificación y regresa a cero los contadores).
- RF-06: Interfaz de usuario en pantalla DWIN 7" táctil capacitiva.
- RF-07: Selección de protocolos preestablecidos (21 protocolos en 4 categorías: Lipolítico, Flacidez, Regeneración celular, Facial) y modo manual (dosis total, dosis parcial, cálculo automático de aplicaciones).

## Requisitos de seguridad (derivados del análisis de riesgos — ver docs/02-analisis-riesgos.md)
- RS-01: Corte automático por sobre-temperatura del gas (>31°C +1°C de margen, valor final a confirmar en verificación).
- RS-02: Detección de fuga de gas.
- RS-03: Confirmación de flujo/gas real durante la dosificación (no solo activación de electroválvula).
- RS-04: Detección de electroválvula atascada en posición abierta.

## Fuera de alcance (Bloque 0)
Selección de componentes, arquitectura de hardware, diseño de PCB y arquitectura de firmware — se definen en bloques posteriores. MCU seleccionado para desarrollo: STM32G431CBT6 (target), NUCLEO-G431RB (prototipado).
