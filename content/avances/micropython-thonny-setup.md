+++
title = 'MicroPython con Thonny IDE'
date = 2026-04-13T14:05:00-05:00
draft = false
tarjeta = 'Plataformas'
estado = 'Completo'
author = 'Equipo DevLab'
tipos = ['documentacion-general']
modulos = ['general']
resumen = 'Configura Thonny IDE para usar MicroPython en las tarjetas de desarrollo UNIT Electronics.'
como_funciona = [
  'Instala Thonny IDE en tu computadora.',
  'Carga el firmware de MicroPython en tu tarjeta.',
  'Configura el intérprete de MicroPython en Thonny.',
  'Conecta tu tarjeta y comienza a programar en Python.'
]
respuesta_rapida = [
  '¿No detecta el intérprete? Ve a Herramientas > Opciones > Intérprete y selecciona MicroPython.',
  '¿Firmware corrupto? Re-flashea el firmware usando esptool o la herramienta oficial.'
]
retro = 'MicroPython permite programar microcontroladores usando Python, ideal para prototipado rápido.'
fotos = ['images/devlab/thonny-ide-apprendre-python-42.webp']
tags = ['micropython', 'thonny', 'python', 'plataformas']
destacado = true
+++

Aprende a configurar Thonny IDE para programar tus tarjetas de desarrollo UNIT Electronics con MicroPython. Una forma rápida y sencilla de usar Python en microcontroladores.

## ¿Qué es MicroPython?

MicroPython es una implementación eficiente de Python 3 optimizada para microcontroladores. Te permite usar la sintaxis familiar de Python para programar hardware.

## Instalación

1. **Descarga Thonny IDE** desde [thonny.org](https://thonny.org)
2. **Instala Thonny** en tu sistema operativo
3. **Descarga el firmware MicroPython** para tu tarjeta desde el sitio oficial
4. **Flashea el firmware** usando esptool (para ESP32) o la herramienta apropiada

## Configuración de Thonny

1. Abre Thonny IDE
2. Ve a **Herramientas > Opciones > Intérprete**
3. Selecciona **MicroPython (ESP32)** o la opción correspondiente a tu tarjeta
4. Selecciona el **puerto COM** correcto
5. Haz clic en **Aceptar**

## Primer programa

```python
from machine import Pin
import time

led = Pin(2, Pin.OUT)

while True:
    led.value(1)
    time.sleep(1)
    led.value(0)
    time.sleep(1)
```

¡Ya puedes programar en Python!
