+++
title = 'RP2040 - ARM Firmware para CH552'
date = 2026-04-13T15:15:00-05:00
draft = false
tarjeta = 'Firmware'
estado = 'Completo'
author = 'Equipo DevLab'
tipos = ['microcontroladores']
modulos = ['general']
resumen = 'Carga firmware ARM en el CH552 para programar microcontroladores RP2040 (Raspberry Pi Pico) mediante interfaz SWD.'
como_funciona = [
  'Flashea firmware RP2040 en el CH552.',
  'Conecta mediante SWD al RP2040.',
  'Usa OpenOCD o PyOCD para programación.',
  'Depura con GDB si es necesario.'
]
respuesta_rapida = [
  '¿No detecta el Pico? Mantén BOOTSEL presionado al conectar.',
  '¿Error SWD? Verifica alimentación 3.3V estable.',
  '¿OpenOCD falla? Usa PyOCD como alternativa.'
]
retro = 'El RP2040 es el microcontrolador de Raspberry Pi, dual-core ARM Cortex-M0+ con PIO programable.'
fotos = ['images/devlab/AR3578-UNIT-DualMCU-ESP32-RP2040-12.jpg']
tags = ['rp2040', 'pico', 'arm', 'ch552', 'programacion']
destacado = false
+++

El **RP2040** es el microcontrolador de Raspberry Pi, con dual-core ARM Cortex-M0+ @ 133MHz, ideal para proyectos avanzados.

## Características RP2040

- 🧠 **CPU**: Dual ARM Cortex-M0+ @ 133MHz
- 💾 **RAM**: 264KB SRAM
- 🔌 **GPIO**: 30 pines multifunción
- ⚡ **PIO**: Programable I/O (8 state machines)
- 📡 **USB**: USB 1.1 device/host nativo
- 🔢 **ADC**: 12-bit, 500ksps

## Conexiones CH552 → RP2040

```
CH552 (SWD Mode)    Raspberry Pi Pico
═════════════════   ══════════════════
SWDIO         ────→ SWDIO (Pin 3)
SWDCLK        ────→ SWDCLK (Pin 4)
GND           ────→ GND (Pin 8)
3.3V          ────→ 3V3 (Pin 36)
```

## Programar con PyOCD

```bash
# Instalar PyOCD
pip install pyocd

# Instalar pack RP2040
pyocd pack install rp2040

# Flashear firmware .uf2
pyocd flash -t rp2040 firmware.uf2

# Flashear .elf
pyocd flash -t rp2040 firmware.elf
```

## Modo UF2 (sin programador)

El RP2040 tiene bootloader USB integrado:

1. Mantén presionado **BOOTSEL**
2. Conecta USB
3. Aparece como unidad de almacenamiento "RPI-RP2"
4. Arrastra el archivo `.uf2` a la unidad
5. ¡Listo! Se reinicia automáticamente

## Arduino IDE con RP2040

### Instalar soporte

```
Archivo → Preferencias → URLs Adicionales:
https://github.com/earlephilhower/arduino-pico/releases/download/global/package_rp2040_index.json
```

### Configurar placa

```
Herramientas → Placa: Raspberry Pi Pico
Herramientas → Puerto: (selecciona puerto COM/ttyACM)
```

### Ejemplo Blink

```cpp
void setup() {
  pinMode(LED_BUILTIN, OUTPUT);
}

void loop() {
  digitalWrite(LED_BUILTIN, HIGH);
  delay(1000);
  digitalWrite(LED_BUILTIN, LOW);
  delay(1000);
}
```

## PlatformIO para RP2040

`platformio.ini`:

```ini
[env:pico]
platform = raspberrypi
board = pico
framework = arduino
upload_protocol = picotool
```

## PIO (Programmable I/O)

El RP2040 tiene PIO state machines programables:

```cpp
#include <hardware/pio.h>

// Generar PWM usando PIO
void setup() {
  PIO pio = pio0;
  uint offset = pio_add_program(pio, &pwm_program);
  pwm_program_init(pio, 0, offset, 25);
}
```

## Dual-Core ejemplo

```cpp
#include <pico/multicore.h>

void core1_entry() {
  while(true) {
    Serial.println("Core 1 ejecutándose");
    delay(1000);
  }
}

void setup() {
  Serial.begin(115200);
  multicore_launch_core1(core1_entry);
}

void loop() {
  Serial.println("Core 0 ejecutándose");
  delay(1000);
}
```

## USB HID (Teclado/Mouse)

```cpp
#include <Adafruit_TinyUSB.h>

Adafruit_USBD_HID usb_hid;

void setup() {
  usb_hid.begin();
  
  // Simular tecla
  uint8_t keys[6] = {HID_KEY_A};
  usb_hid.keyboardReport(0, 0, keys);
  delay(100);
  usb_hid.keyboardRelease(0);
}
```

## Depurar con GDB

```bash
# Iniciar OpenOCD
openocd -f interface/cmsis-dap.cfg -f target/rp2040.cfg

# En otra terminal
arm-none-eabi-gdb firmware.elf
(gdb) target remote :3333
(gdb) load
(gdb) break main
(gdb) continue
```

## MicroPython en RP2040

```bash
# Descargar MicroPython UF2
wget https://micropython.org/download/rp2-pico/rp2-pico-latest.uf2

# Entrar en modo BOOTSEL
# Copiar .uf2 a la unidad RPI-RP2

# Conectar con Thonny o screen
screen /dev/ttyACM0 115200
```

```python
from machine import Pin
import time

led = Pin(25, Pin.OUT)

while True:
    led.toggle()
    time.sleep(1)
```

## CircuitPython

```bash
# Descargar CircuitPython
wget https://circuitpython.org/board/raspberry_pi_pico/

# Flashear en modo BOOTSEL
# Copiar .uf2 a RPI-RP2

# Editar code.py
```

```python
import board
import digitalio
import time

led = digitalio.DigitalInOut(board.LED)
led.direction = digitalio.Direction.OUTPUT

while True:
    led.value = not led.value
    time.sleep(1)
```

## Ventajas del RP2040

- ✅ **Dual-core**: Procesamiento paralelo real
- ✅ **PIO**: Hardware customizable en software
- ✅ **USB nativo**: HID, CDC, MIDI sin chips externos
- ✅ **Alta velocidad**: 133MHz overclockeablehasta 250MHz
- ✅ **Económico**: ~$1 USD el chip
- ✅ **Versatilidad**: Arduino, MicroPython, CircuitPython, C/C++
