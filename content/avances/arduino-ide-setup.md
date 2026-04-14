+++
title = 'Arduino IDE'
date = 2026-04-13T14:00:00-05:00
draft = false
tarjeta = 'Plataformas'
estado = 'Completo'
author = 'Equipo DevLab'
tipos = ['documentacion-general']
modulos = ['general']
resumen = 'Configura Arduino IDE para usar las tarjetas de desarrollo UNIT Electronics.'
como_funciona = [
  'Descarga e instala Arduino IDE desde el sitio oficial.',
  'Agrega el URL del gestor de tarjetas de UNIT Electronics.',
  'Instala el paquete de tarjetas desde el Gestor de Tarjetas.',
  'Selecciona tu tarjeta y puerto de comunicación.'
]
respuesta_rapida = [
  '¿No detecta la tarjeta? Verifica que los drivers USB estén instalados.',
  '¿Error al compilar? Asegúrate de tener la versión correcta del paquete de tarjetas.'
]
retro = 'Arduino IDE es la plataforma más popular para programar microcontroladores de manera sencilla.'
fotos = ['images/devlab/ide-a6b6a068e29663bfde50923fc52fb982.webp']
tags = ['arduino', 'ide', 'plataformas', 'configuracion']
destacado = true
+++

Guía completa para configurar Arduino IDE y comenzar a programar las tarjetas de desarrollo UNIT Electronics. Aprende a instalar las herramientas necesarias y configurar tu entorno de desarrollo.

## Requisitos

- Arduino IDE 2.0 o superior
- Conexión a internet
- Cable USB apropiado

## Instalación paso a paso

1. **Descarga Arduino IDE** desde [arduino.cc](https://www.arduino.cc/en/software)
2. **Instala el software** siguiendo las instrucciones para tu sistema operativo
3. **Abre Arduino IDE** y ve a Archivo > Preferencias
4. **Agrega el URL** del gestor de tarjetas en "URLs Adicionales de Gestor de Tarjetas"
5. **Abre el Gestor de Tarjetas** desde Herramientas > Placa > Gestor de Tarjetas
6. **Busca e instala** el paquete de tarjetas UNIT Electronics

## Configuración

Una vez instalado el paquete:
- Selecciona tu tarjeta en Herramientas > Placa
- Selecciona el puerto COM en Herramientas > Puerto
- ¡Listo para programar!
