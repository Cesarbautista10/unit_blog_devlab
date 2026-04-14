+++
title = 'PWM - Modulación por Ancho de Pulso'
date = 2026-04-13T14:25:00-05:00
draft = false
tarjeta = 'Ejemplos'
estado = 'Completo'
author = 'Equipo DevLab'
tipos = ['firmware']
modulos = ['esp32', 'general']
resumen = 'Aprende a usar la Modulación por ancho de pulso (PWM) para controlar el brillo de un LED en diferentes plataformas y microcontroladores usando Arduino IDE, MicroPython y SDCC.'
como_funciona = [
  'Configura un pin con capacidad PWM.',
  'Define la frecuencia y resolución del PWM.',
  'Ajusta el ciclo de trabajo (duty cycle) de 0% a 100%.',
  'El LED varía su brillo según el ciclo de trabajo.'
]
respuesta_rapida = [
  '¿LED no varía el brillo? Verifica que el pin tenga capacidad PWM.',
  '¿Parpadeo visible? Aumenta la frecuencia PWM a 1kHz o más.'
]
retro = 'PWM es fundamental para controlar motores, LEDs y generar señales analógicas.'
fotos = ['https://unit-electronics.github.io/CH552_Curso_introductorio/docs/9-controlador_pwm/images/pwm.gif']
tags = ['pwm', 'analog', 'led', 'ejemplos']
destacado = false
+++

PWM (Pulse Width Modulation) permite simular señales analógicas usando señales digitales. Perfecto para controlar el brillo de LEDs, velocidad de motores, y más.

## Arduino IDE (ESP32)

```cpp
#define LED_PIN 2
#define PWM_CHANNEL 0
#define PWM_FREQ 5000
#define PWM_RESOLUTION 8

void setup() {
  // Configurar canal PWM
  ledcSetup(PWM_CHANNEL, PWM_FREQ, PWM_RESOLUTION);
  ledcAttachPin(LED_PIN, PWM_CHANNEL);
}

void loop() {
  // Aumentar brillo gradualmente
  for(int duty = 0; duty <= 255; duty++) {
    ledcWrite(PWM_CHANNEL, duty);
    delay(10);
  }
  
  // Disminuir brillo gradualmente
  for(int duty = 255; duty >= 0; duty--) {
    ledcWrite(PWM_CHANNEL, duty);
    delay(10);
  }
}
```

## Arduino IDE (AVR - Arduino UNO/Nano)

```cpp
#define LED_PIN 9  // Debe ser pin con PWM (3, 5, 6, 9, 10, 11)

void setup() {
  pinMode(LED_PIN, OUTPUT);
}

void loop() {
  // Aumentar brillo (0-255)
  for(int brightness = 0; brightness <= 255; brightness++) {
    analogWrite(LED_PIN, brightness);
    delay(10);
  }
  
  // Disminuir brillo
  for(int brightness = 255; brightness >= 0; brightness--) {
    analogWrite(LED_PIN, brightness);
    delay(10);
  }
}
```

## MicroPython (ESP32)

```python
from machine import Pin, PWM
import time

# Configurar PWM (frecuencia 1kHz)
led = PWM(Pin(2), freq=1000)

while True:
    # Aumentar brillo (0-1023 en ESP32)
    for duty in range(0, 1024, 10):
        led.duty(duty)
        time.sleep_ms(10)
    
    # Disminuir brillo
    for duty in range(1023, -1, -10):
        led.duty(duty)
        time.sleep_ms(10)
```

## Conceptos clave

### Ciclo de trabajo (Duty Cycle)
- **0%**: LED apagado completamente
- **50%**: LED al 50% de brillo
- **100%**: LED al máximo brillo

### Frecuencia PWM
- **Baja (< 100Hz)**: Puede verse parpadeo
- **Media (1kHz)**: Ideal para LEDs
- **Alta (20kHz+)**: Necesaria para motores silenciosos

### Resolución
- **8 bits**: 0-255 (256 niveles)
- **10 bits**: 0-1023 (1024 niveles)
- **16 bits**: 0-65535 (65536 niveles)

## Aplicaciones

- 💡 Control de brillo de LEDs
- 🚗 Control de velocidad de motores DC
- 🔊 Generación de tonos (buzzer)
- 📡 Generación de señales analógicas
- 🎮 Control de servomotores

## Ejemplo avanzado: Control RGB

```cpp
#define PIN_R 2
#define PIN_G 4
#define PIN_B 5

void setup() {
  ledcSetup(0, 5000, 8);
  ledcSetup(1, 5000, 8);
  ledcSetup(2, 5000, 8);
  
  ledcAttachPin(PIN_R, 0);
  ledcAttachPin(PIN_G, 1);
  ledcAttachPin(PIN_B, 2);
}

void setColor(int r, int g, int b) {
  ledcWrite(0, r);
  ledcWrite(1, g);
  ledcWrite(2, b);
}

void loop() {
  setColor(255, 0, 0);   // Rojo
  delay(1000);
  setColor(0, 255, 0);   // Verde
  delay(1000);
  setColor(0, 0, 255);   // Azul
  delay(1000);
  setColor(255, 255, 0); // Amarillo
  delay(1000);
}
```
