# USB (Universal Serial Bus)

El USB es un bus de comunicaciones que sigue un estándar que define los cables, conectores y protocolos usados en un bus para conectar, comunicar y proveer de alimentación eléctrica entre computadoras, periféricos y dispositivos electrónicos.  
El USB es utilizado como estándar de conexión de periféricos como: teclados, ratones, memorias USB, joysticks, escáneres, cámaras digitales, teléfonos móviles, reproductores multimedia, impresoras, dispositivos multifuncionales, sistemas de adquisición de datos, módems, tarjetas de red, tarjetas de sonido, tarjetas sintonizadoras de televisión y grabadoras de DVD externas, discos duros externos y disqueteras externas.

## HID (Human Interface Device)
El protocolo HID realiza la implementacion de dispositivos de manera simple. Los dispositivos definen sus paquetes de datos y luego envían un "descriptor HID" al host. El descriptor HID es un vector de bytes hard-codedeados (es decir, que los datos están escritos en el mismo programa) que describe los paquetes de datos. Esto incluye: cuántos paquetes soporta el dispositivo, de que tamaño son los paquetes y el propósito de cada byte y cada bit en el paquete.  
Por ejemplo, en un teclado con un botón con la función de abrir la calculadora le indica al host que el estado de presionado/soltado se encuentra en el 2do bit del 6to byte en el paquete de datos número 4. (Nota: Estas localizaciones son solo de ejemplo y específicas del dispositivo). El dispositivo generalmente guarda el descriptor HID en la ROM y no necesita entender el significado o modificarlo.  

## Generación del descriptor
En resumen, el descriptor es lo que le indica al host qué dispositivo es el que se conectó, los datos que se enviarán y su órden.  
La mejor manera de entender como crear un descriptor para un dispositivo es haciendo uno de ejemplo.  

Antes de empezar se debe tener a mano el documento [Device Class Definition HID](http://www.usb.org/developers/hidpage/HID1_11.pdf) el cual nos proporcionará información muy útil acerca del protocolo HID y la [HID Usage Table](http://www.usb.org/developers/hidpage/Hut1_12v2.pdf) en la que se encuentran las USAGE_PAGE y los USAGE_ID para cada tipo de dispositivo.
También se necesitará el software [HID Descriptor Tool](http://www.usb.org/developers/hidpage/dt2_4.zip) el cual nos brinda una manera rápida de configurar el descriptor sin tener que estar leyendo en la HID Usage Table los valores en hexadecimal, logrando así una mejor comprensión del código.  

Para el ejemplo haremos un dispositivo HID comportandose como un mouse. El mouse cuenta con 3 botones (clic izquierdo, clic central y clic derecho) junto a dos dimensiones las cuales son dadas por el movimiento en el eje X y en el eje Y. De esta manera nos queda la siguiente tabla:

![TablaMouse](https://github.com/luxarts/STM32F103/blob/master/Ejemplos/USB/TablaEjemploMouse.PNG?raw=true)

Por lo que la estructura en C quedaría
```c
struct mouse_t{  
    uint8_t botones;  
    int8_t x;  
    int8_t y;  
}
```
En el descriptor, el primer item debe describir los botones
```
USAGE_PAGE (Button)
USAGE_MINIMUM (Button 1)
USAGE_MAXIMUM (Button 3)
```
Cada boton está representado por un bit, por lo cual puede tomar el valor de 0 o de 1
```
LOGICAL_MINIMUM (0)
LOGICAL_MAXIMUM (1)
```
Son 3 datos (COUNT) en espacios de 1 bit (SIZE)
```
REPORT_COUNT (3)
REPORT_SIZE (1)
```
Envía el reporte a el host como un dato, variable, absoluto
```
INPUT (Data,Var,Abs)
```
Se deben enviar los 5 bits que no se usan como una constante
```
REPORT_COUNT (1)
REPORT_SIZE (5)
INPUT (Cnst,Var,Abs)
```

Ahora es el turno del eje X
```
USAGE_PAGE (Generic Desktop)
USAGE (X)
```
Como se quiere un entero con signo y el tamaño del dato es de 8 bits (un byte), el mínimo será -127 y el máximo 127
```
LOGICAL_MINIMUM (-127)
LOGICAL_MAXIMUM (127)
```
Es 1 dato de 8 bits
```
REPORT_COUNT (1)
REPORT_SIZE (8)
```
Envía el reporte al host como un dato, variable, relativo
```
INPUT (Data,Var,Rel)
```

Por último para el eje Y repetimos los pasos del eje X quedando como
```
USAGE_PAGE (Generic Desktop)
USAGE (Y)
LOGICAL_MINIMUM (-127)
LOGICAL_MAXIMUM (127)
REPORT_COUNT (1)
REPORT_SIZE (8)
INPUT (Data,Var,Rel)
```
Lo cual se puede simplificar en
```
USAGE_PAGE (Generic Desktop)
USAGE (X)
USAGE (Y)
LOGICAL_MINIMUM (-127)
LOGICAL_MAXIMUM (127)
REPORT_COUNT (2)
REPORT_SIZE (8)
INPUT (Data,Var,Rel)
```
Juntando los botones y los ejes obtenemos
```
USAGE_PAGE (Button)
USAGE_MINIMUM (Button 1)
USAGE_MAXIMUM (Button 3)
LOGICAL_MINIMUM (0)
LOGICAL_MAXIMUM (1)
REPORT_COUNT (3)
REPORT_SIZE (1)
INPUT (Data,Var,Abs)
REPORT_COUNT (1)
REPORT_SIZE (5)
INPUT (Cnst,Var,Abs)
USAGE_PAGE (Generic Desktop)
USAGE (X)
USAGE (Y)
LOGICAL_MINIMUM (-127)
LOGICAL_MAXIMUM (127)
REPORT_COUNT (2)
REPORT_SIZE (8)
INPUT (Data,Var,Rel)
```
Para que el host reconozca el dispositivo como un mouse se debe establecer
```
USAGE_PAGE (Generic Desktop)
USAGE (Mouse)
COLLECTION (Application)
    USAGE (Pointer)
    COLLECTION (Physical)
    
    //Acá el código escrito anteriormente
    
    END COLLECTION
END COLLECTION
```

# Agregar el descriptor al proyecto
- Generar un proyecto con CubeMX usando el USB como HID (NO CUSTOM HID)
- Editar el archivo "usbd_hid.c" (Middlewares > USB_Device_library)
    - USBD_HID_CfgDesc[…]
