+++
title = 'Blink - Parpadeo de LED'
date = 2026-04-13T14:20:00-05:00
draft = false
tarjeta = 'Ejemplos'
estado = 'Completo'
author = 'Equipo DevLab'
tipos = ['firmware']
modulos = ['esp32', 'general']
resumen = 'Aprende a hacer parpadear un LED en diferentes plataformas y microcontroladores usando Arduino IDE, MicroPython y SDCC.'
como_funciona = [
  'Configura un pin GPIO como salida.',
  'Activa el pin (HIGH) para encender el LED.',
  'Espera un tiempo determinado.',
  'Desactiva el pin (LOW) para apagar el LED.',
  'Repite el ciclo.'
]
respuesta_rapida = [
  '¿El LED no enciende? Verifica la polaridad y resistencia limitadora.',
  '¿Parpadea muy rápido? Aumenta el tiempo de delay.'
]
retro = 'Blink es el "Hola Mundo" del hardware - el primer programa que todo desarrollador debe hacer.'
fotos = ['https://unit-electronics.github.io/CH552_Curso_introductorio/docs/9-controlador_pwm/images/led.gif']
tags = ['blink', 'led', 'gpio', 'ejemplos', 'basico']
destacado = true
+++

El clásico ejemplo "Blink" es la mejor forma de verificar que tu configuración funciona correctamente. Aprende a hacerlo en diferentes plataformas.

## Arduino IDE (ESP32, AVR, ARM)

```cpp
// Definir el pin del LED (puede variar según la tarjeta)
#define LED_PIN 2  // GPIO2 en ESP32, pin 13 en Arduino UNO

void setup() {
  // Configurar el pin como salida
  pinMode(LED_PIN, OUTPUT);
}

void loop() {
  digitalWrite(LED_PIN, HIGH);   // Encender LED
  delay(1000);                   // Esperar 1 segundo
  digitalWrite(LED_PIN, LOW);    // Apagar LED
  delay(1000);                   // Esperar 1 segundo
}
```

## MicroPython (ESP32, RP2040)

```python
from machine import Pin
import time

# Crear objeto Pin (GPIO2 en ESP32)
led = Pin(2, Pin.OUT)

while True:
    led.value(1)      # Encender LED
    time.sleep(1)     # Esperar 1 segundo
    led.value(0)      # Apagar LED
    time.sleep(1)     # Esperar 1 segundo
```

## SDCC (CH552)

```c
#include <ch552.h>

#define LED_PIN 3  // P33

void delay_ms(unsigned int ms) {
    unsigned int i, j;
    for(i = 0; i < ms; i++)
        for(j = 0; j < 120; j++);
}

void main() {
    // Configurar pin como salida
    P3_MOD_OC &= ~(1 << 3);  // Modo push-pull
    P3_DIR_PU |= (1 << 3);   // Dirección salida
    
    while(1) {
        LED_PIN = 1;  // Encender
        delay_ms(1000);
        LED_PIN = 0;  // Apagar
        delay_ms(1000);
    }
}
```

## Variaciones del ejemplo

### Blink más rápido
```cpp
delay(100);  // 100ms - parpadeo rápido
```

### Patrón SOS en Morse
```cpp
// S: 3 parpadeos cortos
for(int i=0; i<3; i++) {
  digitalWrite(LED_PIN, HIGH);
  delay(200);
  digitalWrite(LED_PIN, LOW);
  delay(200);
}

// O: 3 parpadeos largos  
for(int i=0; i<3; i++) {
  digitalWrite(LED_PIN, HIGH);
  delay(600);
  digitalWrite(LED_PIN, LOW);
  delay(200);
}

// S: 3 parpadeos cortos
for(int i=0; i<3; i++) {
  digitalWrite(LED_PIN, HIGH);
  delay(200);
  digitalWrite(LED_PIN, LOW);
  delay(200);
}

delay(1000); // Pausa entre repeticiones
```

## Solución de problemas

- **LED no enciende**: Verifica conexiones y polaridad
- **LED muy tenue**: Agrega resistencia limitadora (220Ω - 1kΩ)
- **Sin resistencia**: Usa los LEDs integrados en la tarjeta
