# Diseño de PCB — Bloque 2

**Documento:** PCB-001 | **Revisión:** 00 | **Estado:** Especificación previa a captura — herramienta: EasyEDA Pro

## Política de componentes
MCU en SMD (obligatorio, no existe versión THT del STM32G431CBT6, encapsulado LQFP48). Todo lo demás (resistencias, capacitores, transistores/MOSFETs, diodos, reguladores) en THT por preferencia de facilidad de soldado y reparación en campo. Excepción justificada: conversión 12V→5V usa módulo buck prearmado (IC interno SMD, pero módulo completo con headers, reemplazable como componente THT) para evitar pérdidas de potencia significativas de un regulador lineal en ese salto de voltaje.

## Fuente de reloj
Oscilador interno HSI16 del STM32G431 (sin cristal externo) — suficiente precisión para USART, PWM y ADC de esta aplicación; reduce componentes y puntos de falla mecánica frente a un cristal externo.

## Alimentación
- **12V → 5V:** módulo buck prearmado (tipo MP1584EN o LM2596), mínimo 1A de salida
- **5V → 3.3V:** LM1117T-3.3 (LDO fijo, TO-220/THT). Se descartó LM317 por dropout insuficiente (2-3V típico) frente al margen disponible de solo 1.7V (5V-3.3V)
- **Protección de entrada 12V:** diodo Schottky 1N5822 (THT, 3A) en serie, contra polaridad inversa
- **Desacoplo del MCU:** 100nF cerámico en cada pin VDD + 1 capacitor bulk de 4.7-10µF. VDDA con 1µF + 100nF separados, con ferrita/inductor entre VDD y VDDA para aislar ruido digital de la parte analógica (crítico para lecturas limpias de ADC)

**Decisiones de captura cerradas:**
- VREF+ conectado a VDDA; no se usará referencia externa de ADC.
- VBAT conectado a 3.3V; no se usará batería de respaldo.
- LM1117T-3.3 con capacitor de entrada de 10µF entre +5V y GND, y capacitor de salida de 10µF entre 3.3V y GND, colocados cerca del regulador.
- Módulo buck mini DC-DC ajustable configurado/soldado a 5V antes de instalar. Pinout capturado: EN, IN+, GND, V+. EN amarrado a +12V para operación siempre habilitada; IN+ a +12V; GND a GND; V+ a +5V.

## Drivers de potencia
**MOSFET único para hotend y electroválvula:** IRLZ44N (TO-220, THT, nivel lógico) — mismo número de parte para ambos drivers, simplifica inventario de refacciones en campo.
- Resistencia de gate: 100-220Ω (limita corriente de carga inicial desde el GPIO)
- Resistencia de pull-down en gate: ~10kΩ (evita activación accidental del MOSFET durante arranque/reset del MCU, antes de que el firmware configure el GPIO)
- Electroválvula (carga inductiva): diodo flyback (1N4001 o equivalente) en paralelo con la bobina, para proteger el MOSFET del pico de voltaje inverso al apagar

**Driver del buzzer pasivo:** BS170 (MOSFET canal N, TO-92/THT) — mismo criterio de bajo consumo estático que el resto de drivers MOSFET, controlado por PWM desde PA6 (TIM3_CH1)
- Alimentación del buzzer desde +5V.
- Resistencia de gate: 220Ω.
- Resistencia pull-down en gate: 10kΩ.
- Componente de referencia capturado: PS1240P02BT, buzzer piezoeléctrico pasivo THT.

## Acondicionamiento de señales analógicas

**Sensor de presión (salida 5V → ADC 3.3V, PA0):**
- Divisor resistivo: R1=10kΩ, R2=15kΩ (relación 0.6 → escala 5V a 3.0V, con margen bajo el límite de 3.3V del ADC)
- Filtro RC anti-ruido: R=1kΩ, C=100nF (cutoff ~1.6kHz, filtra ruido de conmutación del PWM/electroválvula sin afectar velocidad de lectura)
- Protección: diodo zener 3.3-3.6V a tierra en el pin del ADC, como respaldo contra sobrevoltaje
- Justificación de nivel de protección elegido (Nivel 2: divisor + protección + filtro, sin aislamiento galvánico): todos los componentes comparten el mismo gabinete y tierra, por lo que un amplificador de aislamiento no está justificado en este diseño
