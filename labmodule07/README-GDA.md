# Gateway Device Application (Connected Devices)

## Lab Module 07


### Description

1

La clase MqttClientConnector se ubica dentro del paquete programmingtheiot.cda.connection. Esta clase implementa las interfaces IPubSubClient (interfaz definida en el sistema para gestión de clientes publish-subscribe) y MqttCallbackExtended (proporcionada por la biblioteca Eclipse Paho).

Esta doble implementación permite que la clase pueda tanto ejecutar operaciones de conexión, publicación y suscripción, como manejar eventos asincrónicos del cliente MQTT, tales como reconexiones automáticas o pérdidas de conexión.

Variables de instancia
Se han definido variables a nivel de clase para gestionar el cliente MQTT (MqttClient), las opciones de conexión (MqttConnectOptions), el mecanismo de persistencia en memoria (MemoryPersistence) y el listener que recibe mensajes (IDataMessageListener). También se configuran variables para establecer la dirección del broker, el protocolo, el puerto, el ID del cliente, y un flag para determinar si se usará un cliente asincrónico o no.

Estas variables permiten una configuración flexible del cliente MQTT, obteniendo valores desde el archivo de configuración mediante la clase ConfigUtil.

Constructor sin argumentos
El constructor de la clase inicializa todos los parámetros necesarios para establecer una conexión MQTT. Utiliza el componente ConfigUtil para recuperar los valores de host, puerto, intervalo de keep-alive y un flag que indica si se debe utilizar un cliente asincrónico (MqttAsyncClient) o síncrono (MqttClient).

Además, se genera un ID de cliente único para cada instancia del conector, y se configuran las opciones de conexión como cleanSession en false y automaticReconnect en true, siguiendo las mejores prácticas del protocolo MQTT.

Métodos principales de conexión
connectClient()
Este método permite establecer la conexión al broker MQTT. Si el cliente no ha sido creado aún, se instancia utilizando el ID generado y el mecanismo de persistencia. Luego se invoca connect() si no se encuentra conectado.

Se incluye una gestión de errores adecuada utilizando bloques try-catch y registros de logging para informar sobre fallos en la conexión.

disconnectClient()
Este método gestiona la desconexión del cliente MQTT en caso de que esté conectado. Se asegura de liberar los recursos y registrar los eventos de manera informativa o advertencias cuando no hay conexión activa.

Métodos del interface IPubSubClient
Los métodos publishMessage(), subscribeToTopic() y unsubscribeFromTopic() han sido declarados pero no implementados completamente en esta fase. Actualmente, retornan false y se registran mensajes de que han sido invocados. Estos métodos serán completados en etapas posteriores del proyecto.

Además, se incluye el método setDataMessageListener() que permite registrar un listener externo que será utilizado para procesar mensajes entrantes. Este método comprueba que el listener no sea nulo antes de almacenarlo.

Métodos del callback MqttCallbackExtended
Se han definido los métodos connectComplete(), connectionLost(), deliveryComplete() y messageArrived() tal como requiere la interfaz MqttCallbackExtended. Aunque en esta etapa las implementaciones están vacías, se incorporarán registros y funcionalidades adicionales en etapas futuras del desarrollo, como se detalla en los módulos siguientes del laboratorio.

Pruebas de integración
Se han configurado pruebas de integración que permiten validar la correcta conexión y desconexión del cliente MQTT. En particular, se enfoca en la ejecución del método testConnectAndDisconnect() dentro del archivo MqttClientConnectorTest.

En esta fase inicial, se recomienda comentar las demás pruebas y dejar activa únicamente esta, para centrarse en la verificación básica de la conexión al broker. Los mensajes de advertencia que aparecen durante la ejecución (por ejemplo, intentar conectar dos veces o desconectar sin conexión activa) son esperados y forman parte del comportamiento previsto.

Prueba integración:
- MqttClientConnectorTest.java

2

Agregar callbacks en la clase MqttClientConnector para manejar eventos del cliente MQTT:

Conexión completada (connectComplete)

Pérdida de conexión (connectionLost)

Publicación completada (deliveryComplete)

Mensaje recibido (messageArrived)

Para ello:

Implementar el métoddo connectComplete(boolean reconnect, String serverURI) para registrar en el log cuando se establece una conexión (o reconexión) exitosa con el broker MQTT.

Implementar el método connectionLost(Throwable t) para registrar en el log cuando se pierde la conexión con el broker.

Implementar el método deliveryComplete(IMqttDeliveryToken token) para registrar cuando se ha completado la entrega de un mensaje publicado.

Implementar el método messageArrived(String topic, MqttMessage message) para registrar la llegada de un mensaje en un topic suscrito. Más adelante, se enlazará con una instancia de IDataListener.

