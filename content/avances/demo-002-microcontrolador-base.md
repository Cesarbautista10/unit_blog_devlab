+++
title = 'Demo 002 - Base de microcontrolador para pruebas'
date = 2026-04-13T11:00:00-05:00
draft = false
tarjeta = 'DEMO-002'
estado = 'Validado'
author = 'Julio'
tipos = ['microcontroladores']
modulos = ['esp32']
resumen = 'Se documento el arranque base del microcontrolador y su mapa de pines.'
como_funciona = [
  'Se inicializa reloj y GPIO principales.',
  'Se valida comunicacion serial a 115200.',
  'Se corre test rapido de pines de entrada y salida.'
]
respuesta_rapida = [
  'Pinout rapido: consulta la tabla de GPIO en esta publicacion.',
  'Para flashear usa esptool con baudrate de 460800.'
]
retro = 'Buena estabilidad en laboratorio; pendiente validar ruido en campo.'
fotos = ['/images/demo-002/pinout.jpg']
tags = ['microcontrolador', 'esp32', 'documentacion']
+++

Este documento sirve como referencia base para soporte tecnico y puesta en marcha.
