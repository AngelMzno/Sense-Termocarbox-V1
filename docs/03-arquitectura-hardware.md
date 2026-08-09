# Arquitectura de hardware — Bloque 1 (en definición)

**Documento:** ARQ-001 | **Revisión:** 00 | **Estado:** Borrador — sub-bloques 1.1 y 1.2 cerrados, 1.3-1.6 pendientes

## Microcontrolador
- STM32G431CBT6 (diseño final)
- NUCLEO-G431RB (prototipado/pruebas)

## Cadena de gas (diagrama de flujo físico)
Tanque (CGA940, migración futura a CGA320) → Regulador etapa 1 (en tanque, ajustado a ~20 psi, redundancia de seguridad) → Regulador etapa 2 fino (candidatas: SMC ARM5SA-08-A [1.5-100 psi, con manómetro] o clon RVUM6-6/LRMA-QS-6 [a comparar en banco]) → Hotend (bloque MK8/MK9, calentador cerámico 12V/40W + termistor NTC 100K B3950 integrado) → Válvula solenoide 2/2 vías 12V DC (candidata: 2V025-08 o 35A-ACA-DDBA-1BA con 3er puerto para sensor de presión) → Sensor de temperatura DS18B20 roscado (contacto directo con el gas) → Salida (línea 4mm, calibrada a 4 L/min)

## Sensores
| Requisito | Sensor | Notas |
|---|---|---|
| RF-02/RS-01 (temperatura) | NTC 100K B3950 (hotend) + DS18B20 roscado | Estrategia dual/redundante |
| RS-02 (fuga) / RS-03 (flujo real) / RS-04 (válvula atascada) | Transductor de presión cerámico, rosca 1/8 NPT, salida DC 5V, rango recomendado ~60 psi | Cruzar lectura contra estado comandado de la válvula; un solo sensor cubre los 3 requisitos |

