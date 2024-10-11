# Unidad3_SFI_RICARDO_GUERRERO
# Unidad 3. **Protocolos Binarios**

### **Introducción**

En esta unidad seguiremos profundizando en la integración de dispositivos periféricos a las aplicaciones interactivas, pero esta vez, usando protocolos binarios.

#### **Propósitos de aprendizaje**

Integrar aplicaciones interactivas y periféricos utilizando protocolos seriales binarios.

### **Trayecto de actividades**

#### **Ejercicio 1: introducción a los protocolos binarios - caso de estudio**

¿Cómo se ve un protocolo binario? Para explorar este concepto te voy a mostrar una hoja de datos de un [sensor](http://www.chafon.com/productdetails.aspx?pid=382) comercial que usa un protocolo de comunicación binario. La idea es que tu explores su hoja de datos ([datasheet](https://drive.google.com/file/d/1uDtgNkUCknkj3iTkykwhthjLoTGJCcea/view?pli=1)), tanto como sea posible, invitándote a que observes con detenimiento hasta la página 5.

Durante la lectura, intenta dar respuestas a las siguientes preguntas:

#### - ¿Cómo se ve un protocolo binario?
Un protocolo binario consiste en una serie de bits (0s y 1s) organizados bajo un formato específico. Esta serie se emplea para transmitir datos de forma eficiente entre dos sistemas. A diferencia del texto comprensible para humanos, los mensajes binarios son más compactos, ocupan menos espacio y se envían en menos tiempo.
#### - ¿Puedes describir las partes de un mensaje?
Checksum o CRC:
Es un valor que asegura que el mensaje no haya sido alterado, ayudando a identificar errores en la transmisión.

Final de Mensaje (Footer):
Es un marcador que señala el cierre del mensaje, si resulta necesario.

Cabecera (Header):
Incluye información adicional sobre el mensaje, como el tipo, la versión del protocolo, el tamaño, entre otros detalles.

Cuerpo (Payload):
Contiene los datos esenciales que se están enviando, ya sea información útil o instrucciones.
#### - ¿Para qué sirve cada parte del mensaje?

### **Ejercicio 2: experimento**

En [este](https://www.arduino.cc/reference/en/language/functions/communication/serial/) enlace vas a mirar los siguientes métodos. Te pediré que, por favor, los tengas a mano porque te servirán para resolver problemas. Además, en este punto, hagamos un repaso de las funciones que han apoyado la comunicación seral:

##### ¿Sospecha por qué se ha excluido? La razón es porque en un protocolo binario usualmente no tiene un carácter de FIN DE MENSAJE, como si ocurre con los protocolos ASCII, donde usualmente el último carácter es el `\n`.
