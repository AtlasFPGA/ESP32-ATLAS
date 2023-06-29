# ESP32-ATLAS
   Diseño en desarrollo para el ESP32 DEV KIT V1 usado de forma básica con sus  25 señales,se necesita disponer de un VGA64.
   No podíamos dejar pasar la oportunidad de alojar el ESP32 en el interior de la I/O BOARD ATLAS,este microcontroladro lleva muchos años en el mercado,tant es así que el ESP32 sigue siendo un producto puntero.
   Es importantísimo tener su Software en ATLAS, y con la tercera iteración de correspondencia de pines cubre toda la placa I/O BOARD ATLAS.

La inicialización de FABGL con su configuración en la mayoría de las placas es la siguiente.
inicialización VGA FABGL:

VGAController.begin(GPIO_NUM_22, GPIO_NUM_21, GPIO_NUM_19, GPIO_NUM_18, GPIO_NUM_5, GPIO_NUM_4, GPIO_NUM_23, GPIO_NUM_15);

Leyendo el quema vemos que significado tiene de señales con sus nombres lógicos de los bits de color así como la frecuencia horizontal y vertical.

VGAController.begin(RED_1, RED_0 , GREEN_1, GREEN_0, BLUE_1, BLUE_0, H_SYNC, V_SYNC)

Inicialización ESP32 en ATLAS con el recolocador creado:

´´´
## VGAController.begin(GPIO_NUM_32, GPIO_NUM_33, GPIO_NUM_27, GPIO_NUM_4, GPIO_NUM_17, GPIO_NUM_13, GPIO_NUM_16, GPIO_NUM_15);
´´´



El recolocador de ATLAS tiene una colocación diferente.
---

