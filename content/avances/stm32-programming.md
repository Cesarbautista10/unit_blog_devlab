+++
title = 'STM32 - Programación ARM con CH552'
date = 2026-04-13T15:20:00-05:00
draft = false
tarjeta = 'Firmware'
estado = 'Completo'
author = 'Equipo DevLab'
tipos = ['microcontroladores']
modulos = ['general']
resumen = 'Programa microcontroladores STM32 (Blue Pill y otros) usando el CH552 en modo CMSIS-DAP con Arduino IDE, STM32CubeIDE y PyOCD.'
como_funciona = [
  'Conecta CH552 en modo CMSIS-DAP al STM32 vía SWD.',
  'Instala soporte STM32 en Arduino IDE (opcional).',
  'Usa PyOCD o ST-LINK tools para flashear.',
  'Depura con GDB o STM32CubeIDE.'
]
respuesta_rapida = [
  '¿No encuentra el STM32? Verifica alimentación 3.3V.',
  '¿Flash protegido? Usa st-flash para desbloquear: st-flash unlock.',
  '¿Arduino no compila? Verifica que instalaste el core STM32duino.'
]
retro = 'Los STM32 son la familia ARM más popular, con excelente ecosistema y soporte para Arduino IDE.'
fotos = ['images/stm32/AR1576-STM32F103C8T6-Tarjeta-de-Desarrollo-4.jpg']
tags = ['stm32', 'arm', 'bluepill', 'ch552', 'programacion']
destacado = false
+++

La familia **STM32** de STMicroelectronics es la más popular de microcontroladores ARM Cortex-M, con miles de variantes.

## STM32 Blue Pill (STM32F103C8T6)

- 🧠 **CPU**: ARM Cortex-M3 @ 72MHz
- 💾 **Flash**: 64KB (muchos tienen 128KB)
- 🧮 **RAM**: 20KB
- 📌 **GPIO**: 37 pines
- 🔌 **Periféricos**: 2× UART, 2× SPI, 2× I2C, USB, CAN

## Conexiones CH552 → STM32

```
CH552 (CMSIS-DAP)    STM32F103
══════════════════   ══════════
SWDIO          ────→ SWDIO (PA13)
SWDCLK         ────→ SWDCLK (PA14)
GND            ────→ GND
3.3V           ────→ 3.3V
```

## Programar con PyOCD

```bash
# Instalar PyOCD
pip install pyocd

# Instalar pack STM32
pyocd pack install stm32f1

# Flashear firmware
pyocd flash -t stm32f103c8 firmware.hex

# Otras variantes
pyocd flash -t stm32f401 firmware.hex
pyocd flash -t stm32f407 firmware.hex
```

## Arduino IDE con STM32

### Instalar STM32duino

```
Archivo → Preferencias → URLs Adicionales:
https://github.com/stm32duino/BoardManagerFiles/raw/main/package_stmicroelectronics_index.json
```

### Configurar placa

```
Herramientas → Placa → STM32 boards → Generic STM32F1 series
Herramientas → Board part number: BluePill F103C8
Herramientas → Upload method: CMSIS-DAP
```

### Ejemplo Blink

```cpp
void setup() {
  pinMode(PC13, OUTPUT);  // LED integrado
}

void loop() {
  digitalWrite(PC13, HIGH);
  delay(1000);
  digitalWrite(PC13, LOW);
  delay(1000);
}
```

## PlatformIO para STM32

`platformio.ini`:

```ini
[env:bluepill_f103c8]
platform = ststm32
board = bluepill_f103c8
framework = arduino
upload_protocol = cmsis-dap
debug_tool = cmsis-dap
```

## STM32CubeIDE

1. Descarga [STM32CubeIDE](https://www.st.com/en/development-tools/stm32cubeide.html)
2. Crea nuevo proyecto STM32
3. Configurar debugger:
   - Run → Debug Configurations
   - Interface: CMSIS-DAP

## HAL Library ejemplo

```c
#include "stm32f1xx_hal.h"

int main(void) {
  HAL_Init();
  
  __HAL_RCC_GPIOC_CLK_ENABLE();
  
  GPIO_InitTypeDef GPIO_InitStruct = {0};
  GPIO_InitStruct.Pin = GPIO_PIN_13;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
  HAL_GPIO_Init(GPIOC, &GPIO_InitStruct);
  
  while(1) {
    HAL_GPIO_TogglePin(GPIOC, GPIO_PIN_13);
    HAL_Delay(1000);
  }
}
```

## Desbloquear flash protegido

```bash
# Con PyOCD
pyocd erase --chip -t stm32f103c8

# Con st-flash (si tienes stlink tools)
st-flash unlock
st-flash erase
```

## Depuración con GDB

```bash
# Iniciar servidor GDB con PyOCD
pyocd gdbserver -t stm32f103c8

# En otra terminal
arm-none-eabi-gdb firmware.elf
(gdb) target remote :3333
(gdb) load
(gdb) break main
(gdb) continue
```

## Bootloader STM32

El STM32 tiene bootloader UART integrado:

```bash
# Activar bootloader
# Conectar BOOT0 a 3.3V
# Reset el STM32

# Flashear por UART
stm32flash -w firmware.bin /dev/ttyUSB0
```

## STM32 con USB (STM32F103)

```cpp
#include <USBComposite.h>

void setup() {
  USBComposite.setProductId(0x0031);
  HID.begin(HID_KEYBOARD_MOUSE);
  
  // Simular tecla
  Keyboard.print("Hola desde STM32!");
}

void loop() {
  delay(5000);
}
```

## Familias STM32 populares

### STM32F0 (Cortex-M0)
- Entry-level, bajo costo
- 48MHz, hasta 64KB Flash

### STM32F1 (Cortex-M3)
- Blue Pill, económico
- 72MHz, USB, CAN

### STM32F4 (Cortex-M4)
- Alto rendimiento
- 168MHz, FPU, DSP instructions

### STM32L0/L4 (Cortex-M0+/M4)
- Ultra bajo consumo
- Ideal para batería

### STM32H7 (Cortex-M7)
- Máximo rendimiento
- 480MHz, doble precisión FPU

## Ventajas STM32

- ✅ **Variedad**: Miles de modelos diferentes
- ✅ **Ecosistema**: STM32CubeIDE, HAL, LL drivers
- ✅ **Arduino compatible**: STM32duino oficial
- ✅ **Económico**: Blue Pill <$2 USD
- ✅ **Periféricos avanzados**: USB, CAN, SDIO, etc.
- ✅ **Documentación**: Excelente soporte de ST
