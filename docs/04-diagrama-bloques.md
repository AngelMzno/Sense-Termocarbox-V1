# Diagrama de bloques — Bloque 1.5

**Documento:** DIA-001 | **Revisión:** 00

## Diagrama 1: Cadena de gas (flujo físico)

```mermaid
flowchart LR
    A[Tanque CO2<br/>CGA940] --> B[Regulador etapa 1<br/>~20 psi]
    B --> C[Regulador etapa 2<br/>ARM5SA-08-A<br/>1.5-100 psi]
    C --> D[Hotend<br/>calentador 12V/40W<br/>+ termistor NTC]
    D --> E[Válvula 2/2<br/>12V DC<br/>2V025-08]
    E --> F[Sensor DS18B20<br/>contacto directo]
    F --> G[Salida 4mm<br/>4 L/min<br/>hacia paciente]

    H[Sensor de presión<br/>cerámico, ~60psi] -.monitorea.-> E
```

## Diagrama 2: Arquitectura eléctrica/control

```mermaid
flowchart TB
    MCU[STM32G431CBT6 LQFP48]

    MCU -->|PA9/PA10 USART1| DISP[Pantalla HMI 7 pulg DGUS II]
    MCU -->|PA8 TIM1_CH1 PWM| DRV1[Driver MOSFET]
    DRV1 --> HOT[Resistencia hotend 12V/40W]
    MCU -->|PA6 TIM3_CH1 PWM| BUZ[Buzzer pasivo]
    MCU -->|PA0 ADC1_IN1| PSEN[Sensor de presión]
    MCU -->|PA1 ADC1_IN2| NTC[Termistor NTC]
    MCU -->|PB0 1-Wire| DS[Sensor DS18B20]
    MCU -->|PB1 GPIO| DRV2[Driver MOSFET]
    DRV2 --> VALV[Electrovalvula 12V DC]
    MCU -->|PC13 GPIO pull-up| PED[Pedal NO]
    MCU -->|PA13/PA14 SWD| DBG[Header programacion]

    PWR12[Fuente 12V A-100FAN-12] --> REG5[Regulador 5V]
    PWR12 --> REG33[Regulador 3.3V]
    PWR12 --> HOT
    PWR12 --> VALV
    REG5 --> DISP
    REG33 --> MCU
```

## Notas
Ambos diagramas reflejan el mapeo de pines validado en STM32CubeMX (ver docs/03-arquitectura-hardware.md, sección "Mapeo de pines"). El fusible térmico de hardware en serie con la resistencia y las etapas de regulación de presión redundantes se documentan en detalle en docs/03-arquitectura-hardware.md, aquí se muestran simplificados para claridad visual.