Asegurar que la instancia de MqttClient utilice como callback la propia clase MqttClientConnector, que implementa MqttCallbackExtended, mediante this.mqttClient.setCallback(this) inmediatamente después de su instanciación.

Prueba itegración:
- MqttClientConnectorTest.java


3

Añadir Funcionalidad de Publicar y Suscribirse en MqttClientConnector

Implementar los métodos de publicación, suscripción y cancelación de suscripción en la clase MqttClientConnector, así como completar la implementación del método isConnected().

Se requiere:

Implementar el método publishMessage(ResourceNameEnum topicName, String msg, int qos) para manejar la publicación de mensajes MQTT. Este método debe:

Validar que el topicName y el msg no sean nulos o vacíos.

Validar que el qos esté en el rango permitido (0 a 2); si no, establecerlo en DEFAULT_QOS.

Publicar el mensaje utilizando this.mqttClient.publish(...).

Retornar true si la publicación fue exitosa, de lo contrario false.

Implementar el método subscribeToTopic(ResourceNameEnum topicName, int qos) para manejar la suscripción a topics. Este método debe:

Validar que el topicName no sea nulo.

Validar que el qos esté dentro del rango permitidoo o asignar DEFAULT_QOS.

Llamar a this.mqttClient.subscribe(...).

Retornar true si la suscripción fue exitosa, de lo contrario false.

Implementar el método unsubscribeFromTopic(ResourceNameEnum topicName) para manejar la cancelación de suscripciones. Este método debe:

Validar que el topicName no sea nulo.

Llamar a this.mqttClient.unsubscribe(...).

Retornar true si la operación fue exitosa, de lo contrario false.

Completar la implementación del método isConnected() para verificar si el cliente MQTT está conectado. Este método debe:

Retornar true si this.mqttClient no es nulo y está conectado mediante this.mqttClient.isConnected().

Nota: Si se utiliza MqttAsyncClient, se debe usar una variable booleana de clase que se actualice en los callbacks connectComplete() y connectionLost().


Prueba integracion
- MqttClientConnectorTest.java


4

Esta tarea consiste en conectar MqttClientConnector con DeviceDataManager para habilitar la funcionalidad de cliente MQTT dentro del gestor de datos del dispositivo.


Acciones requeridas:


Habilitar el uso de MQTT en DeviceDataManager mediante una variable booleana de alcance de clase llamada enableMqttClient. Esta variable se puede establecer de dos formas:

Agregar la variable mqttClient como instancia de clase de MqttClientConnector en el constructor de DeviceDataManager.

Inicialización en el método initManager()

En el método initManager(), si enableMqttClient es verdadero, se debe crear una nueva instancia de MqttClientConnector, asignarla a mqttClient, y establecer el listener de mensajes con this.mqttClient.setDataMessageListener(this).

Conexión en el método startManager()

En el método startManager(), si mqttClient no es nulo, se debe invocar this.mqttClient.connectClient(). Si la conexión es exitosa, se deben realizar las suscripciones a los siguientes tópicos con el nivel de calidad de servicio (QoS) predeterminado:

PIOT/GatewayDevice/MgmtStatusMsg

PIOT/ConstrainedDevice/ActuatorResponse

PIOT/ConstrainedDevice/SensorMsg

PIOT/ConstrainedDevice/SystemPerfMsg

Desconexión en el método stopManager()

En el método stopManager(), si mqttClient no es nulo, se deben realizar las cancelaciones de suscripción correspondientes a los tópicos previamente suscritos. Posteriormente, se debe invocar this.mqttClient.disconnectClient() para cerrar la conexión con el broker MQTT.

Pruebas

Crear una clase de pruebas personalizada denominada MqttClientControlPacketTest en el directorio ./src/test/java/programmingtheiot/part03/integration/connection. Esta clase debe contener pruebas que permitan generar todos los paquetes de control del protocolo MQTT 3.1.1:

CONNECT y CONNACK: realizando una conexión exitosa.

PUBLISH y PUBACK: publicando mensajes con QoS 1.

PUBLISH, PUBREC, PUBREL, y PUBCOMP: publicando mensajes con QoS 2.

SUBSCRIBE y SUBACK: suscribiéndose exitosamente a un tópico.

UNSUBSCRIBE y UNSUBACK: anulando una suscripción.

PINGREQ y PINGRESP: manteniendo la conexión activa por al menos el intervalo definido como "Keep-Alive".

DISCONNECT: cerrando la conexión correctamente.


### Code Repository and Branch


URL: https://github.com/BraisFernandezCaride/programmingtheiotjava/tree/lab07






### Unit Tests Executed

No se realizan pruebas unitarias

### Integration Tests Executed


- MqttClientConnectorTest.java

EOF.
