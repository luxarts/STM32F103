# STM32F103
Manual español sobre el microcontrolador STM32F103C8. Toda la documentación y ejemplos aquí expuestos han sido probados.

# Software
El software se compone de dos partes: El IDE y las librerías
### Entorno de desarrollo (IDE)
Todos los ejemplos aquí expuestos fueron desarrollados en el IDE [TrueSTUDIO](https://atollic.com/resources/download/windows/) de la empresa Atollic. Sin embargo la mayoría de los IDE para arquitectura ARM funcionarán. Algunos de ellos son:
- CooCoox CoIDE
- mikroC (pago)
- SW4STM32
- IAR

### Librerías
Existen cuatro librerías para el desarrollo de aplicaciones en C para dispositivos ARM:
- SPL (Standard Peripheral Library): Es la mas antigua. Es la incluida al crear un proyecto desde el TrueSTUDIO.
- HAL (Hardware Abstraction Layer): Es mas nueva que la anterior. Su uso es mas fácil que la SPL a coste de sacrificar un poco de rendimiento. Se incluye al crear el proyecto con el CubeMX y será la utilizada en las explicaciones y ejemplos de este repositorio.
- LL (Low Layer): Es la mas reciente y se creó con el fin de dar la posibilidad al programador de trabajar en un lenguaje de alto nivel (C) teniendo el control como en un lenguaje de bajo nivel (ASM).
- Generica: Librería por defecto incluida al crear un proyecto en cualquier compilador. Esta libreria tiene los defines con los nombres que aparecen en la [hoja de referencia](https://github.com/luxarts/STM32F103/blob/master/Documentos/STM32%20Reference.pdf) y sus respectivas posiciones de memoria.

# Hardware
El hardware utilizado es el microcontrolador STM32F103C8 de la empresa ST Microelectronics. Dicho microcontrolador se puede conseguir de manera individual o soldado sobre una placa similar a una Arduino Micro.

### Características
- Arquitectura: ARM Cortex M3 32-bit
- Frecuencia máxima: 72MHz
- Memoria FLASH: 64kb
- Memoria SRAM: 20kb
- Tensión de trabajo: 2.0v a 3.6v
- Comunicaciónes: 3x USART, 2x I2C, 2x SPI, CAN, USB
- Timer: 3x 16 bit (15 canales PWM)
- ADC: 12 bits (10 canales)
- DMA: 7 canales
- GPIO: 32 pines disponibles, la mayoría tolera 5V

![Pinout](https://github.com/luxarts/STM32F103/blob/master/Documentos/STM32-Pinout.gif?raw=true)

Para programar este microcontrolador se pueden utilizar dos maneras:
- **USB-Serie**: Utilizar el puerto USB integrado en la placa. Para ello se debe cargar un bootloader (programa de arranque) en el microcontrolador de tal manera que use el puerto USB como un puerto serie virtual. Con este método es posible grabar las aplicaciones en la memoria FLASH utilizando el IDE Arduino. Este método no tiene depurador (debug).
- **ST Link V2**: Es un programador/depurador de formato similar a un pendrive. La conexión se lleva a cabo por 2 lineas (SWDIO y SWCLK) junto a la alimentación (3.3v y GND) y el reset. Este dispositivo es el utilizado en este repositorio para cargar los programas en el microcontrolador y depurarlos.

# Links útiles
- [Solución: No device found on target](https://electronics.stackexchange.com/questions/204996/stm32-st-link-cannot-connect-to-mcu-after-successful-programming)
- [Tutorial USB HID report descriptor](http://eleccelerator.com/tutorial-about-usb-hid-report-descriptors/)
- [Custom HID USB para STM32](https://damogranlabs.com/2016/03/stm32-custom-usb-hid-device-yes-please/)
- [STM32CubeIDE not open in macOS](https://community.st.com/s/question/0D50X0000BUh6nK/not-able-to-open-stm32cubeide-110-on-macos-mohave-10146)
