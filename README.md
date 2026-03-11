# 🔵 Arduino UNO R4 WiFi — LED Matrix Text & Animation Display

**Project by Amimer Ayoub**

A complete Arduino project that displays custom text letter by letter with animations on the built-in **8×13 LED matrix** of the Arduino UNO R4 WiFi board.

---

## 📋 Table of Contents

- [Hardware Requirements](#hardware-requirements)
- [Software Requirements](#software-requirements)
- [Project Features](#project-features)
- [File Structure](#file-structure)
- [How It Works](#how-it-works)
- [Animation Sequence](#animation-sequence)
- [Customization](#customization)
- [Font Reference](#font-reference)
- [Troubleshooting](#troubleshooting)
- [Known Issues Fixed](#known-issues-fixed)

---

## 🔧 Hardware Requirements

| Component | Details |
|---|---|
| **Board** | Arduino UNO R4 WiFi |
| **LED Matrix** | Built-in 8×13 matrix (on-board) |
| **Cable** | USB-C |
| **Power** | Via USB or 5V external |

---

## 💻 Software Requirements

| Software | Version | Link |
|---|---|---|
| Arduino IDE | 2.x or higher | [arduino.cc](https://www.arduino.cc) |
| Arduino UNO R4 Board Support | Latest | Via Board Manager |
| Arduino_LED_Matrix Library | Latest | Via Library Manager |

### Installing the Library

1. Open Arduino IDE
2. Go to **Tools → Manage Libraries**
3. Search for `Arduino UNO R4`
4. Install **Arduino UNO R4** board support package
5. The `Arduino_LED_Matrix.h` library is included automatically

---

## ✨ Project Features

- ✅ Displays full text **letter by letter** on the LED matrix
- ✅ Each letter fills the **full 8×13 screen**
- ✅ **Flash effect** between each letter
- ✅ **Intro animation** (diagonal wave)
- ✅ **Mid animation** (explosion effect)
- ✅ **Outro animations** (heart beat, smile face, blinking star)
- ✅ **Progressive letter appearance** (lines appear one by one)
- ✅ Full **A–Z alphabet** font included
- ✅ Easy to **customize the text** in one line
- ✅ Zero compilation errors — no `for` loop indexing issues

---

## 📁 File Structure

```
amimer-ayoub/
│
├── sketch/
│   └── sketch.ino        ← Main Arduino sketch
│
└── README.md             ← This file
```

---

## ⚙️ How It Works

### Matrix Format

Each character is stored as a direct pixel matrix:

```cpp
byte A[8][13] = {
  {0,0,0,0,1,1,0,0,0,0,0,0,0},
  {0,0,0,1,0,0,1,0,0,0,0,0,0},
  {0,0,1,0,0,0,0,1,0,0,0,0,0},
  {0,0,1,0,0,0,0,1,0,0,0,0,0},
  {0,0,1,1,1,1,1,1,0,0,0,0,0},
  {0,0,1,0,0,0,0,1,0,0,0,0,0},
  {0,0,1,0,0,0,0,1,0,0,0,0,0},
  {0,0,0,0,0,0,0,0,0,0,0,0,0},
};
```

- `1` = LED **ON**
- `0` = LED **OFF**
- Grid size: **8 rows × 13 columns**

### Display Method

```cpp
matrix.renderBitmap(A, 8, 13);
```

Each letter is called **directly** — no array indexing, no casting — which prevents all common compilation errors.

### Text Selection

The text is defined in one single line at the top of the sketch:

```cpp
const char* TEXT = "AMIMER AYOUB";
```

The `getChar()` function maps each character to its pixel matrix and returns a pointer to the correct letter frame.

---

## 🎬 Animation Sequence

Each full loop runs the following sequence:

| Step | Event | Duration |
|---|---|---|
| 1 | 〰️ **Wave intro** — diagonal sweep animation | ~1.2s |
| 2 | 📝 **Letters** — first half of text, one per screen | ~0.8s each |
| 3 | 💥 **Explosion** — burst effect mid-text | ~0.9s |
| 4 | 📝 **Letters** — second half of text | ~0.8s each |
| 5 | ❤️ **Heart beat** — pulses 3 times | ~1.7s |
| 6 | 😊 **Smile face** — static display | ~1.2s |
| 7 | ⭐ **Star blink** — flashes 4 times | ~1.4s |
| 8 | 🔄 **Loop restart** | — |

---

## 🎨 Customization

### Change the Text

Open `sketch.ino` and change this line:

```cpp
// Line 5 of sketch.ino
const char* TEXT = "AMIMER AYOUB";

// Examples:
const char* TEXT = "BONJOUR";
const char* TEXT = "HELLO";
const char* TEXT = "ARDUINO";
const char* TEXT = "MAROC";
```

> ⚠️ Only **uppercase A–Z** and **spaces** are supported.

### Change Letter Speed

```cpp
// In showLetter() function — change 600 to speed up or slow down
matrix.renderBitmap(ch, 8, 13);
delay(600);   // ← Letter display time in ms
```

### Change Flash Duration

```cpp
void flash() {
  matrix.renderBitmap(blank, 8, 13);
  delay(150);   // ← Flash duration in ms
}
```

---

## 🔤 Font Reference

All 26 letters are included in pixel art format (8×13):

| Letter | Included | Letter | Included |
|---|---|---|---|
| A | ✅ | N | ✅ |
| B | ✅ | O | ✅ |
| C | ✅ | P | ✅ |
| D | ✅ | Q | ✅ |
| E | ✅ | R | ✅ |
| F | ✅ | S | ✅ |
| G | ✅ | T | ✅ |
| H | ✅ | U | ✅ |
| I | ✅ | V | ✅ |
| J | ✅ | W | ✅ |
| K | ✅ | X | ✅ |
| L | ✅ | Y | ✅ |
| M | ✅ | Z | ✅ |

Space character `' '` is also supported.

---

## 🛠️ Troubleshooting

| Problem | Cause | Solution |
|---|---|---|
| `'ArduinoLEDMatrix' not declared` | Library not installed | Install Arduino UNO R4 board support |
| `too many initializers` | Row has more than 13 values | Count values in each row — must be exactly 13 |
| `lvalue required as unary '&'` | Using cast with `renderBitmap` | Use direct variable, no cast |
| `usleep not declared` | PC function used in Arduino | Use `delay()` instead |
| `int main()` error | Arduino doesn't use `main()` | Use `setup()` and `loop()` |
| All LEDs ON | Bits inverted | Check `(bits >> row) & 0x01` reading |
| Text misaligned | Padding columns in buffer | Remove initial padding zeros |

---

## ✅ Known Issues Fixed

Throughout development, the following bugs were identified and resolved:

1. **`usleep` not declared** → Replaced with Arduino `delay()`
2. **`int main()` not valid** → Replaced with `setup()` + `loop()`
3. **`lvalue required` error** → Removed illegal cast `(byte(*)[12])`, used direct variables
4. **Too many initializers** → Fixed `star` matrix row with 14 elements instead of 13
5. **Text decalage (shift)** → Removed initial padding buffer and fixed reset logic
6. **All LEDs ON** → Fixed bit reading order with `(bits >> row) & 0x01`
7. **Matrix size** → Updated all declarations from `[8][12]` to `[8][13]`

---

## 👤 Author

**Amimer Ayoub**
Arduino UNO R4 WiFi — LED Matrix Project
Built with ❤️ using Arduino IDE

---

## 📄 License

This project is open source and free to use for personal and educational purposes.
