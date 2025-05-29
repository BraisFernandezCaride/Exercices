## Lab Module 10


### Description


1

  La clase `MqttClientConnector` fue actualizada para incorporar soporte de autenticación mediante usuario y contraseña, además de habilitar conexiones cifradas TLS con el broker MQTT. Esto mejora tanto la seguridad como la flexibilidad del sistema, permitiendo configuraciones robustas y conexiones seguras.

  **Objetivos de los cambios:**

  - **Autenticación:** Se añadió la capacidad de cargar credenciales desde un archivo externo, permitiendo configurar usuario y contraseña de forma dinámica.
  - **Cifrado TLS:** Se implementó soporte para conexiones seguras mediante certificados en formato PEM.
  - **Modularidad de configuración:** La carga de parámetros como ID de cliente, puerto, host, uso de cliente asíncrono y opciones como reconexión automática y sesiones limpias fue refactorizada para provenir de archivos de configuración externos.
  - **Mantenimiento y escalabilidad:** La inicialización de parámetros se centralizó en métodos dedicados para facilitar futuras modificaciones y mejorar la organización del código.

  **Cambios específicos:**

  - Incorporación de nuevas propiedades y métodos para la gestión de credenciales (`initCredentialConnectionParameters`) y conexiones seguras (`initSecureConnectionParameters`).
  - Creación del método `initClientParameters` para centralizar la carga de configuración.
  - Modificación del constructor sin argumentos para utilizar este nuevo método.
  - Inclusión de nuevos imports necesarios para manejo de certificados y conexiones seguras.

  **Pruebas realizadas:**

  - Se ejecutaron casos de prueba existentes en `MqttClientConnectorTest` con el broker local sin TLS para verificar el correcto funcionamiento bajo configuración estándar.


2

  La clase `MqttClientConnector` fue adaptada para utilizar el cliente asíncrono `MqttAsyncClient` en lugar de `MqttClient`, con el objetivo de evitar bloqueos en la comunicación entre tópicos del broker MQTT. Esto implicó modificar tanto la declaración del cliente como el método `connectClient()` para ajustarlo a la lógica asíncrona.

  Además, se implementó la suscripción a tópicos clave del CDA desde el método `connectComplete()`:

  - Mensajes de tipo `SensorData`
  - Mensajes de tipo `SystemPerformanceData`
  - Mensajes de respuesta `ActuatorData`

  Para el procesamiento de estos mensajes, se consolidó un único manejador central, `messageArrived()`, que parsea y redirige los mensajes a sus respectivos listeners (`IDataMessageListener`) según el tópico.

  Se eliminó la lógica previa de suscripción en `DeviceDataManager.startManager()` para garantizar que todas las suscripciones se realicen exclusivamente desde `connectComplete()`, asegurando un manejo más ordenado y coherente tras establecer conexión con el broker.

  **Objetivo:**

  Centralizar y simplificar la gestión de mensajes entrantes mediante un único punto de entrada, además de mejorar la robustez del sistema frente a bloqueos utilizando el cliente MQTT asíncrono. Esto favorece la escalabilidad y la capacidad de respuesta ante múltiples dispositivos o servicios.

  **Pruebas realizadas:**

  Se llevaron a cabo pruebas unitarias específicas en `MqttClientConnectorTest`, incluyendo `testConnectAndDisconnect()` y un nuevo test para validar la recepción y procesamiento de mensajes de respuesta `ActuatorData`. Los resultados confirmaron el correcto funcionamiento del cliente asíncrono y la lógica de suscripción unificada.


3

  Se extendió la funcionalidad de `DeviceDataManager` para gestionar y analizar mensajes entrantes del CDA, específicamente los tipos `SensorData`, `SystemPerformanceData` y `ActuatorData`.

  Se implementó un sistema de análisis para datos de sensores de humedad que incluye lógica de cruce de umbrales configurable mediante propiedades definidas en el archivo `PiotConfig.props`. Esta lógica permite que el GDA reaccione ante eventos de humedad fuera de rango, creando y enviando comandos `ActuatorData` al CDA para activar o desactivar un humidificador. La activación ocurre cuando se detectan dos eventos de cruce de umbral separados por un intervalo temporal mínimo configurable.

  Además, se incorporaron métodos privados para manejar y registrar análisis de datos de sensores, estado del sistema y comandos de actuadores, preparando la infraestructura para futuras integraciones con servicios en la nube o persistencia de datos.

  Por último, el constructor fue actualizado para cargar configuraciones desde el archivo de propiedades y se añadieron variables de clase para almacenar el estado y marcas temporales de sensores y actuadores, facilitando así análisis simples de series temporales.

  **Tests realizados:**

  Se desarrollaron pruebas que simulan mensajes `SensorData` con distintos valores de humedad para verificar la lógica de análisis y la generación de eventos de actuación en función de la configuración establecida.


### Code Repository and Branch

  URL: https://github.com/BraisFernandezCaride/programmingtheiotjava/tree/lab10.3


### Integration Tests Executed

  - `MqttClientConnectorTest.java`


EOF.
