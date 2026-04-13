+++
title = 'Demo 006 - Emotionally: cara reactiva con MicroPython en Pulsar C6'
date = 2025-05-05T10:00:00-05:00
draft = false
tarjeta = 'DEMO-006'
estado = 'En progreso'
author = 'Cesar'
destacado = true
tipos = ['documentacion-general']
modulos = ['esp32', 'sensores', 'pantallas']
resumen = 'Dale vida a tu placa: una cara emocional que reacciona al movimiento usando Pulsar C6, MPU6050 y una OLED con MicroPython.'
como_funciona = [
  'Se inicializa Pulsar C6 con MicroPython y se cargan los modulos base.',
  'El MPU6050 mide movimiento y clasifica el nivel de agitacion.',
  'La OLED renderiza expresiones segun el estado emocional detectado.'
]
respuesta_rapida = [
  'No detecta movimiento? Revisa direccion I2C del MPU6050 y cableado SDA/SCL.',
  'Pantalla en negro? Confirma alimentacion 3.3V y contraste de la OLED.'
]
retro = 'La logica emocional responde bien a cambios de movimiento; siguiente paso: suavizar transiciones entre estados para mayor naturalidad.'
fotos = ['https://raw.githubusercontent.com/UNIT-Electronics-MX/unit_emotionally_pulsar_c6_mp/refs/heads/main/hardware/resources/14.gif', 
'/images/demo-006/pulsarc6.png'
]

tags = ['micropython', 'pulsar-c6', 'mpu6050', 'oled', 'embebidos']
+++

{{< img
  src="https://raw.githubusercontent.com/UNIT-Electronics-MX/unit_emotionally_pulsar_c6_mp/refs/heads/main/hardware/resources/14.gif"
  alt="Demo Emotionally en ejecucion"
  caption="Demo 006: Emotionally corriendo sobre Pulsar C6"
  size="xl"
  align="center"
>}}

{{< note type="info" title="Publicacion" >}}
Publicado el 5 de mayo de 2025 por **Cesar**. Licencia **MIT**.
{{< /note >}}

## Que es Pulsar C6

Pulsar C6 es una placa formato Nano basada en **ESP32-C6** (arquitectura RISC-V), pensada para trabajar con **MicroPython**, C o Arduino. Integra conectividad moderna y es compatible con accesorios I2C tipo Qwiic.

Caracteristicas principales:

1. Chip ESP32-C6 (Wi-Fi 2.4 GHz + BLE + Thread).
2. Puerto USB-C nativo.
3. Gestion de bateria LiPo.
4. Conector Qwiic I2C.
5. Compatibilidad con pinout tipo Arduino Nano.

## Objetivo del proyecto

El objetivo de **Emotionally** es crear una interfaz visual con personalidad: una cara en OLED que cambia de expresion dependiendo del movimiento detectado por el MPU6050.

Este proyecto enseña como un sistema embebido puede sentirse "vivo" con poco hardware y una logica de estados bien definida.

## Configuracion de software

1. Flashea MicroPython en la Pulsar C6.
2. Sube los archivos `main.py` y `mpu6050.py` con `mpremote` o Thonny.
3. Energiza la placa.
4. Verifica que la cara aparezca en OLED y reaccione al mover la tarjeta.

## Logica de emociones

La expresion se calcula segun intensidad de movimiento:

1. **Feliz**: movimiento suave.
2. **Sorprendido**: agitacion media.
3. **Enojado**: movimiento brusco.
4. **Dormido**: estado quieto (animacion ZzZ).

Las animaciones se dibujan pixel por pixel en la OLED usando rutinas de render en MicroPython.

## Resumen de demo

{{< note type="success" title="Estado actual" >}}
La demo funciona en tiempo real y cambia expresiones correctamente con base en datos del IMU.
{{< /note >}}

## Componentes usados

### Hardware

| Componente | Cantidad |
|------------|----------|
| Pulsar C6 | 1 |
| SSD1306 OLED | 1 |
| MPU6050 | 1 |

### Software y servicios

1. MicroPython.
2. mpremote o Thonny para carga de scripts.

## Notas finales

Este proyecto es ideal para:

1. Educacion STEM.
2. Juguetes IoT o wearables.
3. Robots con caracter.
4. Experimentos de interfaz humano-dispositivo.

## Licencia

Proyecto abierto bajo licencia **MIT**.

## Comienza hoy

Repositorio oficial:

1. [Unit Emotionally - PULSAR C6](https://github.com/UNIT-Electronics-MX/unit_emotionally_pulsar_c6_mp)

## Esquematicos

Agrega aqui el diagrama electrico principal del proyecto cuando lo tengas en `static/images/demo-006/`.

Ejemplo:

{{< diagram src="/images/demo-006/pulsarc6.png" caption="Esquematico de conexion Pulsar C6 + MPU6050 + OLED" height="520" >}}

## Codigo

```python
# main.py (esqueleto simplificado)
from machine import I2C, Pin
import time

# Inicializacion de perifericos
i2c = I2C(0, scl=Pin(9), sda=Pin(8), freq=400000)

def clasificar_emocion(nivel):
    if nivel < 80:
        return "feliz"
    if nivel < 180:
        return "sorprendido"
    if nivel < 320:
        return "enojado"
    return "dormido"

while True:
    # Aqui iria la lectura real del MPU6050
    nivel_movimiento = 120
    emocion = clasificar_emocion(nivel_movimiento)
    # Aqui iria el render en OLED segun emocion
    print("Emocion:", emocion)
    time.sleep_ms(200)
```

## Creditos

1. Autor: Cesar.
2. Comunidad UNIT Electronics.

## Comentarios

Comentarios actuales: **0**.
