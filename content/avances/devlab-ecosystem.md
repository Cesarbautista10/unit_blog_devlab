+++
title = 'Ecosistema DevLab'
date = 2026-04-13T14:50:00-05:00
draft = false
tarjeta = 'Ecosistema'
estado = 'Completo'
author = 'Equipo DevLab'
tipos = ['productos']
modulos = ['general']
resumen = 'Conoce el ecosistema DevLab: sistema modular de conectores, tarjetas de desarrollo y módulos que simplifican el prototipado electrónico.'
como_funciona = [
  'Conecta módulos DevLab usando el conector estándar.',
  'No necesitas soldar - todo es plug-and-play.',
  'Combina sensores, actuadores y microcontroladores.',
  'Expande fácilmente tu proyecto con nuevos módulos.'
]
respuesta_rapida = [
  '¿Qué conector usa? Conector DevLab propietario de 4 pines (alimentación + I2C).',
  '¿Es compatible con QWIIC? Sí, los módulos I2C son compatibles con QWIIC.',
  '¿Puedo soldar cables? Sí, cada módulo tiene pads accesibles.'
]
retro = 'El ecosistema DevLab facilita el prototipado rápido mediante conectores modulares sin necesidad de soldadura.'
fotos = ['images/devlab/unnamed.webp']
tags = ['devlab', 'ecosistema', 'conector', 'modular', 'hardware']
destacado = true
+++

El **Ecosistema DevLab** es un sistema modular de hardware que simplifica el prototipado electrónico mediante conectores estandarizados, permitiendo conexiones rápidas sin soldadura.

## Características principales

### 🔌 Conector DevLab

El conector DevLab es un sistema de 4 pines que incluye:

- **VCC** (3.3V o 5V según el módulo)
- **GND** (tierra)
- **SDA** (I2C datos)
- **SCL** (I2C reloj)

### 🧩 Ventajas del sistema modular

- ✅ **Plug-and-play**: No requiere soldadura
- ✅ **Daisy-chain**: Conecta múltiples módulos en cadena
- ✅ **Compatibilidad I2C**: Compatible con QWIIC/Stemma QT
- ✅ **Rápido prototipado**: Cambia módulos en segundos
- ✅ **Reutilizable**: Los módulos se pueden usar en múltiples proyectos

## Categorías de productos

### Tarjetas de desarrollo

- **ESP32 DevLab**: Microcontrolador WiFi/Bluetooth con conector DevLab
- **CH552 Multi-Protocol**: Programador USB multi-función
- **PY32 DevLab**: ARM Cortex-M0+ ultra económico
- **AVR DevLab**: Basado en ATmega328P compatible Arduino

### Módulos sensores

- Sensor de temperatura y humedad (DHT22)
- Sensor de presión barométrica (BMP280)
- Sensor de luz ambiente (BH1750)
- Acelerómetro/giroscopio (MPU6050)
- Sensor de distancia ultrasónico

### Módulos actuadores

- Display OLED (128x64)
- Matriz LED 8x8
- Relé de 5V
- Servomotor
- Motor DC con driver

### Módulos de comunicación

- Módulo WiFi (ESP-01)
- Bluetooth HC-05
- NRF24L01 (2.4GHz)
- LoRa SX1278

## Ejemplo de conexión

```
┌──────────────┐    DevLab    ┌──────────────┐    DevLab    ┌──────────────┐
│  ESP32       │──────────────│   BMP280     │──────────────│   OLED       │
│  DevLab      │   Conector   │   (Sensor)   │   Conector   │   Display    │
└──────────────┘              └──────────────┘              └──────────────┘
```

Todos los dispositivos comparten el bus I2C y la alimentación.

## Código ejemplo (Arduino IDE)

```cpp
#include <Wire.h>
#include <Adafruit_BMP280.h>
#include <Adafruit_SSD1306.h>

Adafruit_BMP280 bmp;
Adafruit_SSD1306 display(128, 64, &Wire);

void setup() {
  Wire.begin();
  
  if (!bmp.begin(0x76)) {
    Serial.println("BMP280 no encontrado");
    while(1);
  }
  
  if (!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    Serial.println("OLED no encontrado");
    while(1);
  }
  
  display.clearDisplay();
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);
}

void loop() {
  float temp = bmp.readTemperature();
  float pres = bmp.readPressure() / 100.0;
  
  display.clearDisplay();
  display.setCursor(0, 0);
  display.printf("Temp: %.1f C\n", temp);
  display.printf("Pres: %.1f hPa", pres);
  display.display();
  
  delay(1000);
}
```

## Compatibilidad con otros sistemas

El ecosistema DevLab es **compatible** con:

- **QWIIC** (SparkFun)
- **Stemma QT** (Adafruit)
- **Grove I2C** (Seeed Studio) - con adaptador

Los módulos usan el mismo pinout I2C estándar.

## Expansión personalizada

Puedes crear tus propios módulos DevLab:

1. Diseña tu PCB con el conector DevLab (JST-SH 4 pines)
2. Conecta VCC, GND, SDA, SCL según el estándar
3. Programa la dirección I2C única (si es necesario)
4. ¡Listo! Ahora es parte del ecosistema

## Recursos

- 📘 Guía de inicio rápido
- 🔧 Librerías Arduino para todos los módulos
- 📐 Archivos KiCad de referencia
- 📹 Tutoriales en video
- 💬 Comunidad en Discord