## Actuadores
- **Resistencia de calentamiento:** 12V, 40W (integrada en hotend), driver MOSFET nivel lógico + protección térmica de hardware (fusible/corte térmico) independiente del software, en serie
- **Electroválvula:** 12V DC, NC, 2/2 vías. Decisión: usar modelo simple 2V025-08 (1/4" BSP) para el diseño actual. Migración futura planeada a modelo de alta frecuencia 35A-ACA-DDBA-1BA (accionamiento piloto, 3 puertos, permite micropulsos de gas para control más fino de dosis en versiones futuras del firmware). Para el presupuesto de energía se considera el consumo de la versión de micropulsos (12.7W/12V ≈ 1.06A), no el de la versión simple, para dejar margen a la migración futura sin rediseñar la fuente de alimentación.

## Regulación de presión (dos etapas, por redundancia)
1. Regulador en tanque (CGA940), ajustado a ~20 psi
2. Regulador fino dentro de la máquina — candidatas en evaluación de banco:
   - SMC ARM5SA-08-A (rango 1.5-100 psi, con manómetro, componente trazable/certificado)
   - Clon RVUM6-6 (equivalente económico para prototipo, sin certificación — no usar en producción sin evaluar alternativa certificada)

## Decisión: no usar controlador de flujo másico (MFC)
Se evaluó agregar un controlador de flujo másico (MFC) para regular el caudal de gas de forma activa, en vez de depender de presión regulada + calibración fija de posición de válvula/aguja. Se decidió NO incluirlo en este rediseño por las siguientes razones:
- El enfoque actual (dos etapas de regulación de presión + calibración fija + sensor de presión que cruza contra el estado comandado de la válvula) ya cubre razonablemente el riesgo de dosis inconsistente por variación de presión.
- Un MFC es significativamente más caro y complejo de integrar (electrónica de control propia, calibración de fábrica, comunicación digital) comparado con los componentes ya seleccionados.
- Mientras el tanque tenga CO2 líquido, la presión interna se mantiene razonablemente estable, limitando el beneficio real de un MFC en este caso de uso.

Queda como mejora futura a evaluar si las pruebas de banco muestran variación de dosis mayor a la aceptable.

## Presión de trabajo del sistema
~20 psi (etapa 1), no supera 50 psi; objetivo de salida libre: 4 L/min

## Pendientes de Bloque 1
- 1.5: Diagrama de bloques formal (gráfico)
- 1.6: Mapeo de pines del STM32G431CBT6
- Validar en banco de pruebas: presión real necesaria para 4 L/min, y comparación ARM5 vs RVUM6-6
- Confirmar proveedor de CO2 grado médico con conexión CGA320 antes de migrar de CGA940

## Interfaces (Bloque 1.3 — cerrado)

**Pantalla:** DWIN DMG80480C070-04WTC, 7", 800x480, capacitiva, DGUS II/T5L0. Alimentación: 5V (4.5-5.5V), hasta 510mA con retroiluminación encendida, 170mA apagada. Fuente recomendada por fabricante: 5V 1A DC. Conector: 10 pines, paso 1.0mm.

**Pedal:** switch mecánico simple, conector de 3 pines disponible (COM/NC/NO), se usa solo el contacto NO (normalmente abierto) hacia un GPIO del MCU con antirebote por firmware.

**Buzzer:** pasivo (no activo), controlado por PWM desde un timer del STM32G431, para poder generar tonos distintos (no solo patrones de repetición) y así diferenciar los siguientes eventos: encendido, confirmación de comunicación pantalla-MCU, toque de botón en pantalla, inicio de sesión, inicio de aplicación/dosis, fin de sesión, y error/alarma.

## Alimentación (Bloque 1.4 — cerrado)

**Riel de 12V (fuente principal AC-DC, 110VAC de entrada):**
- Fuente conmutada 12VDC, certificación UL, sin restricción de horas de uso (requisito: mínimo 10 horas continuas garantizadas — descarta fuentes clasificadas para máximo 8 horas)
- Modelo de referencia: A-100FAN-12 (100W, 12V/8.5A)
- Consumo estimado en el riel (peor caso, considerando electroválvula de micropulsos a futuro): hotend 3.33A + electroválvula 1.06A + reguladores downstream ~0.6-0.7A ≈ 5.0-5.1A total, ~40% de margen sobre la capacidad de la fuente
- Pendiente para fase de producción: evaluar fuente con certificación IEC/UL 60601-1 (específica de equipo médico) en vez de UL general

**Riel de 5V:** derivado del riel de 12V vía regulador step-down (buck), sin modelo/marca específico decidido aún. Especificación eléctrica requerida: entrada 12V, salida 5V ±5%, mínimo 1A de capacidad. Alimenta: pantalla DWIN.

**Riel de 3.3V:** derivado del riel de 12V o 5V, sin modelo/marca específico decidido aún. Especificación eléctrica requerida: salida 3.3V ±3%. Alimenta: STM32G431CBT6 y sensores digitales. Corriente exacta pendiente de precisar en Bloque 1.6 (mapeo de pines).

**Contacto con el paciente:** únicamente vía gas (aguja); no hay contacto eléctrico directo con el paciente. Nota a futuro (no implementada): posible función de RF en versiones futuras del equipo.

## Mapeo de pines (Bloque 1.6 — cerrado)

**Microcontrolador:** STM32G431CBT6, encapsulado LQFP48. Validado en STM32CubeMX (proyecto MCU directo, no template de Board, para evitar heredar asignaciones específicas de la placa NUCLEO-G431RB usada en prototipado).

| Función | Pin | Configuración |
|---|---|---|
| UART pantalla HMI | PA9 (TX) / PA10 (RX) | USART1, modo Asynchronous |
| PWM resistencia de calentamiento (hotend) | PA8 | TIM1_CH1 |
| PWM buzzer pasivo | PA6 | TIM3_CH1 |
| ADC sensor de presión | PA0 | ADC1_IN1, Single-ended |
| ADC termistor NTC (hotend) | PA1 | ADC1_IN2, Single-ended |
| 1-Wire sensor DS18B20 | PB0 | GPIO Output Open Drain, sin pull-up/pull-down |
| Salida electroválvula (a driver MOSFET) | PB1 | GPIO Output Push-Pull |
| Entrada pedal | PC13 | GPIO Input, con Pull-up interno (switch NO a tierra) |
| Debug SWD | PA13 (SWDIO) / PA14 (SWCLK) | Reservado en modo "Serial Wire" |

**Notas:**
- El encapsulado LQFP48 no expone los puertos PC0-PC12, PD ni PE (disponibles solo en encapsulados más grandes) — el mapeo se concentra en PA/PB/PC13-15, que sí están disponibles.
- Recursos usados: 1 de 4-6 UARTs disponibles, 2 de varios canales ADC, 2 de 14 timers — amplio margen para futuras adiciones (micropulsos de electroválvula, RF, voz).

**Header de programación SWD (PCB final):** se incluirá un header de 4-5 pines (2.54mm, no el conector estándar ARM de 10 pines) con las señales SWDIO (PA13), SWCLK (PA14), GND, 3.3V, y NRST (opcional). Compatible con ST-LINK V2 externo. Es necesario tanto para desarrollo/depuración como para el primer flasheo de cada placa en producción (los chips salen de fábrica sin firmware).
