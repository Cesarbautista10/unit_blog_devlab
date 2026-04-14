+++
title = 'CH552 Multi-Protocol - Modo CMSIS-DAP'
date = 2026-04-13T15:10:00-05:00
draft = false
tarjeta = 'Firmware'
estado = 'Completo'
author = 'Equipo DevLab'
tipos = ['firmware']
modulos = ['general']
resumen = 'Configura el CH552 en modo CMSIS-DAP para programar y depurar microcontroladores ARM Cortex-M con PyOCD y Arduino IDE.'
como_funciona = [
  'Flashea firmware CMSIS-DAP en el CH552.',
  'Conecta mediante interfaz SWD (SWDIO, SWDCLK).',
  'Usa PyOCD o Arduino IDE para programar.',
  'Depura con GDB si es necesario.'
]
respuesta_rapida = [
  '¿No detecta el target? Verifica conexiones SWD y alimentación.',
  '¿Error de comunicación? Reduce velocidad SWD con PyOCD.',
  '¿Arduino no lo reconoce? Instala paquete de placas ARM.'
]
retro = 'CMSIS-DAP es el estándar de depuración ARM, convirtiendo el CH552 en un programador/debugger profesional.'
fotos = ['images/py32/CH552-USB-programador-multiprotocolo-UNIT-2.webp']
tags = ['cmsis-dap', 'py32', 'arm', 'ch552', 'programacion', 'debugging']
destacado = false
+++

El **CH552 Multi-Protocol** en modo CMSIS-DAP permite programar y depurar microcontroladores ARM Cortex-M usando el estándar de ARM.

## Flashear firmware CMSIS-DAP

```bash
# Descargar firmware
wget https://github.com/UNIT-Electronics/CH552-CMSIS-DAP/releases/latest/ch552_cmsis_dap.bin

# Flashear con WCHISPTool
wchisptool -f ch552_cmsis_dap.bin
```

## Conexiones SWD

```
CH552 CMSIS-DAP    Target ARM
════════════════   ═══════════
SWDIO        ────→ SWDIO
SWDCLK       ────→ SWDCLK
GND          ────→ GND
3.3V         ────→ VDD
RESET (opt)  ────→ RESET
```

## Programar con PyOCD

```bash
# Instalar PyOCD
pip install pyocd

# Listar targets
pyocd list

# Flashear firmware
pyocd flash -t stm32f103 firmware.hex

# Para PY32F003
pyocd pack install puya
pyocd flash -t py32f003 firmware.hex
```

## Depurar con GDB

```bash
# Terminal 1: Iniciar servidor GDB
pyocd gdbserver -t stm32f103

# Terminal 2: Conectar GDB
arm-none-eabi-gdb firmware.elf
(gdb) target remote :3333
(gdb) load
(gdb) break main
(gdb) continue
```

## Arduino IDE con PY32F003

### 1. Instalar soporte PY32

```
Archivo → Preferencias → URLs Adicionales:
https://github.com/py32duino/platform-py32f0.git
```

### 2. Configurar programador

```
Herramientas → Placa: PY32F003
Herramientas → Programador: CMSIS-DAP
```

### 3. Cargar sketch

```cpp
void setup() {
  pinMode(PA0, OUTPUT);
}

void loop() {
  digitalWrite(PA0, HIGH);
  delay(1000);
  digitalWrite(PA0, LOW);
  delay(1000);
}
```

Presiona **Ctrl+U** para cargar.

## VS Code con PlatformIO

`platformio.ini`:

```ini
[env:py32f003]
platform = py32f0
board = py32f003f24p6
framework = arduino
upload_protocol = cmsis-dap
debug_tool = cmsis-dap
```

## Microcontroladores compatibles

### ARM Cortex-M0/M0+
- PY32F003, PY32F030
- STM32F0 series
- LPC11xx series
- GD32E103

### ARM Cortex-M3
- STM32F103 (Blue Pill)
- LPC1768
- EFM32

### ARM Cortex-M4
- STM32F4 series
- TM4C123
- NRF52840

## Verificar conexión

```bash
# Detectar dispositivo CMSIS-DAP
pyocd list

# Debería mostrar:
#   0 => CMSIS-DAP CMSIS-DAP [Serial: XXXX]
```

## Velocidad SWD

```bash
# Velocidad por defecto (1MHz)
pyocd flash firmware.hex

# Reducir velocidad si hay errores
pyocd flash firmware.hex --frequency 100000
```

## Solución de problemas

### No detecta el target

1. Verifica alimentación (3.3V en VDD)
2. Verifica conexiones SWDIO/SWDCLK
3. Prueba con RESET conectado
4. Reduce velocidad SWD

### Error "Wire ACK fault"

```bash
# Conectar RESET y forzar reset durante conexión
pyocd flash --frequency 100000 --connect-mode under-reset firmware.hex
```

### Windows: driver no instalado

1. Descarga [Zadig](https://zadig.akeo.ie/)
2. Selecciona "CMSIS-DAP"
3. Instala driver **WinUSB**

## Depuración avanzada

```bash
# Configurar breakpoints de hardware
(gdb) hbreak main

# Ver registros ARM
(gdb) info registers

# Ver memoria flash
(gdb) x/16x 0x08000000

# Continuar hasta breakpoint
(gdb) continue
```

## Ventajas CMSIS-DAP

- ✅ **Estándar ARM**: Compatible con todas las herramientas
- ✅ **Debugging real**: Breakpoints, step-by-step
- ✅ **Multi-vendor**: STM32, NXP, Puya, etc.
- ✅ **Open-source**: Firmware modificable
- ✅ **VS Code/PlatformIO**: Integración completa
