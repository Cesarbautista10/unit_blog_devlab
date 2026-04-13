+++
title = '{{ replace .File.ContentBaseName "-" " " | title }}'
date = '{{ .Date }}'
draft = true
tarjeta = 'DEMO-001'
estado = 'En progreso'
author = 'Equipo DevLab'
tipos = ['documentacion-general']
modulos = ['general']
resumen = 'Breve resumen del avance realizado.'
como_funciona = [
  'Paso 1: Describe la preparacion.',
  'Paso 2: Explica la ejecucion principal.',
  'Paso 3: Indica el resultado esperado.'
]
respuesta_rapida = [
  'Que es este modulo? Respuesta corta para soporte.',
  'Como reiniciar rapido? Paso breve para atencion inmediata.'
]
retro = 'Comentarios cortos de retroalimentacion del equipo o usuarios.'
fotos = [
  '/images/tu-foto-1.jpg',
  '/images/tu-foto-2.jpg'
]
tags = ['demo', 'avance', 'soporte']
+++

Escribe aqui el detalle del avance: decisiones, bloqueos, pendientes y proximo paso.

## Bloque de codigo

```js
function calcularResultado(valor) {
  return valor * 2;
}
```

## Carrusel en cualquier zona

Usa este shortcode donde quieras dentro del contenido:

{{< carousel id="demo-001-zona-a" >}}
/images/demo-001/paso-1.jpg|Vista general del modulo
/images/demo-001/paso-2.jpg|Detalle de conexion
{{< /carousel >}}

Tambien puedes insertar imagen suelta en otra zona:

![Evidencia puntual](/images/demo-001/paso-1.jpg)

## Notas / Callouts

{{< note type="info" title="Informacion" >}}
Texto informativo o contexto general del paso.
{{< /note >}}

{{< note type="tip" title="Consejo" >}}
Sugerencia practica para optimizar el proceso.
{{< /note >}}

{{< note type="warning" title="Advertencia" >}}
Revisa esta configuracion antes de continuar o podrias perder datos.
{{< /note >}}

{{< note type="danger" title="Peligro" >}}
No conectes alimentacion mientras el modulo esta en modo de escritura.
{{< /note >}}

{{< note type="success" title="Listo" >}}
Si llegaste aqui, el modulo esta correctamente configurado.
{{< /note >}}