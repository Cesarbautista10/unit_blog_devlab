+++
title = 'GPIO - Entradas Digitales'
date = 2026-04-13T14:30:00-05:00
draft = false
tarjeta = 'Ejemplos'
estado = 'Completo'
author = 'Equipo DevLab'
tipos = ['firmware']
modulos = ['esp32', 'general']
resumen = 'Aprende a leer entradas digitales en diferentes plataformas y microcontroladores usando Arduino IDE, MicroPython y SDCC.'
como_funciona = [
  'Configura un pin GPIO como entrada.',
  'Opcionalmente activa resistencia pull-up o pull-down interna.',
  'Lee el estado del pin (HIGH/LOW).',
  'Procesa la lectura en tu lógica de programa.'
]
respuesta_rapida = [
  '¿Lecturas inestables? Activa resistencia pull-up o pull-down.',
  '¿Botón no responde? Verifica conexiones y agrega debounce por software.'
]
retro = 'Las entradas digitales son esenciales para leer sensores, botones y switches.'
fotos = ['https://unit-electronics.github.io/CH552_Curso_introductorio/docs/7-entradas_open_drain/images/input4.gif']
tags = ['gpio', 'digital', 'input', 'ejemplos', 'boton']
destacado = false
+++

Aprende a leer entradas digitales para detectar el estado de botones, switches y sensores digitales.

## Arduino IDE - Lectura de botón

```cpp
#define BUTTON_PIN 4
#define LED_PIN 2

void setup() {
  pinMode(BUTTON_PIN, INPUT_PULLUP);  // Entrada con pull-up
  pinMode(LED_PIN, OUTPUT);
  Serial.begin(115200);
}

void loop() {
  int buttonState = digitalRead(BUTTON_PIN);
  
  if (buttonState == LOW) {  // Botón presionado (pull-up)
    digitalWrite(LED_PIN, HIGH);
    Serial.println("Botón presionado");
  } else {
    digitalWrite(LED_PIN, LOW);
  }
  
  delay(50);  // Debounce simple
}
```

## MicroPython

```python
from machine import Pin
import time

button = Pin(4, Pin.IN, Pin.PULL_UP)
led = Pin(2, Pin.OUT)

while True:
    if button.value() == 0:  # Presionado
        led.value(1)
        print("Botón presionado")
    else:
        led.value(0)
    
    time.sleep_ms(50)
```

## Debounce por software

```cpp
#define BUTTON_PIN 4
unsigned long lastDebounceTime = 0;
unsigned long debounceDelay = 50;
int lastButtonState = HIGH;
int buttonState = HIGH;

void setup() {
  pinMode(BUTTON_PIN, INPUT_PULLUP);
  Serial.begin(115200);
}

void loop() {
  int reading = digitalRead(BUTTON_PIN);
  
  if (reading != lastButtonState) {
    lastDebounceTime = millis();
  }
  
  if ((millis() - lastDebounceTime) > debounceDelay) {
    if (reading != buttonState) {
      buttonState = reading;
      
      if (buttonState == LOW) {
        Serial.println("¡Botón presionado!");
      }
    }
  }
  
  lastButtonState = reading;
}
```

## Resistencias Pull-up/Pull-down

- **Pull-up**: Mantiene el pin en HIGH cuando no hay conexión. Lectura LOW cuando se presiona.
- **Pull-down**: Mantiene el pin en LOW cuando no hay conexión. Lectura HIGH cuando se presiona.

## Interrupciones

Para detectar cambios sin polling constante:

```cpp
volatile bool buttonPressed = false;

void IRAM_ATTR handleButtonPress() {
  buttonPressed = true;
}

void setup() {
  pinMode(BUTTON_PIN, INPUT_PULLUP);
  attachInterrupt(digitalPinToInterrupt(BUTTON_PIN), handleButtonPress, FALLING);
  Serial.begin(115200);
}

void loop() {
  if (buttonPressed) {
    Serial.println("¡Botón presionado por interrupción!");
    buttonPressed = false;
  }
  // Aquí puedes hacer otras tareas
}
```
