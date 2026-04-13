+++
title = 'Demo 003 - Producto sensor listo para integracion'
date = 2026-04-13T11:30:00-05:00
draft = false
tarjeta = 'DEMO-003'
estado = 'En pruebas'
author = 'Julio'
tipos = ['productos']
modulos = ['sensores']
resumen = 'Se integra el sensor en el flujo principal de producto para pruebas funcionales.'
como_funciona = [
  'El sensor envia lectura cada 500 ms.',
  'El firmware filtra ruido y calcula promedio movil.',
  'Se publica telemetria al panel de monitoreo.'
]
respuesta_rapida = [
  'Error de lectura: revisar conexion I2C y direccion del dispositivo.',
  'Lectura congelada: reiniciar el bus y limpiar buffer.'
]
retro = 'Usuarios pidieron un indicador visual de estado del sensor.'
fotos = ['/images/demo-003/integracion.jpg']
tags = ['producto', 'sensor', 'iot']
+++

Se habilito la ruta de diagnostico para responder dudas del equipo de soporte.
