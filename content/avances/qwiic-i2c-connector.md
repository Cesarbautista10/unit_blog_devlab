+++
title = 'Conector QWIIC - Comunicación I2C'
date = 2026-04-13T14:15:00-05:00
draft = false
tarjeta = 'Hardware'
estado = 'Completo'
author = 'Equipo DevLab'
tipos = ['productos']
modulos = ['comunicacion']
resumen = 'Configura y utiliza el conector JST SH 1.0mm 4 pines para comunicación I2C en las tarjetas de desarrollo UNIT Electronics.'
como_funciona = [
  'Conecta tu módulo QWIIC al conector JST SH de 4 pines.',
  'Identifica los pines: GND, 3.3V, SDA, SCL.',
  'Inicializa el bus I2C en tu código.',
  'Comunícate con el dispositivo usando su dirección I2C.'
]
respuesta_rapida = [
  '¿No detecta el dispositivo? Escanea el bus I2C para encontrar la dirección.',
  '¿Lecturas erróneas? Verifica las resistencias pull-up (usualmente 4.7kΩ).'
]
retro = 'El sistema QWIIC facilita la conexión rápida de sensores y módulos sin soldadura.'
fotos = ['images/devlab/jst-sh-4-pin-cable-with-alligator-clips-stemma-qt-qwiic-the-pi-hut-ada4398-28609459290307.webp']
tags = ['qwiic', 'i2c', 'hardware', 'conectores']
destacado = true
+++

El conector QWIIC es un estándar de conexión rápida que utiliza comunicación I2C para conectar múltiples sensores y módulos sin necesidad de soldadura.

## Especificaciones del conector

- **Tipo**: JST SH 1.0mm 4 pines
- **Voltaje**: 3.3V
- **Protocolo**: I2C
- **Pinout estándar**:
  1. GND (Negro)
  2. 3.3V (Rojo)
  3. SDA (Azul)
  4. SCL (Amarillo)

## Ventajas del sistema QWIIC

- ✅ Conexión rápida sin soldadura
- ✅ Polaridad protegida (no se puede conectar al revés)
- ✅ Daisy-chain (conectar múltiples dispositivos)
- ✅ Compatible con SparkFun Qwiic y Adafruit STEMMA QT

## Uso con Arduino

```cpp
#include <Wire.h>

void setup() {
  Serial.begin(115200);
  Wire.begin(); // Inicializa I2C
  
  // Escanea dispositivos I2C
  Serial.println("Escaneando...");
  for(byte address = 1; address < 127; address++) {
    Wire.beginTransmission(address);
    byte error = Wire.endTransmission();
    
    if (error == 0) {
      Serial.print("Dispositivo encontrado en 0x");
      Serial.println(address, HEX);
    }
  }
}

void loop() {
  // Tu código aquí
}
```

## Conexión de múltiples dispositivos

Puedes conectar múltiples módulos en serie (daisy-chain) siempre que:
- Cada dispositivo tenga una dirección I2C única
- No excedas la capacitancia máxima del bus (400pF típico)
- Uses resistencias pull-up apropiadas (4.7kΩ usualmente)

## Módulos compatibles

La mayoría de sensores y módulos con conector QWIIC/STEMMA QT son compatibles con las tarjetas UNIT Electronics.
