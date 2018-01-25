# USB (Universal Serial Bus)

El USB es un bus de comunicaciones que sigue un estándar que define los cables, conectores y protocolos usados en un bus para conectar, comunicar y proveer de alimentación eléctrica entre computadoras, periféricos y dispositivos electrónicos.  
El USB es utilizado como estándar de conexión de periféricos como: teclados, ratones, memorias USB, joysticks, escáneres, cámaras digitales, teléfonos móviles, reproductores multimedia, impresoras, dispositivos multifuncionales, sistemas de adquisición de datos, módems, tarjetas de red, tarjetas de sonido, tarjetas sintonizadoras de televisión y grabadoras de DVD externas, discos duros externos y disqueteras externas.

## HID (Human Interface Device)
El protocolo HID realiza la implementacion de dispositivos de manera simple. Los dispositivos definen sus paquetes de datos y luego envían un "descriptor HID" al host. El descriptor HID es un vector de bytes hard-codedeados (es decir, que los datos están escritos en el mismo programa) que describe los paquetes de datos. Esto incluye: cuántos paquetes soporta el dispositivo, de que tamaño son los paquetes y el propósito de cada byte y cada bit en el paquete.  
Por ejemplo, en un teclado con un botón con la función de abrir la calculadora le indica al host que el estado de presionado/soltado se encuentra en el 2do bit del 6to byte en el paquete de datos número 4. (Nota: Estas localizaciones son solo de ejemplo y específicas del dispositivo). El dispositivo generalmente guarda el descriptor HID en la ROM y no necesita entender el significado o modificarlo.  

En resumen, el descriptor es lo que le indica al host qué dispositivo es el que se conectó, los datos que se enviarán y su órden.  
La mejor manera de entender como crear un descriptor para un dispositivo es haciendo uno de ejemplo.  

Antes de empezar se debe tener a mano el documento [Device Class Definition HID](http://www.usb.org/developers/hidpage/HID1_11.pdf) el cual nos proporcionará información muy útil acerca del protocolo HID y la [HID Usage Table](http://www.usb.org/developers/hidpage/Hut1_12v2.pdf) en la que se encuentran las USAGE_PAGE y los USAGE_ID para cada tipo de dispositivo.
También se necesitará el software [HID Descriptor Tool](http://www.usb.org/developers/hidpage/dt2_4.zip) el cual nos brinda una manera rápida de configurar el descriptor sin tener que estar leyendo en la HID Usage Table los valores en hexadecimal, logrando así una mejor comprensión del código.

