# Gateway Device Application (Connected Devices)

## Lab Module 11

### Descripción

#### 1. Mejoras en `MqttClientConnector`

Se introdujeron diversas actualizaciones en la clase `MqttClientConnector` con el propósito de incrementar su flexibilidad y facilidad de configuración. Entre los principales cambios se destacan:

- **Carga dinámica de configuración**  
  Nuevos constructores permiten inicializar parámetros desde secciones personalizadas del archivo `PiotConfig.props`, incluyendo compatibilidad con configuraciones específicas de `Cloud.GatewayService`.

- **Refactorización del proceso de inicialización**  
  Se encapsuló la lógica de inicialización dentro del método privado `initClientParameters(String configSection)` para favorecer la reutilización y facilitar el mantenimiento.

- **Conexión externa mediante `IConnectionListener`**  
  Se incorporó el método público `setConnectionListener(IConnectionListener listener)` que habilita la notificación a clases externas en eventos de conexión o desconexión.

- **Mayor accesibilidad para clases derivadas**  
  Métodos protegidos como `publishMessage`, `subscribeToTopic` y `unsubscribeFromTopic`, que aceptan parámetros tipo `String`, permiten una interacción más directa desde subclases o clases del mismo paquete.

- **Delegación en métodos públicos**  
  La lógica de los métodos públicos fue reestructurada para delegar en las nuevas implementaciones protegidas, manteniendo compatibilidad y mejorando la encapsulación.

- **Lógica condicional para suscripciones**  
  La función `connectComplete()` fue modificada para gestionar suscripciones basadas en la configuración `useCloudGatewayConfig`.

**Pruebas realizadas**  
Se ejecutaron pruebas de integración utilizando la clase `MqttClientConnectorTest`, comprobando la conectividad, publicación y suscripción a tópicos a través de un broker MQTT local.

---

#### 2. Creación de la interfaz `ICloudClient`

Con el fin de definir un contrato común para los clientes de comunicación con servicios en la nube bajo el modelo pub/sub, se diseñó la interfaz `ICloudClient`. Esta establece los métodos necesarios para:

- Gestionar la conexión y desconexión con la nube.
- Enviar objetos `SensorData` y `SystemPerformanceData`.
- Suscribirse o cancelar la suscripción a eventos relevantes.
- Establecer un `IDataMessageListener` para la recepción de mensajes desde la nube.

Esta interfaz promueve una arquitectura desacoplada y modular, facilitando la interoperabilidad con diversos servicios y protocolos de mensajería en la nube.

---

#### 3. Implementación de `CloudClientConnector`

En el paquete `programmingtheiot.gda.connection`, se desarrolló la clase `CloudClientConnector`, una implementación concreta de la interfaz `ICloudClient`. Esta clase utiliza internamente una instancia de `MqttClientConnector` para realizar las operaciones de conexión, publicación y suscripción.

- **Configuración de tópicos a través de `PiotConfig.props`**  
  Se estableció una estructura flexible para facilitar la interoperabilidad con múltiples proveedores de servicios en la nube.

- **Operaciones principales incluidas**  
  - Conexión y desconexión del servicio.
  - Envío de mensajes con datos de sensores y desempeño del sistema.
  - Subscripción a eventos provenientes de la nube.
  - Manejo opcional de `IDataMessageListener`.

- **Integración con `DeviceDataManager`**  
  Se actualizó este componente para activar o desactivar el cliente en la nube mediante una bandera booleana (`enableCloudClient`). También se ajustaron métodos como `startManager()`, `stopManager()`, `handleSensorMessage()` y `handleSystemPerformanceMessage()` para habilitar la transmisión automatizada de datos.

- **Soporte para comandos de actuación**  
  Se incorporó la lógica necesaria para interpretar comandos de actuadores desde la nube mediante el método `handleActuatorCommandRequest()`.

**Pruebas realizadas**  
Las pruebas de integración fueron llevadas a cabo en la clase `CloudClientConnectorTest`, validando tanto la conexión como el envío y recepción de mensajes por tópicos definidos.

---

#### 4. Flujo de datos completo: CDA ↔ GDA ↔ Nube

Durante el desarrollo se implementaron funcionalidades clave en los tres niveles del sistema (CDA, GDA y nube), permitiendo una comunicación bidireccional integral.

##### Cambios destacados:

- **Recolección y envío de datos**  
  - Configuración del servicio en la nube para recibir `SensorData` y `SystemPerformanceData` desde CDA y GDA.
  - Estructuración adecuada de tópicos para una organización eficiente.

- **Activación remota de LED mediante eventos**  
  - Se diseñó una regla en la nube que detecta umbrales críticos (e.g., temperatura o CPU elevada) y genera un evento de actuación.
  - Publicación de eventos en un tópico dedicado al control de LED (ON/OFF).

- **Subscripción del GDA a eventos de actuación**  
  - Configuración del GDA para escuchar el tópico correspondiente al control de LED.
  - Implementación de un `MessageListener` en `CloudClientConnector` que genera una instancia de `ActuatorData` (0 = OFF, 1 = ON) y la reenvía al CDA.

- **Procesamiento de comandos en el CDA**  
  - Validación de que el CDA pueda interpretar `ActuatorData` y ejecutar las acciones mediante `ActuatorAdapterManager`.

- **Gestión de conexión**  
  - Integración de la interfaz `IConnectionListener` para garantizar la suscripción tras una conexión exitosa.

- **Manejo eficiente de mensajes en GDA**  
  - Adaptación del método `handleIncomingMessage()` para procesar `ActuatorData` en formato JSON y reenviarlo al CDA.

- **Generación dinámica de tópicos**  
  - Se añadieron métodos auxiliares en `CloudClientConnector` para crear nombres de tópicos según los requerimientos del proveedor (ej. Ubidots).

##### Objetivo general  
Establecer un flujo confiable de extremo a extremo que permita visualizar métricas clave del sistema y activar dispositivos en respuesta a eventos detectados por la nube. Las pruebas de funcionamiento se documentaron con logs, comportamiento observable del LED y capturas visibles en `README.md`.

---

### Code Repository and Branch

**URL:**  
[https://github.com/BraisFernandezCaride/programmingtheiotjava/tree/lab11.4](https://github.com/BraisFernandezCaride/programmingtheiotjava/tree/lab11.4)

---

### Integration Tests Executed

- `MqttClientConnectorTest.java`  
- `CloudClientConnectorTest.java`

---

**EOF**
