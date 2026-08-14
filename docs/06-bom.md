# BOM — Sense-Termocarbox-V1

**Documento:** BOM-001 | **Revisión:** 01 | **Estado:** Baseline de layout PCB exportado

Lista de materiales alineada al layout PCB en EasyEDA Pro y al export `hardware/Sense-Termocarbox-V1-BOM-v1.csv`.

| Designador | Cant. | Descripción | Valor / especificación | Fabricante / marca | Número de parte | Proveedor | Parte proveedor | Notas |
|---|---:|---|---|---|---|---|---|---|
| U1 | 1 | Microcontrolador STM32G4, LQFP48 | STM32G431CBT6 | STMicroelectronics | STM32G431CBT6 | TBD | TBD | MCU principal; HSI16 interno, sin cristal externo |
| C_VDD1, C_VDD2, C_VDD3 | 3 | Capacitor cerámico SMD 0603 de desacoplo VDD | 100nF | TBD | TBD | TBD | TBD | Uno por cada pin VDD; colocar lo más cerca posible del STM32G431CBT6 |
| C_BULK_3V3 | 1 | Capacitor electrolítico radial THT | 10µF, 25V | Chengx | KF106M025C05RR0VH2FP0 | LCSC | C43846 | Bulk local de 3.3V para MCU |
| FB1 | 1 | Ferrita THT para aislamiento VDDA | 100Ω @ 100MHz | FH | RH3.5x0.8x9P52E | LCSC | C192455 | Entre 3.3V y VDDA |
| C_VDDA_100N | 1 | Capacitor cerámico SMD 0603 para VDDA | 100nF | TBD | TBD | TBD | TBD | Del lado VDDA de FB1; colocar cerca del pin VDDA |
| C_VDDA_1U | 1 | Capacitor cerámico SMD 0603 para VDDA | 1µF | TBD | TBD | TBD | TBD | Del lado VDDA de FB1; colocar cerca del pin VDDA |
| J_PWR_IN | 1 | Bornera THT 2 pines, paso 5.08mm | 1x2P, P=5.08mm | DLL | 5.08-2A | LCSC | C22394555 | Entrada principal +12V/GND |
| D_PROT_12V | 1 | Diodo Schottky THT serie para polaridad inversa | 1N5822, 40V, 3A | ST | 1N5822 | LCSC | C915962 | En serie entre +12V_IN y +12V |
| U_BUCK_5V | 1 | Módulo buck mini DC-DC ajustable | 12V a 5V, 3A max, salida fijada a 5V | TBD | TBD | AliExpress / TBD | TBD | Pinout: EN, IN+, GND, V+; EN a +12V |
| U_LDO_3V3 | 1 | Regulador LDO fijo THT | LM1117T-3.3, 3.3V, 1A | HGSEMI | LM1117T-3.3 | LCSC | C498321 | 5V a 3.3V |
| C_LDO_IN, C_LDO_OUT | 2 | Capacitor electrolítico radial THT | 10µF, 25V | Chengx | KF106M025C05RR0VH2FP0 | LCSC | C43846 | Entrada/salida del LM1117T-3.3 |
| Q_HOTEND, Q_VALVE | 2 | MOSFET N logic-level THT | IRLZ44N, TO-220AB | Infineon | IRLZ44NPBF | LCSC | C38774 | Drivers low-side para hotend y electroválvula |
| R_HOTEND_GATE, R_VALVE_GATE | 2 | Resistencia axial THT metal film | 150Ω, 1%, 250mW | VO | MF1/4W-150Ω±1%-ST52 | LCSC | C2903242 | Serie en gate de IRLZ44N |
| R_HOTEND_PD, R_VALVE_PD, R_BUZZER_PD | 3 | Resistencia axial THT metal film | 10kΩ, 1%, 250mW | VO | MF1/4W-10K±1%-ST52 | LCSC | C2903232 | Pull-down de gate |
| J_HOTEND | 1 | Bornera THT 2 pines, paso 5.08mm | HOTEND_12V_40W | DLL | 5.08-2A | LCSC | C22394555 | Salida a resistencia hotend |
| J_VALVE | 1 | Bornera THT 2 pines, paso 5.08mm | 2V025-08 12VDC | DLL | 5.08-2A | LCSC | C22394555 | Salida a electroválvula |
| D_VALVE_FLYBACK | 1 | Diodo rectificador THT para flyback | 1N4001, 50V, 1A | baocheng | 1N4001 | LCSC | C47018536 | En paralelo con bobina de electroválvula |
| Q_BUZZER | 1 | MOSFET N THT | BS170, TO-92 | onsemi | BS170 | LCSC | C111691 | Driver low-side del buzzer pasivo |
| R_BUZZER_GATE | 1 | Resistencia axial THT metal film | 220Ω, 250mW | TBD | TBD | TBD | TBD | Serie en gate de BS170 |
| BUZ1 | 1 | Buzzer piezoeléctrico pasivo THT, 12.2mm | 3V, 4kHz | TDK | PS1240P02BT | LCSC | C76871 | Alimentado desde +5V; validar en banco |
| J_BUZZER | 1 | Header macho THT 1x2, paso 2.54mm | Buzzer 5V | TBD | HDR-M_2.54_1x2P | TBD | TBD | Conector interno para BUZ1 |
| J_DWIN | 1 | Conector PH2.0 THT 8 pines | DWIN DMG80480C070-04WTC UART TTL/CMOS | BOOMELE | BX-PH2.0-8PZZ | LCSC | C18077750 | 1-2 GND, 3 RX4/NC, 4 RX2, 5 TX2, 6 TX4/NC, 7-8 +5V |
| J_TEST | 1 | Conector PH2.0 THT 8 pines | Header de pruebas | BOOMELE | BX-PH2.0-8PZZ | LCSC | C18077750 | 1 GND, 2 +12V, 3 +5V, 4 3V3, 5 PRESS, 6 NTC, 7 DS18, 8 PED |
| J_PEDAL | 1 | Bornera THT 2 pines, paso 5.08mm | COM/NO | DLL | 5.08-2A | LCSC | C22394555 | COM a GND, NO a PC13; se eliminó NC |
| J_SWD | 1 | Header macho THT 1x5, paso 2.54mm | SWD + NRST | TBD | 2.54-1*5 | LCSC | C5156614 | 3V3, SWDIO, SWCLK, NRST, GND |
| J_NTC_HOTEND | 1 | Header macho THT 1x2, paso 2.54mm | NTC hotend | TBD | HDR-M_2.54_1x2P | TBD | TBD | Compatible con Dupont 2P del hotend |
| R_NTC_PULLUP | 1 | Resistencia axial THT metal film | 100kΩ, 1%, 250mW | CCO | TBD | LCSC | C119369 | Pull-up divisor NTC a 3V3 |
| J_PRESSURE | 1 | Bornera THT 3 pines, paso 5.08mm | Sensor presión 60psi, salida 5V | TBD | KF128-5.08-3P-AA | LCSC | C474953 | +5V, GND, PRESSURE_SIG_RAW |
| R_PRESSURE_TOP | 1 | Resistencia axial THT metal film | 10kΩ, 1%, 250mW | VO | MF1/4W-10K±1%-ST52 | LCSC | C2903232 | Divisor sensor presión |
| R_PRESSURE_BOT | 1 | Resistencia axial THT metal film | 15kΩ, 1%, 250mW | VO | TBD | LCSC | C2903263 | Divisor sensor presión |
| R_PRESSURE_FILT | 1 | Resistencia axial THT metal film | 1kΩ, 250mW | TBD | TBD | TBD | TBD | Filtro RC hacia PA0 |
| C_PRESSURE_FILT | 1 | Capacitor cerámico THT | 100nF | TBD | TBD | TBD | TBD | Filtro RC en PA0 |
| D_PRESSURE_ZENER | 1 | Diodo zener THT | 1N4728ATR, 3.3V | TBD | 1N4728ATR | TBD | TBD | Protección ADC PA0; cátodo al pin ADC, ánodo a GND |
| J_DS18B20 | 1 | Bornera THT 3 pines, paso 5.08mm | DS18B20 roscado G1/2" | TBD | KF128-5.08-3P-AA | LCSC | C474953 | 3V3, GND, DQ |
| R_DS18B20_PULLUP | 1 | Resistencia axial THT metal film | 4.7kΩ, 1%, 250mW | CCO | TBD | LCSC | C119339 | Pull-up 1-Wire a 3V3 |
