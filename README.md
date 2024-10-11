# Unidad3_SFI_RICARDO_GUERRERO
# Unidad 3. **Protocolos Binarios**

### **Introducción**

En esta unidad seguiremos profundizando en la integración de dispositivos periféricos a las aplicaciones interactivas, pero esta vez, usando protocolos binarios.

#### **Propósitos de aprendizaje**

Integrar aplicaciones interactivas y periféricos utilizando protocolos seriales binarios.

#### **Trayecto de actividades**

### **Ejercicio 1: introducción a los protocolos binarios - caso de estudio**

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
- Cabecera (Header):  Proporciona detalles sobre el mensaje, como su tipo, protocolo y tamaño, permitiendo que se gestione correctamente.

- Cuerpo (Payload): Contiene la información principal o comandos que se están enviando al destinatario.

- Checksum o CRC: Asegura que el mensaje no tenga errores al comparar un valor enviado con el valor calculado al recibirlo.

- Final de Mensaje (Footer): Marca dónde termina el mensaje para que el receptor sepa hasta qué punto procesarlo.
### **Ejercicio 2: experimento**

En [este](https://www.arduino.cc/reference/en/language/functions/communication/serial/) enlace vas a mirar los siguientes métodos. Te pediré que, por favor, los tengas a mano porque te servirán para resolver problemas. Además, en este punto, hagamos un repaso de las funciones que han apoyado la comunicación seral:

#### ¿Sospecha por qué se ha excluido? La razón es porque en un protocolo binario usualmente no tiene un carácter de FIN DE MENSAJE, como si ocurre con los protocolos ASCII, donde usualmente el último carácter es el `\n`.
`Serial.readBytesUntil()` fue excluida porque está diseñada para leer hasta un carácter específico, como en los protocolos ASCII con `\n` al final. En protocolos binarios, no hay un carácter de fin de mensaje, por lo que se usa la longitud del mensaje en la cabecera.

### **Ejercicio 4: transmitir números en punto flotante**

#### - ¿En qué *endian* estamos transmitiendo el número?
Los procesadores de las placas Arduino con arquitectura AVR, por defecto, emplean el formato little-endian.

#### - Y si queremos transmitir en el *endian* contrario, ¿Cómo se modifica el código?
Para enviar un número en formato big-endian, es necesario invertir el orden de los bytes antes de la transmisión. Esto se logra cambiando manualmente las posiciones de los bytes en un buffer previo al envío.


### **Ejercicio 5: envía tres números en punto flotante**

void setup() {
    Serial.begin(115200);
}

void loop() {
    // Definir dos números flotantes
    static float num1 = 4567.8912;
    static float num2 = 987.654;
    static uint8_t arr1[4] = {0};
    static uint8_t arr2[4] = {0};

    if (Serial.available()) {
        if (Serial.read() == 's') {
            // Copiar los bytes de ambos números en los arreglos
            memcpy(arr1, (uint8_t *)&num1, 4);
            memcpy(arr2, (uint8_t *)&num2, 4);

            // Transmitir en formato little-endian
            Serial.println("Transmitting in Little-endian:");
            for (int8_t i = 0; i < 4; i++) {
                Serial.write(arr1[i]);
            }
            for (int8_t i = 0; i < 4; i++) {
                Serial.write(arr2[i]);
            }

            // Transmitir en formato big-endian
            Serial.println("Transmitting in Big-endian:");
            for (int8_t i = 3; i >= 0; i--) {
                Serial.write(arr1[i]);
            }
            for (int8_t i = 3; i >= 0; i--) {
                Serial.write(arr2[i]);
            }
        }
    }
}

### Ejercicio 6: aplicación interactiva

###### El primer fragmento: 
de código se encarga de configurar y abrir el puerto serial, estableciendo la comunicación entre el dispositivo y el microcontrolador. Esto es esencial para permitir el intercambio de datos y garantizar que ambos dispositivos puedan enviar y recibir información de manera efectiva.

###### El segundo fragmento:
envía un byte específico a través del puerto serial, lo que implica que se están transmitiendo datos hacia el microcontrolador. Esta acción es fundamental para el control y la interacción con el microcontrolador, ya que le permite recibir órdenes o información necesaria para su funcionamiento.

###### El tercer fragmento de código:
se dedica a leer 4 bytes que han sido enviados desde el microcontrolador y los imprime en formato hexadecimal. Este proceso es crucial, ya que permite verificar y visualizar los datos que se reciben, asegurando que la comunicación entre el dispositivo y el microcontrolador esté funcionando correctamente y que los datos sean los esperados.
