

## Lab Module 06


### Description

1

El módulo MqttClientConnector, ubicado en el paquete programmingtheiot.cda.connection, implementa la interfaz IPubSubClient y tiene como objetivo establecer una conexión MQTT que permita la publicación y suscripción de mensajes en un entorno de dispositivos IoT. Este módulo sirve como capa de abstracción del protocolo MQTT, facilitando su uso dentro del sistema.

Dependencias Importantes
paho.mqtt.client: Librería cliente de MQTT para Python.

ConfigUtil: Clase para la carga de parámetros de configuración desde archivos externos.

IDataMessageListener: Interfaz que permite manejar mensajes entrantes.

ResourceNameEnum, ConfigConst: Enumeraciones para constantes y claves de configuración.

Implementaciones Clave
Constructor __init__(self, clientID: str = None)

Inicializa las propiedades del cliente MQTT utilizando los valores definidos en el archivo de configuración, como el host, puerto, keepAlive y nivel de QoS. También se asigna un clientID, ya sea desde la configuración o recibido como argumento. Este identificador debe ser único para evitar conflictos con otros clientes conectados al broker.

Método connectClient(self) -> bool

Establece la conexión con el broker MQTT. Si el cliente aún no ha sido creado, se instancia y se asocian los métodos callback para los distintos eventos MQTT: conexión, desconexión, recepción de mensajes, publicación y suscripción. Una vez configurado, se conecta al broker y se inicia el bucle de red que permite recibir y enviar mensajes de manera asíncrona.

Método disconnectClient(self) -> bool

Verifica si el cliente está conectado al broker. En caso afirmativo, detiene el bucle de red y cierra la conexión. Esto asegura un cierre controlado de la sesión MQTT y la liberación adecuada de recursos.

Método setDataMessageListener(self, listener: IDataMessageListener = None)

Permite establecer un listener externo para el manejo de mensajes entrantes. Este listener debe implementar la interfaz IDataMessageListener, permitiendo así una lógica personalizada de procesamiento de mensajes sin acoplarla directamente al cliente MQTT.

Métodos Pendientes de Implementación

publishMessage(...)

subscribeToTopic(...)

Actualmente estos métodos están definidos como stubs y solo registran mensajes en el log. Se han incluido como parte de la interfaz requerida y se desarrollarán más adelante.

Consideraciones de Diseño
El uso de ConfigUtil permite mantener los parámetros del sistema de forma centralizada y configurable.

La implementación de callbacks permite un modelo de eventos eficiente y desacoplado.

La interfaz IPubSubClient garantiza que el sistema sea extensible y pueda incorporar otros protocolos en el futuro, como CoAP o HTTP.

La separación entre la lógica de red (cliente MQTT) y la lógica de negocio (listener de mensajes) mejora la mantenibilidad y escalabilidad del sistema.

Pruebas de integración: 
- MqttClientConnectorTest


2


Claro, aquí tienes el texto redactado de manera similar al anterior, listo para documentación técnica:

Se agregaron múltiples métodos de callback al módulo MqttClientConnector con el propósito de manejar eventos generados por el cliente MQTT. Estas modificaciones permiten al conector responder de manera adecuada a eventos comunes del ciclo de vida de una conexión MQTT y facilitar futuras integraciones con otros componentes del sistema.

Los métodos implementados son los siguientes:

onConnect(): Se agregó para manejar la notificación de conexión exitosa con el broker MQTT. Su función actual es registrar un mensaje en el log, lo cual permite confirmar que el cliente se ha conectado correctamente.

onDisconnect(): Se implementó para manejar eventos de desconexión del broker. También registra un mensaje en el log para confirmar que la desconexión se ha producido.

onMessage(): Este método se añadió para gestionar la recepción de mensajes desde el broker. Por el momento, solo se registra el contenido del mensaje recibido (decodificado en UTF-8 si hay payload), lo que permite verificar la recepción efectiva de datos. Este método será clave en futuras etapas, donde los datos deberán enviarse a otros componentes del sistema como IDataListener.

onPublish(): Permite manejar la notificación de publicación exitosa de un mensaje en un topic. Se utiliza actualmente para registrar el evento en el log.

onSubscribe(): Se utiliza para manejar eventos de suscripción a topics. Como los anteriores, su implementación actual consiste en registrar el evento para propósitos de validación y depuración.

