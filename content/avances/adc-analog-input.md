+++
title = 'ADC - Entradas Analógicas'
date = 2026-04-13T14:35:00-05:00
draft = false
tarjeta = 'Ejemplos'
estado = 'Completo'
author = 'Equipo DevLab'
tipos = ['firmware']
modulos = ['esp32', 'general']
resumen = 'Aprende a leer entradas analógicas y convertirlas a digitales en diferentes plataformas y microcontroladores usando Arduino IDE, MicroPython y SDCC.'
como_funciona = [
  'Configura un pin con capacidad ADC como entrada analógica.',
  'Lee el valor analógico (voltaje).',
  'El ADC convierte el voltaje a un valor digital.',
  'Procesa el valor en tu aplicación.'
]
respuesta_rapida = [
  '¿Lecturas ruidosas? Promedia múltiples lecturas.',
  '¿Valores incorrectos? Verifica el voltaje de referencia del ADC.'
]
retro = 'El ADC permite leer sensores analógicos como potenciómetros, foto-resistencias y sensores de temperatura.'
fotos = ['https://unit-electronics.github.io/CH552_Curso_introductorio/docs/8-entradas_analogicas/images/cocket_nova.gif']
tags = ['adc', 'analog', 'input', 'ejemplos', 'sensor']
destacado = false
+++

El ADC (Analog to Digital Converter) convierte señales analógicas (voltaje variable) en valores digitales que el microcontrolador puede procesar.

## Arduino IDE (ESP32)

```cpp
#define POT_PIN 34  // GPIO34 tiene ADC

void setup() {
  Serial.begin(115200);
  analogReadResolution(12);  // 12 bits = 0-4095
}

void loop() {
  int adcValue = analogRead(POT_PIN);
  float voltage = (adcValue / 4095.0) * 3.3;
  
  Serial.print("ADC: ");
  Serial.print(adcValue);
  Serial.print(" | Voltaje: ");
  Serial.print(voltage);
  Serial.println("V");
  
  delay(500);
}
```

## MicroPython (ESP32)

```python
from machine import ADC, Pin
import time

adc = ADC(Pin(34))
adc.atten(ADC.ATTN_11DB)  # Rango completo 0-3.3V

while True:
    adc_value = adc.read()  # 0-4095 (12 bits)
    voltage = (adc_value / 4095) * 3.3
    
    print(f"ADC: {adc_value} | Voltaje: {voltage:.2f}V")
    time.sleep(0.5)
```

## Promediado para reducir ruido

```cpp
#define NUM_READINGS 10

float readADCAverage(int pin) {
  long sum = 0;
  for(int i = 0; i < NUM_READINGS; i++) {
    sum += analogRead(pin);
    delay(10);
  }
  return sum / (float)NUM_READINGS;
}

void loop() {
  float avgValue = readADCAverage(POT_PIN);
  float voltage = (avgValue / 4095.0) * 3.3;
  
  Serial.print("Promedio: ");
  Serial.print(avgValue);
  Serial.print(" | Voltaje: ");
  Serial.print(voltage);
  Serial.println("V");
  
  delay(1000);
}
```

## Calibración del ADC (ESP32)

El ESP32 puede tener variaciones en el ADC. Para mejor precisión:

```cpp
#include <driver/adc.h>
#include <esp_adc_cal.h>

esp_adc_cal_characteristics_t adc_chars;

void setup() {
  Serial.begin(115200);
  
  // Caracterizar ADC para mejor precisión
  esp_adc_cal_characterize(ADC_UNIT_1, ADC_ATTEN_DB_11, ADC_WIDTH_BIT_12, 1100, &adc_chars);
  
  analogSetAttenuation(ADC_11db);
}

void loop() {
  uint32_t voltage = esp_adc_cal_raw_to_voltage(analogRead(34), &adc_chars);
  Serial.printf("Voltaje calibrado: %d mV\n", voltage);
  delay(1000);
}
```

## Aplicaciones comunes

- 📏 Lectura de potenciómetros
- 🌡️ Sensores de temperatura analógicos (LM35, TMP36)
- 💡 Foto-resistencias (LDR)
- 🎤 Micrófonos analógicos
- 🔋 Medición de voltaje de baterías
