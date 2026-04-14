+++
title = 'PyOCD - Depuración ARM Cortex-M'
date = 2026-04-13T14:45:00-05:00
draft = false
tarjeta = 'Plataformas'
estado = 'Completo'
author = 'Equipo DevLab'
tipos = ['documentacion-general']
modulos = ['general']
resumen = 'Herramienta de depuración y programación para microcontroladores ARM Cortex-M usando interfaces CMSIS-DAP y DAPLink.'
como_funciona = [
  'Instala PyOCD con pip install pyocd.',
  'Conecta tu programador CMSIS-DAP al microcontrolador.',
  'Usa comandos pyocd para flashear y depurar.',
  'Opcional: integra con VS Code para depuración gráfica.'
]
respuesta_rapida = [
  '¿No detecta el dispositivo? Verifica que el programador sea CMSIS-DAP.',
  '¿Error de target? Instala el pack: pyocd pack install <pack_name>.',
  '¿Problemas de permisos? En Linux agrega reglas udev.'
]
retro = 'PyOCD es una herramienta Python open-source para debugging y programación de microcontroladores ARM Cortex-M.'
fotos = ['images/devlab/pyocd_logo.webp']
tags = ['pyocd', 'arm', 'debugging', 'cortex-m', 'cmsis-dap']
destacado = false
+++

PyOCD es una herramienta de línea de comandos y librería Python para depurar y programar microcontroladores ARM Cortex-M usando sondas CMSIS-DAP/DAPLink.

## Instalación

```bash
# Instalar PyOCD
pip install pyocd

# Verificar instalación
pyocd --version

# Listar dispositivos conectados
pyocd list
```

## Instalar packs de soporte

Para microcontroladores específicos:

```bash
# Buscar pack
pyocd pack find stm32

# Instalar pack
pyocd pack install stm32f4

# Para PY32F003
pyocd pack install puya
```

## Flashear firmware

```bash
# Flashear archivo .hex
pyocd flash -t stm32f103 firmware.hex

# Flashear archivo .bin (especificar dirección)
pyocd flash -t stm32f103 --base-address 0x08000000 firmware.bin

# Auto-detectar target
pyocd flash firmware.hex
```

## Depuración con GDB

```bash
# Iniciar servidor GDB
pyocd gdbserver -t stm32f103

# En otra terminal, conectar GDB
arm-none-eabi-gdb firmware.elf
(gdb) target remote :3333
(gdb) load
(gdb) continue
```

## Integración con VS Code

Archivo `.vscode/launch.json`:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "PyOCD Debug",
      "type": "cppdbg",
      "request": "launch",
      "program": "${workspaceFolder}/build/firmware.elf",
      "args": [],
      "stopAtEntry": true,
      "cwd": "${workspaceFolder}",
      "environment": [],
      "externalConsole": false,
      "MIMode": "gdb",
      "miDebuggerPath": "arm-none-eabi-gdb",
      "miDebuggerServerAddress": "localhost:3333",
      "setupCommands": [
        {
          "text": "target remote localhost:3333"
        },
        {
          "text": "monitor reset halt"
        },
        {
          "text": "load"
        }
      ]
    }
  ]
}
```

## Comandos útiles

```bash
# Leer memoria
pyocd commander -t stm32f103
>>> read32 0x08000000

# Borrar flash
pyocd erase -t stm32f103 --chip

# Info del target
pyocd info

# Reset del microcontrolador
pyocd reset -t stm32f103
```

## Configuración para PY32F003

```bash
# Instalar pack Puya
pyocd pack install puya

# Flashear PY32
pyocd flash -t py32f003 firmware.hex

# Depurar
pyocd gdbserver -t py32f003
```

## Permisos en Linux

Crear `/etc/udev/rules.d/99-cmsis-dap.rules`:

```
# CMSIS-DAP compatible devices
SUBSYSTEM=="usb", ATTR{idVendor}=="0d28", MODE="0666"
SUBSYSTEM=="usb", ATTR{idVendor}=="c251", MODE="0666"
```

Aplicar reglas:

```bash
sudo udevadm control --reload
sudo udevadm trigger
```

## Ventajas de PyOCD

- ✅ Open-source y multi-plataforma
- ✅ Soporte para múltiples fabricantes ARM
- ✅ Integración con VS Code, Eclipse, etc.
- ✅ API Python para automatización
- ✅ Compatible con CMSIS-DAP estándar
