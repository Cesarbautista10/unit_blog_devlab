+++
title = 'Comunicación I2C'
date = 2026-04-13T14:40:00-05:00
draft = false
tarjeta = 'Ejemplos'
estado = 'Completo'
author = 'Equipo DevLab'
tipos = ['firmware']
modulos = ['comunicacion', 'esp32']
resumen = 'Aprende a usar el protocolo I2C para comunicarte con sensores y módulos, escanear direcciones y manejar múltiples dispositivos en el mismo bus.'
como_funciona = [
  'Conecta SDA y SCL del módulo al microcontrolador.',
  'Inicializa el bus I2C con la librería Wire.',
  'Escanea direcciones o comunica con dispositivos.',
  'Lee/escribe datos usando el protocolo I2C.'
]
respuesta_rapida = [
  '¿No detecta dispositivos? Verifica las conexiones pull-up (4.7kΩ).',
  '¿Errores de comunicación? Reduce la velocidad del bus (100kHz).',
  '¿Dirección desconocida? Usa el scanner I2C para encontrarla.'
]
retro = 'I2C es un protocolo de comunicación serie que permite conectar múltiples dispositivos (sensores, displays, EEPROMs) con solo 2 cables.'
fotos = ['https://unit-electronics.github.io/CH552_Curso_introductorio/docs/10-comunicacion_i2c/images/oled2.png']
tags = ['i2c', 'comunicacion', 'protocolo', 'ejemplos', 'wire']
destacado = true
+++

I2C (Inter-Integrated Circuit) es un protocolo de comunicación serie síncrono que utiliza solo 2 líneas: SDA (datos) y SCL (reloj).

## Scanner I2C (Arduino)

```cpp
#include <Wire.h>

void setup() {
  Serial.begin(115200);
  Wire.begin();
  Serial.println("\nEscaneando bus I2C...");
  
  byte count = 0;
  for(byte i = 8; i < 120; i++) {
    Wire.beginTransmission(i);
    if(Wire.endTransmission() == 0) {
      Serial.print("Dispositivo encontrado: 0x");
      Serial.println(i, HEX);
      count++;
    }
  }
  
  if(count == 0) Serial.println("No se encontraron dispositivos");
  else Serial.printf("\nTotal: %d dispositivo(s)", count);
}

void loop() {}
```

## Leer sensor I2C (BMP280)

```cpp
#include <Wire.h>
#include <Adafruit_BMP280.h>

Adafruit_BMP280 bmp;

void setup() {
  Serial.begin(115200);
  
  if(!bmp.begin(0x76)) {  // Dirección I2C del BMP280
    Serial.println("Error: BMP280 no encontrado");
    while(1);
  }
}

void loop() {
  float temp = bmp.readTemperature();
  float pres = bmp.readPressure() / 100.0;
  
  Serial.printf("Temperatura: %.1f°C | Presión: %.1f hPa\n", temp, pres);
  delay(2000);
}
```

## MicroPython I2C

```python
from machine import Pin, I2C
import time

# Inicializar I2C
i2c = I2C(0, scl=Pin(22), sda=Pin(21), freq=100000)

# Escanear dispositivos
devices = i2c.scan()
print("Dispositivos encontrados:", [hex(d) for d in devices])

# Leer de un dispositivo (ejemplo: 0x76)
if 0x76 in devices:
    data = i2c.readfrom(0x76, 6)
    print("Datos:", data)
```

## Conectar múltiples dispositivos

Todos los dispositivos I2C van en paralelo:

```
            +3.3V
              │
    ┌────────┴────────┐
  4.7kΩ            4.7kΩ  (Pull-up resistors)
    │                │
  ──SDA──┬──────┬────┴─────┬──────  
         │      │          │
  ──SCL──┼──────┼──────────┼──────
         │      │          │
       Device1 Device2  Device3
```

## I2C personalizado (sin librería)

```cpp
#define SDA_PIN 21
#define SCL_PIN 22
#define I2C_ADDR 0x3C

void i2cWrite(byte addr, byte data) {
  Wire.beginTransmission(addr);
  Wire.write(data);
  Wire.endTransmission();
}

byte i2cRead(byte addr) {
  Wire.requestFrom(addr, 1);
  if(Wire.available()) {
    return Wire.read();
  }
  return 0;
}
```

## Solución de problemas

- **No detecta dispositivos**: Verifica resistencias pull-up 4.7kΩ en SDA/SCL
- **Comunicación lenta**: Reduce frecuencia a 100kHz: `Wire.setClock(100000)`
- **Conflicto de direcciones**: Cada dispositivo debe tener dirección única
- **Cables largos**: I2C funciona mejor con cables cortos (<30cm)
