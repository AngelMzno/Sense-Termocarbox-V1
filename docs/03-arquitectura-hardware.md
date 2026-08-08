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
- **Electroválvula:** 12V DC, NC, 2/2 vías (evolución futura a 3/2 vías); candidatas: 2V025-08 (1/4" BSP) o 35A-ACA-DDBA-1BA (alta frecuencia, permite micropulsos a futuro)

## Regulación de presión (dos etapas, por redundancia)
1. Regulador en tanque (CGA940), ajustado a ~20 psi
2. Regulador fino dentro de la máquina — candidatas en evaluación de banco:
   - SMC ARM5SA-08-A (rango 1.5-100 psi, con manómetro, componente trazable/certificado)
   - Clon RVUM6-6 (equivalente económico para prototipo, sin certificación — no usar en producción sin evaluar alternativa certificada)

## Presión de trabajo del sistema
~20 psi (etapa 1), no supera 50 psi; objetivo de salida libre: 4 L/min

## Pendientes de Bloque 1
- 1.3: Interfaces (pantalla DWIN, pedal, buzzer)
- 1.4: Alimentación (fuente de poder, aislamiento)
- 1.5: Diagrama de bloques formal (gráfico)
- 1.6: Mapeo de pines del STM32G431CBT6
- Validar en banco de pruebas: presión real necesaria para 4 L/min, y comparación ARM5 vs RVUM6-6
- Confirmar proveedor de CO2 grado médico con conexión CGA320 antes de migrar de CGA940
