# Arquitectura y Flujo del Programa

## 1) Objetivo del firmware
Implementar control de 12 salidas digitales (LEDs) mediante eventos de teclado matricial 4x4 en Raspberry Pi Pico W, conservando la lógica original del código fuente.

## 2) Estructura lógica

### Inicialización (`setup`)
1. Recorre los 12 pines en `ledPins`.
2. Configura cada pin como salida (`OUTPUT`).
3. Inicializa cada LED en bajo (`LOW`, apagado).

### Ciclo principal (`loop`)
1. Lee tecla actual con `keypad.getKey()`.
2. Si no hay tecla (`NO_KEY`), no ejecuta cambios de salida.
3. Si hay tecla válida, ejecuta bloque `switch`:
   - `1..8`: enciende LED individual correspondiente (índices `0..7`).
   - `9`: enciende todos los LEDs del banco numérico (`0..7`).
   - `0`: apaga todos los LEDs del banco numérico (`0..7`).
   - `A..D`: enciende LED individual del banco alfabético (`8..11`).
   - `*`: enciende todos los LEDs del banco alfabético (`8..11`).
   - `#`: apaga todos los LEDs del banco alfabético (`8..11`).
4. Espera `10 ms` con `delay(10)` para estabilizar el ciclo de sondeo.

## 3) Tabla de comportamiento tecla → acción

| Tecla | Acción |
|---|---|
| `1` | Enciende LED índice 0 (GP11) |
| `2` | Enciende LED índice 1 (GP10) |
| `3` | Enciende LED índice 2 (GP9) |
| `4` | Enciende LED índice 3 (GP8) |
| `5` | Enciende LED índice 4 (GP7) |
| `6` | Enciende LED índice 5 (GP6) |
| `7` | Enciende LED índice 6 (GP5) |
| `8` | Enciende LED índice 7 (GP4) |
| `9` | Enciende LEDs 0..7 |
| `0` | Apaga LEDs 0..7 |
| `A` | Enciende LED índice 8 (GP3) |
| `B` | Enciende LED índice 9 (GP2) |
| `C` | Enciende LED índice 10 (GP28) |
| `D` | Enciende LED índice 11 (GP27) |
| `*` | Enciende LEDs 8..11 |
| `#` | Apaga LEDs 8..11 |

## 4) Instrucciones de ejecución

### En Wokwi
1. Crear proyecto Pico.
2. Cargar `src/main.cpp` como sketch.
3. Reproducir conexiones del teclado según `docs/wiring.md`.
4. Conectar 12 LEDs a los GPIO de `ledPins`.
5. Ejecutar simulación y verificar respuesta por tecla.

### En hardware real
1. Cablear teclado y LEDs según tablas de mapeo.
2. Verificar polaridad de LEDs y resistencias en serie.
3. Cargar firmware en Pico W.
4. Validar funcionamiento tecla por tecla y acciones grupales.

## 5) Nota sobre análisis de diagramas
**CODEX NO tiene capacidades de OCR ni visión de imágenes.** Su procesamiento se basa en entradas textuales (código, JSON, descripciones). **ChatGPT sí puede ayudar a describir diagramas de forma interpretativa** cuando se proporciona contexto textual suficiente.

## 6) Sección final solicitada
- **Qué hace el sistema:** controla LEDs a partir de teclado matricial 4x4 con acciones unitarias y grupales.
- **Cómo funciona el flujo del programa:** inicialización de salidas, sondeo continuo de teclado, despacho por `switch-case`, retardo breve de ciclo.
- **Qué representa el diagrama:** topología de conexión entre filas/columnas del keypad y GPIO del Pico, base física para detectar teclas y ejecutar control de salidas.