Además de la implementación de los métodos anteriores, se modificó el método connectClient() para asignar los nuevos callbacks al cliente MQTT antes de realizar la conexión al broker. Esto asegura que el cliente esté preparado para gestionar adecuadamente los eventos tan pronto como se establezca la conexión.

Pruebas de integración: 
- MqttClientConnectorTest


3-

Se añadieron métodos al módulo MqttClientConnector con el fin de implementar la funcionalidad de publicación y suscripción de mensajes mediante el protocolo MQTT. Estas capacidades permiten la comunicación entre dispositivos o servicios mediante el intercambio de mensajes en topics definidos.

Las modificaciones realizadas fueron las siguientes:

publishMessage(): Este método permite publicar un mensaje a un topic específico. Se agregaron validaciones para asegurar que el topic y el mensaje no estén vacíos, y que el nivel de QoS esté dentro del rango permitido (0–2). En caso de valores inválidos, se utiliza un valor por defecto definido en ConfigConst.DEFAULT_QOS. Además, se invoca wait_for_publish() para garantizar que la publicación se complete antes de continuar. Esta función retorna un valor booleano que indica si la operación fue válida.

subscribeToTopic(): Se implementó este método para permitir la suscripción a un topic determinado. Se realizan validaciones similares al método anterior respecto al topic y al nivel de QoS. Una vez validados, se ejecuta la suscripción mediante el cliente MQTT y se registra el evento para facilitar el seguimiento durante pruebas. Este método también devuelve un valor booleano que indica el éxito de la operación.

unsubscribeFromTopic(): Se añadió para permitir cancelar la suscripción a un topic. Se valida la entrada y, si es válida, se ejecuta la operación de desuscripción mediante el cliente MQTT, registrando igualmente el evento. El método retorna un valor booleano que indica si la operación fue realizada correctamente.

Estas funcionalidades son fundamentales para habilitar el patrón pub/sub en el sistema, permitiendo que el conector publique mensajes hacia el broker y escuche mensajes en topics de interés. Esto facilita la integración con otros dispositivos IoT y módulos de software que utilicen MQTT como canal de comunicación.

Pruebas: 
- No se realizaron pruebas

4-

Integración de MqttClientConnector en DeviceDataManager
Como parte de la implementación de conectividad MQTT en el sistema del CDA (Constrained Device Application), se ha realizado la integración del conector MQTT (MqttClientConnector) dentro del módulo principal de gestión de datos: DeviceDataManager. A continuación, se detallan los cambios realizados y su justificación:

1. Modificaciones en la clase DeviceDataManager
La clase DeviceDataManager se ha actualizado para permitir la conexión, suscripción, desconexión y limpieza del cliente MQTT de forma automática durante el ciclo de vida del sistema (inicio y parada del gestor de datos).

Constructor (__init__)
Se añadió una nueva variable de instancia: self.mqttClient = None. Además, se utiliza el componente ConfigUtil para verificar si el cliente MQTT está habilitado mediante la clave ENABLE_MQTT_CLIENT_KEY, definida en la sección CONSTRAINED_DEVICE. Si el cliente está habilitado, se instancia MqttClientConnector y se establece como listener de mensajes con el propio DeviceDataManager, lo que le permite reaccionar a mensajes entrantes.

Este cambio permite una inicialización condicional del cliente MQTT basada en parámetros de configuración, favoreciendo la flexibilidad del sistema.

Método startManager()
Si self.mqttClient está definido, se establece la conexión con el broker MQTT mediante connectClient(). Posteriormente, se realiza una suscripción al tópico CDA_ACTUATOR_CMD_RESOURCE utilizando una calidad de servicio (QoS) predeterminada, sin definir un callback personalizado.

Estas acciones aseguran que, al arrancar el gestor de datos, el sistema esté preparado para recibir comandos de actuador a través de MQTT.

Método stopManager()
Si el cliente MQTT está activo, se anula la suscripción al tópico mencionado previamente. Finalmente, se cierra la conexión al broker utilizando disconnectClient().

Esto garantiza una gestión adecuada de los recursos de red, evitando conexiones persistentes o suscripciones no liberadas.

Prubas de integración:
- MqttClientConnectorTest

### Code Repository and Branch

URL: https://github.com/BraisFernandezCaride/programmingtheiot/tree/lab06


### Unit Tests Executed

- No se han ejecutado test de unidad

### Integration Tests Executed


- MqttClientConnectorTest.py



