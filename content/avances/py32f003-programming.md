+++
title = 'Programación PY32F003 con Arduino IDE'
date = 2026-04-13T15:00:00-05:00
draft = false
tarjeta = 'Hardware'
estado = 'Completo'
author = 'Equipo DevLab'
tipos = ['microcontroladores']
modulos = ['general']
resumen = 'Aprende a programar el PY32F003L24D6TR usando Arduino IDE y el programador CH552 en modo CMSIS-DAP.'
como_funciona = [
  'Instala el soporte PY32 en Arduino IDE.',
  'Conecta el CH552 en modo CMSIS-DAP al PY32.',
  'Selecciona la placa PY32F003 en Arduino.',
  'Programa y depura tu código.'
]
respuesta_rapida = [
  '¿No reconoce la placa? Verifica que el CH552 esté en modo CMSIS-DAP.',
  '¿Error al cargar? Verifica las conexiones SWD (SWDIO, SWDCLK).',
  '¿No compila? Instala el paquete PY32 desde el gestor de placas.'
]
retro = 'El PY32F003 es un ARM Cortex-M0+ ultra económico compatible con Arduino IDE mediante el programador CH552.'
fotos = ['images/py32/PY32F003L24D6TR-ARM-32-bits-UNIT-DevLab-AR-4354-2.webp']
tags = ['py32', 'arduino', 'cmsis-dap', 'programacion', 'arm']
destacado = true
+++

El **PY32F003** es un microcontrolador ARM Cortex-M0+ de Puya Semiconductor, ultra económico y compatible con Arduino IDE.

## Características PY32F003L24D6TR

- 🧠 **CPU**: ARM Cortex-M0+ @ 24MHz
- 💾 **Flash**: 20KB
- 🧮 **RAM**: 3KB
- 📌 **GPIO**: 13 pines
- 🔌 **Periféricos**: UART, I2C, SPI, ADC (12-bit)
- 💰 **Precio**: ~$0.10 USD

## Instalación Arduino IDE

1. Abre Arduino IDE
2. Ve a **Archivo → Preferencias**
3. Agrega esta URL en "Gestor de URLs Adicionales":

```
https://github.com/py32duino/platform-py32f0.git
```

4. Ve a **Herramientas → Placa → Gestor de placas**
5. Busca "PY32" e instala el paquete

## Conexiones CH552 → PY32F003

```
CH552 (CMSIS-DAP)    PY32F003
═════════════════    ════════
SWDIO         ────→  SWDIO (PA13)
SWDCLK        ────→  SWDCLK (PA14)
GND           ────→  GND
3.3V          ────→  VDD
```

## Ejemplo Blink (Arduino)

```cpp
// PY32F003 Blink LED
#define LED_PIN PA0

void setup() {
  pinMode(LED_PIN, OUTPUT);
}

void loop() {
  digitalWrite(LED_PIN, HIGH);
  delay(1000);
  digitalWrite(LED_PIN, LOW);
  delay(1000);
}
```

## Ejemplo Serial

```cpp
void setup() {
  Serial.begin(115200);
}

void loop() {
  Serial.println("Hola desde PY32F003!");
  delay(1000);
}
```

## Ejemplo ADC

```cpp
#define ADC_PIN PA4

void setup() {
  Serial.begin(115200);
  pinMode(ADC_PIN, INPUT_ANALOG);
}

void loop() {
  int adcValue = analogRead(ADC_PIN);
  float voltage = (adcValue / 4095.0) * 3.3;
  
  Serial.print("ADC: ");
  Serial.print(adcValue);
  Serial.print(" | Voltaje: ");
  Serial.print(voltage);
  Serial.println("V");
  
  delay(500);
}
```

## Configurar programador

En Arduino IDE:

- **Placa**: "PY32F003 (20KB Flash, 3KB RAM)"
- **Programador**: "CMSIS-DAP"
- **Puerto**: (selecciona el puerto del CH552)

## Cargar programa

1. Conecta el CH552 al PY32F003
2. Presiona **Ctrl+U** o **Sketch → Subir**
3. Espera la confirmación "Subida correcta"

## Depuración con PyOCD

```bash
# Instalar PyOCD con soporte PY32
pip install pyocd
pyocd pack install puya

# Depurar
pyocd gdbserver -t py32f003

# En otra terminal
arm-none-eabi-gdb firmware.elf
(gdb) target remote :3333
```

## Pinout PY32F003L24D6TR

```
        ┌─────────┐
 PA14 ──┤1  •  24├── VDD
 PA13 ──┤2     23├── VSS
 PA12 ──┤3     22├── PA11
 PA11 ──┤4     21├── PA10
 PA10 ──┤5     20├── PA9
 PA9  ──┤6     19├── PA8
 PA8  ──┤7     18├── PA7
 PA7  ──┤8     17├── PA6
 PA6  ──┤9     16├── PA5
 PA5  ──┤10    15├── PA4
 PA4  ──┤11    14├── PA3
 PA3  ──┤12    13├── PA2
        └─────────┘
```

## Proyectos ejemplo

- 💡 Control LED PWM
- 🌡️ Lectura de temperatura (sensor interno)
- 📡 Comunicación I2C con OLED
- ⚡ Bajo consumo (sleep modes)

## Ventajas del PY32F003

- ✅ **Muy económico**: <$0.10 USD
- ✅ **ARM Cortex-M0+**: Arquitectura moderna
- ✅ **Arduino compatible**: Fácil de programar
- ✅ **Bajo consumo**: Ideal para batería
- ✅ **CMSIS-DAP**: Programación y depuración estándar
