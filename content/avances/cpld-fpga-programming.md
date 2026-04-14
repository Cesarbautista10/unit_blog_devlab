+++
title = 'CPLD/FPGA - Programación Intel MAX II'
date = 2026-04-13T15:25:00-05:00
draft = false
tarjeta = 'Firmware'
estado = 'Completo'
author = 'Equipo DevLab'
tipos = ['firmware']
modulos = ['general']
resumen = 'Programa dispositivos CPLD Intel MAX II usando el CH552 en modo JTAG para diseños de lógica digital programable.'
como_funciona = [
  'Carga firmware JTAG en el CH552.',
  'Conecta mediante interfaz JTAG al CPLD.',
  'Diseña tu lógica en Quartus Prime Lite.',
  'Programa el CPLD con urjtag o Quartus.'
]
respuesta_rapida = [
  '¿No detecta el CPLD? Verifica conexiones JTAG (TDI, TDO, TCK, TMS).',
  '¿Error de ID? El chip puede estar en blanco, es normal.',
  '¿Quartus no compila? Verifica que instalaste la familia MAX II.'
]
retro = 'Los CPLD permiten crear hardware digital customizado (contadores, decodificadores, máquinas de estado) programables.'
fotos = ['images/cpld/AR3962-Altera-MAX-II-EPM240-CPLD-2.jpg']
tags = ['cpld', 'fpga', 'max-ii', 'jtag', 'ch552', 'quartus']
destacado = false
+++

Los **CPLD** (Complex Programmable Logic Device) permiten implementar lógica digital customizada sin fabricar un chip ASIC.

## Intel MAX II EPM240

- 🔧 **Logic Elements**: 240 LEs
- 💾 **Flash**: 8192 bits RAM integrada
- 📌 **GPIO**: Hasta 80 pines I/O
- ⚡ **Velocidad**: Hasta 304MHz
- 🔌 **Voltaje**: 3.3V / 2.5V / 1.8V
- 💰 **No-volátil**: Configuración permanente

## Conexiones JTAG

```
CH552 (JTAG Mode)    MAX II EPM240
══════════════════   ═══════════════
TCK          ────→   TCK
TMS          ────→   TMS
TDI          ────→   TDI
TDO          ────←   TDO
GND          ────→   GND
3.3V         ────→   VCCIO
```

## Instalar Quartus Prime Lite

1. Descarga [Quartus Prime Lite](https://www.intel.com/content/www/us/en/software-kit/825278/intel-quartus-prime-lite-edition-design-software-version-23-1-1-for-linux.html) (gratis)
2. Instala componente MAX II/MAX V
3. Crea nuevo proyecto → MAX II → EPM240

## Ejemplo: Contador 4-bit

```verilog
module contador4bit (
    input wire clk,
    input wire reset,
    output reg [3:0] count
);

always @(posedge clk or posedge reset) begin
    if (reset)
        count <= 4'b0000;
    else
        count <= count + 1'b1;
end

endmodule
```

## Ejemplo: Decodificador 7 segmentos

```verilog
module decodificador7seg (
    input wire [3:0] bcd,
    output reg [6:0] seg
);

always @(*) begin
    case (bcd)
        4'h0: seg = 7'b1111110;  // 0
        4'h1: seg = 7'b0110000;  // 1
        4'h2: seg = 7'b1101101;  // 2
        4'h3: seg = 7'b1111001;  // 3
        4'h4: seg = 7'b0110011;  // 4
        4'h5: seg = 7'b1011011;  // 5
        4'h6: seg = 7'b1011111;  // 6
        4'h7: seg = 7'b1110000;  // 7
        4'h8: seg = 7'b1111111;  // 8
        4'h9: seg = 7'b1111011;  // 9
        default: seg = 7'b0000000;
    endcase
end

endmodule
```

## Ejemplo: Máquina de estados (FSM)

```verilog
module fsm_semaforo (
    input wire clk,
    input wire reset,
    output reg [2:0] luces  // [R, Y, G]
);

reg [1:0] estado;
parameter VERDE = 2'b00,
          AMARILLO = 2'b01,
          ROJO = 2'b10;

always @(posedge clk or posedge reset) begin
    if (reset)
        estado <= ROJO;
    else begin
        case (estado)
            VERDE:    estado <= AMARILLO;
            AMARILLO: estado <= ROJO;
            ROJO:     estado <= VERDE;
            default:  estado <= ROJO;
        endcase
    end
end

always @(*) begin
    case (estado)
        VERDE:    luces = 3'b001;
        AMARILLO: luces = 3'b010;
        ROJO:     luces = 3'b100;
        default:  luces = 3'b000;
    endcase
end

endmodule
```

## Programar con Quartus Programmer

1. Compila tu diseño (Processing → Start Compilation)
2. Abre Programmer (Tools → Programmer)
3. Agrega archivo `.pof` o `.sof`
4. Selecciona hardware: USB-Blaster (si detecta CH552 JTAG)
5. Presiona **Start** para programar

## Programar con urjtag (Linux)

```bash
# Instalar urjtag
sudo apt install urjtag

# Conectar
jtag
> cable ft2232
> detect
> pld load blink.svf

# Salir
> quit
```

## VHDL vs Verilog

### Verilog (más común en tutoriales)
```verilog
module blink (
    input clk,
    output reg led
);
    reg [24:0] counter;
    
    always @(posedge clk) begin
        counter <= counter + 1;
        led <= counter[24];
    end
endmodule
```

### VHDL (más verboso, industria)
```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;

entity blink is
    Port ( clk : in  STD_LOGIC;
           led : out STD_LOGIC);
end blink;

architecture Behavioral of blink is
    signal counter : unsigned(24 downto 0);
begin
    process(clk)
    begin
        if rising_edge(clk) then
            counter <= counter + 1;
            led <= counter(24);
        end if;
    end process;
end Behavioral;
```

## Simulación con ModelSim

```bash
# Compilar testbench
vlog contador4bit.v
vlog contador4bit_tb.v

# Simular
vsim contador4bit_tb

# Ver formas de onda
add wave *
run 1000ns
```

## Ejemplo testbench

```verilog
`timescale 1ns/1ps

module contador4bit_tb;
    reg clk, reset;
    wire [3:0] count;
    
    contador4bit uut (
        .clk(clk),
        .reset(reset),
        .count(count)
    );
    
    initial begin
        clk = 0;
        forever #5 clk = ~clk;  // 100MHz clock
    end
    
    initial begin
        reset = 1;
        #20 reset = 0;
        #500 $finish;
    end
    
    initial begin
        $monitor("Time=%0t Reset=%b Count=%d", $time, reset, count);
    end
endmodule
```

## Ventajas CPLD vs FPGA

### CPLD (MAX II)
- ✅ **No-volátil**: Configuración permanente
- ✅ **Instant-on**: Sin tiempo de carga
- ✅ **Bajo consumo**: Ideal para batería
- ✅ **Económico**: <$5 USD
- ✅ **Simple**: Menos complejidad que FPGA

### FPGA (más avanzado)
- Más logic elements (10K+)
- RAM interna grande
- Multiplicadores hardware
- PLLs integrados
- Mayor costo y consumo

## Aplicaciones CPLD

- 🔌 **GPIO expander**: Más pines I/O
- 🎮 **Controlador customizado**: Protocolos propietarios
- 📊 **Generador de señales**: PWM multi-canal
- 🔐 **Glue logic**: Interfaces entre chips
- ⏱️ **Contador hardware**: Alta velocidad
