# ESP32-ATLAS
   Diseño en desarrollo para el ESP32 DEV KIT V1 usado de forma básica con sus  25 señales, dispone de un VGA64.
   
   No podíamos dejar pasar la oportunidad de alojar el ESP32 en el interior de la I/O BOARD ATLAS, estando muchos años en el mercado, el ESP32 sigue siendo un producto puntero.

   Es importantísimo tener su Software en ATLAS, y cubre toda la placa con sus conectores disponibles.

señales ATLAS| aclaración en ESP32 DEV KIT 1 | numero pines
| :--- | ---: | :---:
Señal VGA64 | Casi todos los cores de ESP32 usan 64colores esquema 222+HS+VS | 8
Señal JOYSTICK DB9 | En preparación ATARI-PADDLE-ATLAS  | 6
Señal SD SPI | las señales QPI se usan en modo SPI| 4
Señal PS2 TECLADO  | Protocolo PS/2 | 2
Señal PS2 RATÓN | Protocolo PS/2 | 2
Señal Sonido Estereo | sonido delta sigma_(12bits) o un pwm_(10bits)| 2
Señal Transmisión y Recepcion | RX TX sin gestión de flujo| 2
Señal EAR | Señal de entrada | 1

   En este diseño son ecesarias 27 señales, pero el ESP32 DEV KIT V1 dispone de 25 patillas/señales de las cuales 4 son sólo de entrada, ahí conectamos el mando de juegos.

   Usamos un esquema de bitluni para el recolocador:
   http://www.fabglib.org/conf_v_g_a.html
   
