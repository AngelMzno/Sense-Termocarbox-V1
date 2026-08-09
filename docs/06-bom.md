# BOM — Sense-Termocarbox-V1

**Documento:** BOM-001 | **Revisión:** 00 | **Estado:** En construcción

Lista de materiales conforme se definan componentes específicos durante la captura en EasyEDA Pro.

| Designador | Cant. | Descripción | Valor / especificación | Fabricante / marca | Número de parte | Proveedor | Parte proveedor | Notas |
|---|---:|---|---|---|---|---|---|---|
| U1 | 1 | Microcontrolador STM32G4, LQFP48 | STM32G431CBT6 | STMicroelectronics | STM32G431CBT6 | TBD | TBD | MCU principal; HSI16 interno, sin cristal externo |
| C_VDD1, C_VDD2, C_VDD3 | 3 | Capacitor cerámico THT de desacoplo VDD | 100nF, 50V | Dersonic | CC1H104MC1FD3F6C10MF | LCSC | C254085 | Uno por cada pin VDD |
| C_BULK_3V3 | 1 | Capacitor electrolítico radial THT | 10µF, 25V | Chengx | KF106M025C05RR0VH2FP0 | LCSC | C43846 | Bulk local de 3.3V para MCU |
| FB1 | 1 | Ferrita THT para aislamiento VDDA | 100Ω @ 100MHz | FH | RH3.5x0.8x9P52E | LCSC | C192455 | Entre 3.3V y VDDA |
| C_VDDA_100N | 1 | Capacitor cerámico THT para VDDA | 100nF, 50V | Dersonic | CC1H104MC1FD3F6C10MF | LCSC | C254085 | Del lado VDDA de FB1 |
| C_VDDA_1U | 1 | Capacitor cerámico THT para VDDA | 1µF, 50V | TDK | FK26X7R1H105KRE06 | LCSC | C2839237 | Del lado VDDA de FB1 |
| J_PWR_IN | 1 | Bornera THT 2 pines, paso 5.08mm | 1x2P, P=5.08mm | DLL | 5.08-2A | LCSC | C22394555 | Entrada principal +12V/GND |
| D_PROT_12V | 1 | Diodo Schottky THT serie para polaridad inversa | 1N5822, 40V, 3A | ST | 1N5822 | LCSC | C915962 | En serie entre +12V_IN y +12V |
| U_BUCK_5V | 1 | Módulo buck mini DC-DC ajustable | 12V a 5V, 3A max, salida fijada a 5V | TBD | TBD | AliExpress / TBD | TBD | Pinout: EN, IN+, GND, V+; EN a +12V |
| U_LDO_3V3 | 1 | Regulador LDO fijo THT | LM1117T-3.3, 3.3V, 1A | HGSEMI | LM1117T-3.3 | LCSC | C498321 | 5V a 3.3V |
| C_LDO_IN, C_LDO_OUT | 2 | Capacitor electrolítico radial THT | 10µF, 25V | Chengx | KF106M025C05RR0VH2FP0 | LCSC | C43846 | Entrada/salida del LM1117T-3.3 |
| Q_HOTEND, Q_VALVE | 2 | MOSFET N logic-level THT | IRLZ44N, TO-220AB | Infineon | IRLZ44NPBF | LCSC | C38774 | Drivers low-side para hotend y electroválvula |
| R_HOTEND_GATE, R_VALVE_GATE | 2 | Resistencia axial THT metal film | 150Ω, 1%, 250mW | VO | MF1/4W-150Ω±1%-ST52 | LCSC | C2903242 | Serie en gate de IRLZ44N |
| R_HOTEND_PD, R_VALVE_PD, R_BUZZER_PD | 3 | Resistencia axial THT metal film | 10kΩ, 1%, 250mW | VO | MF1/4W-10K±1%-ST52 | LCSC | C2903232 | Pull-down de gate |
| J_HOTEND | 1 | Bornera THT 2 pines, paso 5.08mm | HOTEND_12V_40W | DLL | 5.08-2A | LCSC | C22394555 | Salida a resistencia hotend |
| J_VALVE | 1 | Bornera THT 2 pines, paso 5.08mm | VALVE_12V_DC | DLL | 5.08-2A | LCSC | C22394555 | Salida a electroválvula |
| D_VALVE_FLYBACK | 1 | Diodo rectificador THT para flyback | 1N4001, 50V, 1A | baocheng | 1N4001 | LCSC | C47018536 | En paralelo con bobina de electroválvula |
| Q_BUZZER | 1 | MOSFET N THT | BS170, TO-92 | onsemi | BS170 | LCSC | C111691 | Driver low-side del buzzer pasivo |
| R_BUZZER_GATE | 1 | Resistencia axial THT metal film | 220Ω, 250mW | TBD | TBD | TBD | TBD | Serie en gate de BS170 |
| BUZ1 | 1 | Buzzer piezoeléctrico pasivo THT, 12.2mm | 3V, 4kHz | TDK | PS1240P02BT | LCSC | C76871 | Alimentado desde +5V; validar en banco |
| J_BUZZER | 1 | Header macho THT 1x2, paso 2.54mm | Buzzer 5V | TBD | HDR-M_2.54_1x2P | TBD | TBD | Conector interno para BUZ1 |