Colocación del ESP32 en la placa I/O Board ATLAS Morada:
![Colocación del ESP32 en la placa I/O Board ATLAS Morada](https://github.com/AtlasFPGA/ESP32-ATLAS/blob/main/FOTOS_PLACA_REALES/Visi%C3%B3n%20de%20ESP32%20con%20modelo%20prototipo%20Morado%20P1050220.JPG)

---

Placa "La Roja" vista con el recolocador ESP32 desde el bus del sbc:
![Placa "La Roja" vista con el recolocador ESP32 desde el bus del sbc](https://github.com/AtlasFPGA/ESP32-ATLAS/blob/main/FOTOS_PLACA_REALES/Visi%C3%B3n%20de%20la%20placa%20con%20la%20roja%20B%20P1050235.JPG)

---

Ensamblado ESP32 visto desde los puertos ps2 y db9 norma atari:
![Ensamblado ESP32 visto desde los puertos ps2 y db9 norma atari](https://github.com/AtlasFPGA/ESP32-ATLAS/blob/main/FOTOS_PLACA_REALES/Visi%C3%B3n%20de%20la%20placa%20con%20la%20roja%20P1050232.JPG)

---

Visión de la placa recolocadora ESP32 superior:
![Visión de la placa recolocadora superior](https://github.com/AtlasFPGA/ESP32-ATLAS/blob/main/FOTOS_PLACA_REALES/Visi%C3%B3n%20de%20la%20placa%20recolocadora%20superior%20P1050208.JPG)

---
Visión de la placa recolocadora ESP32 infernior:
![](https://github.com/AtlasFPGA/ESP32-ATLAS/blob/main/FOTOS_PLACA_REALES/Visi%C3%B3n%20de%20la%20placa%20inferior%20P1050211.JPG)
   
---   

# Versión final sin amplificación de sonido:


[![Ver vídeo diseño recolocador](https://img.youtube.com/vi/FslLuDT2TB8/0.jpg)](https://www.youtube.com/watch?v=FslLuDT2TB8)


---   
 
   
# Vídeo del proceso de desarrollo del recolocador para ESP32 DEV KIT V1; así como mostrar el funcionamiento del VGA64 222+HS+VS de la placa I/O Board ATLAS:

[![Ver vídeo primera iteracion bitluni](https://img.youtube.com/vi/wdI3RePPbeQ/0.jpg)](https://www.youtube.com/watch?v=wdI3RePPbeQ)
  
---

señales ATLAS| aclaración en ESP32 DEV KIT 1 | numero pines
| :--- | ---: | :---:
Señal VGA64 | Casi todos los cores de ESP32 usan 64colores esquema 222+HS+VS | 8
Señal JOYSTICK DB9 | En preparación ATARI-PADDLE-ATLAS  | 6
Señal SD SPI | las señales QPI se usan en modo SPI| 4
Señal PS2 TECLADO  | Protocolo PS/2 | 2
Señal PS2 RATÓN | Protocolo PS/2 | 2
Señal Sonido Estereo | sonido delta sigma_(12bits) o un pwm_(10bits), DAC_ESP32(8bits)| 2
Señal Transmisión y Recepcion | RX TX sin gestión de flujo| 2
Señal EAR | Señal de entrada | 1

   No hay lugar para Las Señales de RX y TX como GPIOS, se tendrá que acceder desde el USB.
   En este diseño son Necesarias 27 señales, pero el ESP32 DEV KIT V1 dispone de 25 patillas/señales de las cuales 4 son de entrada, ahí conectamos las direcciones del mando de juegos de norma ATARI en el bus DB9.

   Inicialmente antes de realizar el recolocador final,hemos usado el esquema de bitluni:
   
   http://www.fabglib.org/conf_v_g_a.html
   
   Con la siguiente disposición de pines para la primera iteración de pines:
   
   VGAController.begin(GPIO_NUM_22, GPIO_NUM_21, GPIO_NUM_19, GPIO_NUM_18, GPIO_NUM_5, GPIO_NUM_4, GPIO_NUM_23, GPIO_NUM_15);
   
![PINEADO](https://github.com/AtlasFPGA/ESP32-ATLAS/blob/main/FOTOS/ESP32-DOIT-DEV-KIT-v1-pinout-mischianti.png)

##   Nota importante fijarse en los 4 pines que sólo permiten señales de entrada corresponden a los GPIO36, GPIO39, GPIO34, y GPIO35.

---

# ASIGNACIÓN ACTUAL; buscando tener el I2C con pull up en el DB9 y los 2 DAC de 8bits de resolución en la salida de sonido:
   los GPIO asociados a la salida de los dac de 8bit cada uno son, el GPIO25 y GPIO26.
   Esta es tercera iteración en su configuración final para el recolocador, a nivel lógico, sin amplificación de la señal de audio, para habilitar el puerto de audio con tensiones, es necesario cortar 2 pad stereo para que no llegue la señal a la I/O BOARD ATLAS, y estañar los pads que llevan las señales ESP_AUDIO_8BIT, al conector con tensión 3v3 y GND.

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
ATLAS Mini -> L2_KCLK,Morada -> K2_KDAT| GPIO14 | GPIO04 | TMDS_1_N
ATLAS Mini -> K2_KDAT,Morada -> L2_KCLK| GPIO12 | GPIO02 | EAR
TMDS_0_N| GPIO13 | GPIO15 | TMDSC_P
GND| GND | GND| GND
+5V| VIN | +3V3| +3V3

---
# TABLA EQUIVALENCIAS SALIDA CONECTOR DE VÍDEO DIGITAL
Nomenclatura DVI | Pares diferenciales | VGA64 | SCART128 | ZX-SCART
| ---: | ---: | ---: | ---: |:---: 
TMDS[0]|CLK- | HS | CSYNC | CSYNC
TMDS[1]|CLK+ | VS | G[0]  | BRIGHT
TMDS[2]|0-   | BLUE[0] | BLUE[0] | I2S_DIN_SD 
TMDS[3]|0+   | BLUE[1] | BLUE[1] | B
TMDS[4]|1-   | GREEN[0] | G[1] | I2S_BCLK_SCK
TMDS[5]|1+   | GREEN[1] | G[2] | G
TMDS[6]|2-   | RED[0] | RED[0] | I2S_LRC_WS
TMDS[7]|2+   | RED[1] | RED[1] | R

Las señales por determinar del módulo ZX-SCART llevan al menos en serie una resistencia de 270 ohm, pero al ser 3 señales se puede luchar icorporar al DAC de 15 colores un I2S, se han reflejado tanto la terminología de DAC como Micrófono por I2S.

Nota: en modo DVI y anulando los pares diferenciales negativos es decir a 0V, una señal de baja resolución tendría que generar los datos para los 3 colores en digital, y un reloj constate y 5 veces más veloz que el reloj de pixel asociado a su misma resolucion en analógico, en total 4 señales de vídeo digital, no sabemos si estas señales DVI pueden ser generadas.

---

   Esquema de referencia usado al inicio de las iteraciones:
![Esquemático de referencia FABGL](https://github.com/AtlasFPGA/ESP32-ATLAS/blob/main/FOTOS/fabglcircuit.gif)
    
---

   Lo que hace un total de 25 pines GPIO, para aumentar el diseño, esta el DB9 con 6 pines en PULL UP (Consultar esquema ATLAS-MINI), permite ampliarse el modelo existente mediante integrados I2C, con un buses de entrada salida en el bus DB9, tener en cuenta de las otras 4 señales, obligatoriamente tiene que ser señales de entrada.
   
   Consulta del esquema ATLAS MINI "La Roja" con licencia CERN OHL V2 STRICT:
   
   https://github.com/AtlasFPGA/BASECARRIERBOARDATLAS
   
   Consulta del esquema en easyeda:
   
   https://oshwlab.com/subcritical/carrier_io_board_atlas_mini_copy
   
## Dimensiones:

![ESP32-DOIT-DEVKIT-V1](https://github.com/AtlasFPGA/ESP32-ATLAS/blob/main/FOTOS/ESP32-DevKitC-Dimensions.png)

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