![PINEADO](https://github.com/AtlasFPGA/ESP32-ATLAS/blob/main/FOTOS/ESP32-DOIT-DEV-KIT-v1-pinout-mischianti.png)

##   Nota importante fijarse en los 4 pines que sólo permiten señales de entrada corresponden al GPIO36, GPIO39, GPIO34, Y GPIO35.

---

señales ATLAS | Patillaje izquierda | patilaje derecha | señales ATLAS
| ---: | ---: | ---: | :---: 
NC| ENABLE | GPIO23| HSYNC
JOY_UP| GPIO36 | GPIO22| RED[1]
JOY_DOWN| GPIO39 | GPIO01 | JOY_P1
JOY_RIGHT| GPIO34 | GPIO03 | JOY_P2
JOY_LEFT| GPIO35 | GPIO21| RED[0]
KB_DATA| GPIO32 | GPIO19 | GREEN[1]
KB_CLK| GPIO33 | GPIO18 | GREEN[0]
AUDIO_R| GPIO25 | GPIO05 | BLUE[1]
MOUSE_CLK| GPIO26 |GPIO17 | SD_MOSI
MOUSE_DATA| GPIO27 | GPIO16 | SD_MISO
SD_CLK| GPIO14 | GPIO04 | BLUE[0]
AUDIO_L| GPIO12 | GPIO02 | EAR
SD_CS| GPIO13 | GPIO15 | VSYNC
GND| GND | GND| GND
VIN| VIN | +3V3| +3V3

---
# ASIGNACIÓN ACTUAL BUSCANDO TENER I2C EN EL PUERTO DB9, ASÍ COMO SUS 4 DIRECCIONES DEL JOYSTICK EN PINES DE SÓLO ENTRADA:
# ESTA VERSIÓN TIENE EN CUENTA LOS CONVERSORES DE DIGITAL A ANOLÓGICO DE 8BITS POR CANAL Y LOS COLOCA EN LA SALIDA DE AUDIO.

los GPIO asociados a la salida de los dac de 8bit cada uno son, el GPIO25 y GPIO26.

Esta tercera iteración será la configuración final para el recolocador. 

señales ATLAS | Patillaje izquierda | patilaje derecha | señales ATLAS
| ---: | ---: | ---: | :---: 
ENABLE| ENABLE | GPIO23| T14_SD_MOSI
J14_JOY_UP| GPIO36 | GPIO22| K15_JOY_FIRE1
R1_JOY_DOWN| GPIO39 | GPIO01 | MOUSE_DAT
N16_JOY_RIGHT| GPIO34 | GPIO03 | MOUSE_CLK
T15_JOY_LEFT| GPIO35 | GPIO21| K16_JOY_FIRE2
TMDS_2_P| GPIO32 | GPIO19 | R13_SD_MISO
TMDS_2_N| GPIO33 | GPIO18 | T13_SD_CLK
R11_RIGHT| GPIO25 | GPIO05 | R12_SD_CS
T12_LEFT| GPIO26 |GPIO17 | TMDS_O_P
TMDS_1_P| GPIO27 | GPIO16 | TMDSC_N
K2_KDAT| GPIO14 | GPIO04 | TMDS_1_N
L2_KCLK| GPIO12 | GPIO02 | EAR
TMDS_0_N| GPIO13 | GPIO15 | TMDSC_P
GND| GND | GND| GND
+5V| VIN | +3V3| +3V3

---
# TABLA EQUIVALENCIAS SALIDA CONECTOR DE VÍDEO DIGITAL
Nomenclatura DVI | Pares diferenciales | VGA64 | SCART128
| ---: | ---: | ---: | :---: 
TMDS[0]|CLK- | HS | CSYNC
TMDS[1]|CLK+ | VS | G[0]
TMDS[2]|0-   | BLUE[0] | BLUE[0]
TMDS[3]|0+   | BLUE[1] | BLUE[1]
TMDS[4]|1-   | GREEN[0] | G[1]
TMDS[5]|1+   | GREEN[1] | G[2]
TMDS[6]|2-   | RED[0] | RED[0] 
TMDS[7]|2+   | RED[1] | RED[1] 

---





   Esquema de referencia:
   
![Esquemático de referencia FABGL](https://github.com/AtlasFPGA/ESP32-ATLAS/blob/main/FOTOS/fabglcircuit.gif)
   
   
---

   No hay lugar para Las Señales de RX y TX como GPIOS, se tendrá que acceder desde el USB.

   Lo que hace un total de 25 pines GPIO, para aumentar el diseño, esta el DB9 con 6 pines en PULL UP (Consultar esquema ATLAS-MINI), permite ampliarse el modelo existente mediante integrados I2C, con un buses de entrada salida en el bus DB9, tener en cuenta de las otras 4 señales, obligatoriamente tiene que ser señales de entrada.
   
   Consulta del esquema ATLAS MINI "La Roja" con licencia CERN OHL V2 STRICT:
   
   https://github.com/AtlasFPGA/BASECARRIERBOARDATLAS
   
   Consulta del esquema en easyeda:
   
   https://oshwlab.com/subcritical/carrier_io_board_atlas_mini_copy
   

![ESP32-DOIT-DEVKIT-V1](https://github.com/AtlasFPGA/ESP32-ATLAS/blob/main/FOTOS/ESP32-DOIT-DEVKIT-V1-Board-Pinout-30-GPIOs-2.jpg)

## Las características del Esp32:

Los lenguajes de programación más usados para NodeMCU ESP32, son Arduino y MicroPython. Ambos son de código abierto y cuentan con herramientas de desarrollo gratuitas.
 
## Especificaciones:

• Placa: ESP32 DEVKIT V1 (Espressif)

• SoM (System on module): ESP-WROOM-32 (Espressif)

• SoC (System on chip): ESP32 (ESP32-D0WDQ6)

• Procesador: Tensilica Xtensa Dual-Core 32-bit LX6.

• Frecuencia del reloj: 160 a 240 Mhz.

• Desempeño: Hasta 600 DMIPS

• Memoria:

448 KByte ROM

520 KByte SRAM

16 KByte SRAM in RTC (Real Time Clock)

QSPI Flash/SRAM, 4 MBytes

• Número total de pines GPIO: 25

• WiFi con antena integrada: estándar 802.11 b/g/n. Velocidad de 150.0 Mbps

• Bluetooth: BLE (Bluetooth Low Energy) y Bluetooth Classic

• Interfaz USB-Serial CP2102 on board.

• Modo de funcionamiento Low Power.

• Peripheral Input/Output (25 pines GPIO)

• ADCs (Convertidores Analógico Digital) de 12 bits

• DACs (Convertidores Digital Analógico) de 8 bits

• I²C (Inter-Integrated Circuit)

• UART (Universal Asynchronous Receiver/Transmitter)

• SPI (Serial Peripheral Interface)

• I²S (Integrated Interchip Sound)

• RMII (Reduced Media-Independent Interface)

• PWM (Pulse-Width Modulation).

• Regulador de 3.3 volts on board.

• Voltaje de Alimentación (conector MicroUSB): 5v, 0.5A

• Niveles de voltaje de entradas/salidas: 3.3v

• Corriente máxima en salidas: IOH=40ma (source), IOL=28ma (sink).

- Dimensiones: 51.5 mm x 28.3 mm x 12 mm

- Distancia entre pines: estándar 2.54 mm

- Distancia entre ambas filas de pines: 26.0 mm

- Número total de pines: 30

   Fuente de características y mapeado de pines del microcontrolador de Espressif ESP32.

https://www.puntoflotante.net/NODEMCU-ESP32-DEVKIT-V1-STARTER-KIT.htm

##   En total ATLAS pone a dispocisión con el recolocador ESP32-ATLAS unas 25 señales, completando todas las señales que le puede brindar la I/O BOARD ATLAS: 

Si   hubiera una linea a mayores disponible sería muy interesante que actuara automáticamente un CS_PADDLE sobre el ATARI-PADDLE-ATLAS.

---

![ESP32-ATLAS-AKA-NEPTUNE](https://github.com/AtlasFPGA/ESP32-ATLAS/blob/main/FOTOS/ESP32-ATLAS-AKA-NEPTUNE.jpg)

---

![ESP32-ATLAS-PINES-MKR-Y-PMOD](https://github.com/AtlasFPGA/ESP32-ATLAS/blob/main/FOTOS/ESP32-ATLAS-PINES-MKR-Y-PMOD.jpg)

---

![ESP32-ATLAS-WITH-VGA64](https://github.com/AtlasFPGA/ESP32-ATLAS/blob/main/FOTOS/ESP32-ATLAS-WITH-VGA64.jpg)

---

![VERIFICACIÓN FUNCIONAMIENTO](https://github.com/AtlasFPGA/ESP32-ATLAS/blob/main/FOTOS/VGA64_IO_BOARD_ATLAS_ESP32_DEV_KIT_V1_P1000386.JPG)
