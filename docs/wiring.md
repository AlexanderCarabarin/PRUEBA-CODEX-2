# Cableado y Mapeo GPIO

## 1) Componentes del sistema

### Componentes explícitos del JSON (Wokwi)
1. Raspberry Pi Pico (`wokwi-pi-pico`)
2. Teclado de membrana 4x4 (`wokwi-membrane-keypad`)

### Componentes implícitos por el código
3. 12 LEDs de salida digital
4. 12 resistencias limitadoras (recomendado 220–330 Ω) para implementación física
5. Cables de conexión y GND común

---

## 2) Conexiones del teclado matricial (según JSON)

| Señal keypad | GPIO Pico |
|---|---|
| C4 | GP16 |
| C3 | GP17 |
| C2 | GP18 |
| C1 | GP19 |
| R4 | GP20 |
| R3 | GP21 |
| R2 | GP22 |
| R1 | GP26 |

Este mapeo coincide con el código:
- `colPins = {19, 18, 17, 16}`  → C1..C4
- `rowPins = {26, 22, 21, 20}`  → R1..R4

---

## 3) Mapeo de LEDs en GPIO

`ledPins = {11, 10, 9, 8, 7, 6, 5, 4, 3, 2, 28, 27}`

| Índice lógico LED | GPIO | Grupo funcional |
|---:|---:|---|
| 0 | GP11 | Numérico |
| 1 | GP10 | Numérico |
| 2 | GP9  | Numérico |
| 3 | GP8  | Numérico |
| 4 | GP7  | Numérico |
| 5 | GP6  | Numérico |
| 6 | GP5  | Numérico |
| 7 | GP4  | Numérico |
| 8 | GP3  | Alfabético |
| 9 | GP2  | Alfabético |
| 10 | GP28 | Alfabético |
| 11 | GP27 | Alfabético |

---

## 4) Explicación del teclado matricial 4x4
Un teclado matricial 4x4 está organizado en **4 filas x 4 columnas** (16 teclas). El controlador realiza barrido de filas/columnas para identificar qué intersección está activa cuando se pulsa una tecla.

En este proyecto, la librería `Keypad` abstrae el escaneo y entrega el carácter de tecla (`'0'..'9'`, `'A'..'D'`, `'*'`, `'#'`). Ese carácter se usa directamente en un `switch-case` para ejecutar acciones sobre los LEDs.

---

## 5) Explicación del bloque de LEDs
Los 12 LEDs están divididos funcionalmente en dos subconjuntos:

- **Banco numérico (LED 0–7):** asociado a teclas `1..8`, con acciones grupales en `9` (encender todos) y `0` (apagar todos).
- **Banco alfabético (LED 8–11):** asociado a teclas `A..D`, con acciones grupales en `*` (encender todos) y `#` (apagar todos).

Esto permite control individual y por grupo desde un único periférico de entrada (keypad).
