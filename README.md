# Sistema de Control de LEDs con Teclado 4x4 (Raspberry Pi Pico W)

## Descripción general
Este repositorio documenta e implementa un sistema embebido en C/C++ para **Raspberry Pi Pico W (RP2040)**, orientado a simulación en **Wokwi**. El sistema usa un **teclado matricial 4x4** para activar o desactivar **12 LEDs** mediante mapeo directo de teclas a salidas GPIO.

> Alcance: se conserva exactamente la lógica del programa proporcionado, sin cambios funcionales.

## Estructura del repositorio

- `src/main.cpp`: código fuente principal (lógica original).
- `docs/wiring.md`: cableado, componentes y mapeo GPIO.
- `docs/architecture.md`: arquitectura, flujo y comportamiento del software.
- `README.md`: guía de uso rápida y contexto del proyecto.

## Componentes (basado en JSON de Wokwi)
Del JSON del diagrama se identifican explícitamente:

1. `wokwi-pi-pico` (Raspberry Pi Pico/Pico W en simulación)
2. `wokwi-membrane-keypad` (teclado matricial 4x4)

Además, el código controla 12 salidas para LEDs (`LEDS = 12`), por lo que para una implementación física se consideran **12 LEDs + resistencias limitadoras**.

## Mapeo GPIO (resumen)

### Teclado 4x4
- Filas: GP26, GP22, GP21, GP20
- Columnas: GP19, GP18, GP17, GP16

### LEDs
- Banco numérico (índices `0..7`): GP11, GP10, GP9, GP8, GP7, GP6, GP5, GP4
- Banco alfabético (índices `8..11`): GP3, GP2, GP28, GP27

Para el detalle completo consultar `docs/wiring.md`.

## Ejecución en Wokwi
1. Crear un proyecto nuevo de Raspberry Pi Pico en Wokwi.
2. Copiar `src/main.cpp` como archivo principal del sketch.
3. Configurar el diagrama con el JSON proporcionado (teclado 4x4 conectado a GP16–GP26 según tabla).
4. Añadir 12 LEDs virtuales y conectarlos a los GPIO definidos en `ledPins`.
5. Iniciar simulación y pulsar teclas del keypad.

## Ejecución en hardware real
1. Usar una Raspberry Pi Pico W.
2. Conectar teclado 4x4 exactamente como en `docs/wiring.md`.
3. Conectar 12 LEDs con resistencia serie (220–330 Ω recomendado) a los GPIO definidos.
4. Unir todas las tierras (GND común).
5. Compilar/cargar el firmware C/C++ en la placa.

## Nota importante sobre interpretación de diagramas
**CODEX NO tiene capacidades de OCR ni visión de imágenes.** Solo puede trabajar con **texto** (por ejemplo, código fuente y JSON de conexiones). **ChatGPT sí puede ayudar a describir diagramas de forma interpretativa** cuando se aporta una descripción textual o estructura de datos del diagrama.

## Sección final: resumen funcional
- **Qué hace el sistema:** lee un teclado 4x4 y controla 12 LEDs en dos bancos (numérico y alfabético).
- **Cómo funciona el flujo del programa:** inicializa GPIO de LEDs en `setup()`, escanea teclas en `loop()`, y ejecuta acciones `switch-case` para encender/apagar LEDs individuales o grupos.
- **Qué representa el diagrama:** la interconexión eléctrica entre Pico y el teclado matricial (filas/columnas), que habilita la detección de teclas para gobernar las salidas LED.
