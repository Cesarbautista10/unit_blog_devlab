+++
title = 'Cocket Nova - SDK Docker'
date = 2026-04-13T14:55:00-05:00
draft = false
tarjeta = 'Software'
estado = 'Completo'
author = 'Equipo DevLab'
tipos = ['documentacion-general']
modulos = ['general']
resumen = 'Cocket Nova es un SDK basado en Docker que facilita la generación de archivos .bin para microcontroladores sin configurar toolchains complejas.'
como_funciona = [
  'Instala Docker en tu sistema.',
  'Descarga la imagen de Cocket Nova.',
  'Monta tu proyecto en el contenedor.',
  'Ejecuta el comando de compilación y obtén el .bin'
]
respuesta_rapida = [
  '¿No encuentra Docker? Instala Docker Desktop o docker.io.',
  '¿Errores de permisos? Agrega tu usuario al grupo docker.',
  '¿El .bin no se genera? Verifica los logs del contenedor.'
]
retro = 'Cocket Nova elimina la complejidad de configurar toolchains, ofreciendo un entorno de desarrollo listo para usar en un contenedor Docker.'
fotos = ['images/ch55x/AR4349-Cocket-Nova-Ch552-10-e1727911693175.jpg']
tags = ['cocket', 'docker', 'sdk', 'firmware', 'toolchain']
destacado = false
+++

**Cocket Nova** es un SDK dockerizado que proporciona un entorno de compilación completo para generar firmware sin instalar toolchains localmente.

## Instalación de Docker

### Linux
```bash
sudo apt install docker.io
sudo usermod -aG docker $USER
# Cerrar sesión y volver a iniciar
```

### Windows/Mac
Descarga e instala [Docker Desktop](https://www.docker.com/products/docker-desktop)

## Descargar imagen Cocket Nova

```bash
docker pull unitelectronics/cocket-nova:latest
```

## Compilar proyecto

```bash
# Estructura del proyecto
mi-proyecto/
├── src/
├── include/
└── Makefile

# Compilar usando Cocket Nova
docker run --rm -v $(pwd):/project unitelectronics/cocket-nova:latest make

# El archivo .bin se genera en la carpeta actual
```

## Ejemplo completo

```bash
# Crear proyecto
mkdir blink-project && cd blink-project

# Crear código fuente
cat > main.c << 'CODE'
#include <ch32v00x.h>

void delay(uint32_t ms) {
    for(uint32_t i = 0; i < ms * 1000; i++) {
        __NOP();
    }
}

int main(void) {
    GPIO_InitTypeDef GPIO_InitStructure;
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOD, ENABLE);
    
    GPIO_InitStructure.GPIO_Pin = GPIO_Pin_0;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOD, &GPIO_InitStructure);
    
    while(1) {
        GPIO_SetBits(GPIOD, GPIO_Pin_0);
        delay(500);
        GPIO_ResetBits(GPIOD, GPIO_Pin_0);
        delay(500);
    }
}
CODE

# Compilar
docker run --rm -v $(pwd):/project unitelectronics/cocket-nova:latest

# Flashear (requiere WCHISPTool)
wchisptool -f firmware.bin
```

## Ventajas de Cocket Nova

- ✅ **Sin instalación de toolchain**: Todo está en el contenedor
- ✅ **Reproducible**: Misma versión de compilador siempre
- ✅ **Multi-plataforma**: Funciona en Linux, Windows, Mac
- ✅ **Actualizable**: `docker pull` para nueva versión
- ✅ **Aislado**: No contamina el sistema host

## Toolchains incluidos

- ARM GCC (Cortex-M0/M0+/M3/M4)
- RISC-V GCC (CH32V, GD32V)
- SDCC (8051, CH552)
- AVR GCC (ATmega)

## Comandos útiles

```bash
# Ver versión de compiladores
docker run --rm unitelectronics/cocket-nova:latest arm-none-eabi-gcc --version

# Compilar con opciones
docker run --rm -v $(pwd):/project unitelectronics/cocket-nova:latest make OPTIMIZE=-Os

# Limpiar proyecto
docker run --rm -v $(pwd):/project unitelectronics/cocket-nova:latest make clean

# Ejecutar shell interactivo
docker run --rm -it -v $(pwd):/project unitelectronics/cocket-nova:latest bash
```

## Actualizar Cocket Nova

```bash
# Descargar última versión
docker pull unitelectronics/cocket-nova:latest

# Ver cambios
docker images unitelectronics/cocket-nova
```
