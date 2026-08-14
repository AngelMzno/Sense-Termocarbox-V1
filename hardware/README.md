# Hardware - Sense-Termocarbox-V1

Esta carpeta contiene el diseno electrico y el layout PCB de Sense-Termocarbox-V1, la placa de control para la migracion del equipo de termocarboxiterapia S Carbox de PIC + Nextion a STM32G431 + DWIN.

## Herramienta

- EasyEDA Pro
- Proyecto principal: `Sense-Termocarbox-V1.eprj2`

## Estado actual

- Esquematico capturado en EasyEDA Pro.
- Layout PCB del Bloque 2 completado.
- Plano GND inferior generado.
- DRC revisado sin fatal errors ni errores electricos.
- Ratlines revisadas sin conexiones pendientes.
- Gerbers exportados para revision previa a fabricacion.

Los warnings conocidos son de modelos 3D no vinculados para algunos conectores THT; no bloquean fabricacion electrica.

## Archivos relevantes

- `Sense-Termocarbox-V1.eprj2`: proyecto EasyEDA Pro con esquematico y PCB.
- `Sense-Termocarbox-V1-esquematico-v1.pdf`: export del esquematico.
- `Sense-Termocarbox-V1-BOM-v1.csv`: BOM exportada desde EasyEDA y ajustada al layout actual.
- `Gerber_PCB1_2026-08-14.zip`: paquete Gerber/Drill exportado para revision.
- `Sense-Termocarbox-V1_backup/`: backups automaticos de EasyEDA.

## Componentes principales del diseno

- MCU: STM32G431CBT6, LQFP48.
- HMI: DWIN DMG80480C070-04WTC, UART TTL/CMOS por conector PH2.0 8 pines.
- Entrada: 12V DC con proteccion serie por 1N5822.
- 5V: modulo buck ajustable 12V a 5V, configurado antes de instalar.
- 3V3: LM1117T-3.3 desde 5V.
- Sensor de presion: transductor 60 psi, salida 5V, acondicionado a PA0 con divisor, filtro RC y zener.
- Temperatura hotend: NTC 100K B3950 hacia PA1.
- Temperatura de gas: DS18B20 roscado G1/2", 1-Wire hacia PB0.
- Hotend: salida 12V/40W con MOSFET IRLZ44N low-side.
- Electrovalvula: 2V025-08 12VDC con MOSFET IRLZ44N low-side y diodo flyback 1N4001.
- Buzzer: PS1240P02BT con driver BS170.
- Programacion/debug: header SWD 1x5.
- Test: header J_TEST con GND, 12V, 5V, 3V3, PRESS, NTC, DS18 y PED.

## Notas antes de fabricar

- Verificar en visor Gerber: outline, drill, solder mask, serigrafia y planos.
- Confirmar pinout fisico del modulo buck antes de ensamblar.
- Confirmar orientacion de diodos, electroliticos, MOSFETs y LM1117T-3.3.
- Soldar/ajustar el buck a 5V antes de alimentar DWIN o la placa.
