+++
title = 'CH552 Multi-Protocol - Modo AVR'
date = 2026-04-13T15:05:00-05:00
draft = false
tarjeta = 'Firmware'
estado = 'Completo'
author = 'Equipo DevLab'
tipos = ['firmware']
modulos = ['general']
resumen = 'Usa el CH552 Multi-Protocol en modo AVR para programar microcontroladores ATmega con soporte USBasp compatible con Arduino IDE.'
como_funciona = [
  'Flashea el firmware AVR en el CH552.',
  'Conecta el programador a tu ATmega.',
  'Selecciona "USBasp" como programador en Arduino IDE.',
  'Carga tu sketch normalmente.'
]
respuesta_rapida = [
  '¿No detecta el programador? Instala drivers libusb (Linux) o Zadig (Windows).',
  '¿Error de sincronización? Verifica las conexiones MISO, MOSI, SCK, RESET.',
  '¿Velocidad lenta? Agrega -B 10 en avrdude para reducir clock.'
]
retro = 'El CH552 en modo AVR emula un USBasp, permitiendo programar ATmega y otros AVRs sin necesitar hardware adicional.'
fotos = ['images/avr/UNIT-UNO-DIY-KIT-ARDUINO.jpg']
tags = ['avr', 'ch552', 'programador', 'multiprotocol', 'atmega']
destacado = false
+++

El **CH552 Multi-Protocol** puede funcionar como programador AVR (USBasp), compatible con Arduino IDE y avrdude.

## Flashear firmware AVR

1. Descarga el firmware AVR para CH552
2. Usa WCHISPStudio para flashear el CH552

```bash
# Linux
wchisptool -f ch552_usbasp.bin

# Windows
# Usar WCHISPStudio GUI
```

## Conexiones programador AVR

```
CH552 AVR Mode    ATmega328P
══════════════    ═══════════
MISO        ────→ MISO (D12)
MOSI        ────→ MOSI (D11)
SCK         ────→ SCK  (D13)
RESET       ────→ RESET
GND         ────→ GND
VCC         ────→ VCC (5V)
```

## Configurar Arduino IDE

1. Ve a **Herramientas → Programador**
2. Selecciona **"USBasp"**
3. Conecta tu placa AVR
4. Usa **Sketch → Subir usando programador** (Ctrl+Shift+U)

## Usar con avrdude

```bash
# Leer firma del chip
avrdude -c usbasp -p m328p

# Flashear archivo .hex
avrdude -c usbasp -p m328p -U flash:w:firmware.hex

# Leer fusibles
avrdude -c usbasp -p m328p -U lfuse:r:-:h -U hfuse:r:-:h

# Escribir fusibles (ejemplo: habilitar clock externo 16MHz)
avrdude -c usbasp -p m328p -U lfuse:w:0xFF:m -U hfuse:w:0xDE:m
```

## Instalar drivers (Windows)

1. Descarga [Zadig](https://zadig.akeo.ie/)
2. Conecta el CH552 en modo AVR
3. Selecciona dispositivo "USBasp"
4. Instala driver **libusbK**

## Drivers Linux

```bash
# Ubuntu/Debian
sudo apt install libusb-dev

# Agregar reglas udev
echo 'SUBSYSTEM=="usb", ATTR{idVendor}=="16c0", ATTR{idProduct}=="05dc", MODE="0666"' | sudo tee /etc/udev/rules.d/99-usbasp.rules

sudo udevadm control --reload
```

## Placas AVR soportadas

- ✅ ATmega328P (Arduino Uno)
- ✅ ATmega2560 (Arduino Mega)
- ✅ ATtiny85, ATtiny13
- ✅ ATmega32U4 (Arduino Leonardo)
- ✅ ATmega8, ATmega16

## Solución de problemas

### Error: device not found

```bash
# Verifica que el dispositivo se detecte
lsusb | grep -i usbasp
# Deberías ver: Bus XXX Device XXX: ID 16c0:05dc Van Ooijen Technische Informatica shared ID for use with libusb
```

### Error: initialization failed

Reduce la velocidad del programador:

```bash
avrdude -c usbasp -p m328p -B 10 -U flash:w:firmware.hex
```

### No puede escribir flash

Verifica que el chip no tenga fusibles bloqueados:

```bash
# Restaurar fusibles de fábrica ATmega328P
avrdude -c usbasp -p m328p -e -U lock:w:0x3F:m
```

## Programar bootloader Arduino

```bash
# ATmega328P con bootloader Optiboot
avrdude -c usbasp -p m328p -e -U flash:w:optiboot_atmega328.hex \
  -U lfuse:w:0xFF:m -U hfuse:w:0xDE:m -U efuse:w:0xFD:m
```

## Ventajas del modo AVR

- ✅ **Compatible USBasp**: Funciona con herramientas estándar
- ✅ **Arduino IDE integrado**: No necesita configuración extra
- ✅ **Open-source**: Firmware modificable
- ✅ **Económico**: Un solo dispositivo para múltiples protocolos
- ✅ **Multi-plataforma**: Linux, Windows, macOS
