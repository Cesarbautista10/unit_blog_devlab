+++
title = 'Demo 001 - Primer avance de funcionalidad'
date = 2026-04-13T10:00:00-05:00
draft = false
tarjeta = 'DEMO-001'
estado = 'En progreso'
author = 'Julio'
tipos = ['firmware']
modulos = ['esp32']
resumen = 'Se implemento la base del flujo y se valido una ejecucion inicial con el modulo de microfono PDM UNIT-MP34DT05TR-A sobre ESP32.'
como_funciona = [
  'Se inicia el modulo desde el menu principal.',
  'El sistema valida los datos de entrada y ejecuta el proceso.',
  'Se muestra el resultado junto con el estado de la tarjeta.'
]
respuesta_rapida = [
  'Reinicio rapido? Presiona reset del modulo por 2 segundos.',
  'No responde? Valida alimentacion 3.3V y vuelve a cargar firmware.'
]
retro = 'El equipo solicito simplificar la configuracion inicial y mejorar mensajes de error.'
fotos = [
  '/images/demo-001/UNIT-MP34DT05TR-A-Pinout.webp',
  '/images/demo-001/AR3631-UNIT-MP34DT05TR-A-Modulo-Microfono-PDM-2_1.webp'
]
tags = ['demo-001', 'avance', 'pdm', 'microfono']
destacado = true
+++

En este avance se completo la primera version funcional del flujo de captura de audio usando el microfono PDM **UNIT-MP34DT05TR-A** conectado al **ESP32**. Se valido la inicializacion del periferico y la lectura de muestras en tiempo real.

{{< note type="info" title="Modulo utilizado" >}}
El UNIT-MP34DT05TR-A es un microfono digital PDM de ST Microelectronics. Opera a 3.3V y se comunica con el ESP32 via los pines CLK y DATA.
{{< /note >}}

## Pinout del modulo

{{< diagram type="pdf"
  src="https://unit-electronics-mx.github.io/unit_devlab_ws28b20_1010_rgb_led_matrix_module/hardware/unit_pinout_v_1_0_0_ue00112_WS28B20_matrix_en.pdf"
  caption="Pinout WS2812B Matrix v1.0.0 — UE00112"
  height="580" >}}

## Fotos del modulo

{{< carousel id="demo-001-fotos" >}}
/images/demo-001/UNIT-MP34DT05TR-A-Pinout.webp|Vista del pinout del microfono PDM
/images/demo-001/AR3631-UNIT-MP34DT05TR-A-Modulo-Microfono-PDM-2_1.webp|Modulo fisico UNIT-MP34DT05TR-A
{{< /carousel >}}

## Codigo de inicializacion

```c
#include <driver/i2s.h>

#define PDM_CLK_PIN  26
#define PDM_DATA_PIN 22
#define SAMPLE_RATE  16000

void setup() {
  Serial.begin(115200);

  i2s_config_t cfg = {
    .mode = (i2s_mode_t)(I2S_MODE_MASTER | I2S_MODE_RX | I2S_MODE_PDM),
    .sample_rate = SAMPLE_RATE,
    .bits_per_sample = I2S_BITS_PER_SAMPLE_16BIT,
    .channel_format = I2S_CHANNEL_FMT_ONLY_LEFT,
    .communication_format = I2S_COMM_FORMAT_STAND_PCM_SHORT,
    .intr_alloc_flags = ESP_INTR_FLAG_LEVEL1,
    .dma_buf_count = 4,
    .dma_buf_len = 256,
  };

  i2s_pin_config_t pins = {
    .bck_io_num   = PDM_CLK_PIN,
    .ws_io_num    = PDM_CLK_PIN,
    .data_out_num = I2S_PIN_NO_CHANGE,
    .data_in_num  = PDM_DATA_PIN,
  };

  i2s_driver_install(I2S_NUM_0, &cfg, 0, NULL);
  i2s_set_pin(I2S_NUM_0, &pins);
  Serial.println("Microfono PDM listo");
}

void loop() {
  int16_t buffer[256];
  size_t bytes_read;
  i2s_read(I2S_NUM_0, buffer, sizeof(buffer), &bytes_read, portMAX_DELAY);
  // Procesar muestras...
}
```

{{< note type="warning" title="Advertencia de alimentacion" >}}
El modulo opera a **3.3V**. No conectar a 5V o se danara el MEMS. Verifica el nivel logico antes de encender.
{{< /note >}}

## Conexion con ESP32

| Pin modulo | Pin ESP32 | Descripcion         |
|------------|-----------|---------------------|
| VCC        | 3.3V      | Alimentacion        |
| GND        | GND       | Tierra              |
| CLK        | GPIO 26   | Reloj PDM           |
| DATA       | GPIO 22   | Datos de audio      |

{{< note type="success" title="Validado" >}}
La lectura de muestras a 16 kHz funciona correctamente. El buffer de 256 muestras no genera underrun en pruebas de 5 minutos continuos.
{{< /note >}}

## Pendientes

1. Mejorar mensajes de error en caso de fallo de inicializacion.
2. Agregar validaciones de borde para deteccion de silencio.
3. Registrar evidencia de pruebas con senales de audio reales.
