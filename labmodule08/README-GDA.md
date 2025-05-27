# Gateway Device Application (Connected Devices)

## Lab Module 08

---

### Descripción

1. **Implementación de CoapServerGateway**

Se ha implementado una nueva clase Java denominada `CoapServerGateway` dentro del paquete `programmingtheiot.gda.connection` con el objetivo de proporcionar la funcionalidad de servidor CoAP utilizando la biblioteca open source **Californium** del proyecto Eclipse. Esta clase actúa como un adaptador entre el servidor CoAP y el sistema de gestión de datos del dispositivo (`DeviceDataManager`), permitiendo la integración de recursos locales accesibles mediante solicitudes **GET, PUT, POST y DELETE**.

**Objetivos y funcionalidades principales:**

- **Inicialización del servidor CoAP:**
  - Configuración estática requerida para Californium 3.8.0+ (`CoapConfig.register()` y `UdpConfig.register()`).
  - Creación de una instancia `CoapServer` y método `initServer()` para inicializar recursos.
  
- **Inyección de dependencias y control de flujo de datos:**
  - Constructor que acepta un objeto `IDataMessageListener` para comunicación bidireccional entre el servidor CoAP y `DeviceDataManager`.
  
- **Gestión del ciclo de vida del servidor:**
  - Métodos `startServer()` y `stopServer()` para controlar el inicio y parada segura del servidor.
  - Interceptor `MessageTracer` para registro de mensajes entrantes y salientes con fines de depuración.
  
- **Integración con DeviceDataManager:**
  - Configuración con atributo booleano `enableCoapServer` para activar/desactivar el servidor.
  - Modificación de métodos `initManager()`, `startManager()` y `stopManager()` para controlar el servidor CoAP según la configuración.
  - Instancia de `CoapServerGateway` dentro de `DeviceDataManager`, actuando como `IDataMessageListener`.

---

2. **Nuevos Handlers de Recursos CoAP**

Se implementaron dos nuevas clases Java dentro del paquete `programmingtheiot.gda.connection.handlers`:

- `UpdateSystemPerformanceResourceHandler`
- `UpdateTelemetryResourceHandler`

Estas clases manejan solicitudes CoAP de tipo **PUT**, permitiendo que el CDA (Constrained Device App) envíe datos de telemetría (`SensorData`) y rendimiento del sistema (`SystemPerformanceData`) al GDA (Gateway Device App).

**Características principales:**

- Basadas en la clase `GenericCoapResourceHandler`.
- Manejan solicitudes CoAP (`GET`, `PUT`, `POST`, `DELETE`).
- Permiten establecer un listener para manejar datos entrantes con `setDataMessageListener(IDataMessageListener listener)`.
- Procesan mensajes PUT con datos JSON, convierten los datos a objetos Java correspondientes y delegan el manejo a `DeviceDataManager`.
- Responden al cliente CoAP con códigos estándar (p. ej. `CHANGED`, `BAD_REQUEST`).

---

3. **Recurso CoAP Observable para Comandos de Actuador**

Se creó la clase `GetActuatorCommandResourceHandler` en el paquete `programmingtheiot.gda.connection.handlers` para implementar un recurso CoAP observable que permita al GDA notificar al CDA comandos de actuación usando la especificación **OBSERVE** de CoAP.

**Características destacadas:**

- Hereda de `CoapResource` y se declara observable (`super.setObservable(true)`).
- Implementa la interfaz `IActuatorDataListener` con el método `onActuatorDataUpdate(ActuatorData data)`.
- Notifica a los clientes observadores mediante `super.changed()` cuando hay actualización.
- Maneja solicitudes GET, enviando datos JSON del actuador al cliente.
- Almacena y gestiona los datos de comando en una instancia de `ActuatorData`.

Este recurso facilita la comunicación reactiva y automática de comandos desde el GDA al CDA.

---

4. **Actualizaciones en CoapServerGateway y DeviceDataManager**

Se mejoró la flexibilidad, modularidad e integración del servidor CoAP con los siguientes cambios:

- **CoapServerGateway**
  - Inicialización flexible para recursos internos y externos.
  - Implementación de `initServer()` para crear e inicializar el servidor y registrar recursos por defecto.
  - Método `addResource()` para agregar recursos dinámicamente, soportando nombres jerárquicos.
  - Estructura tipo árbol para recursos CoAP, permitiendo expansión en tiempo de ejecución.

- **DeviceDataManager**
  - Soporte para `IActuatorDataListener` con método `setActuatorDataListener(String name, IActuatorDataListener listener)`.
  - Modificación de `handleIncomingDataAnalysis(...)` para llamar al listener cuando se reciben datos de actuador, mejorando separación de responsabilidades.

- **Pruebas de Integración**
  - Implementación de `CoapServerGatewayTest`.
  - Simulación con cliente CoAP Californium para validar creación, inicialización y visibilidad de recursos expuestos.

---

### Repositorio de Código y Rama

URL:  
[https://github.com/BraisFernandezCaride/programmingtheiotjava/tree/lab08.4](https://github.com/BraisFernandezCaride/programmingtheiotjava/tree/lab08.4)

---

### Pruebas de Integración Ejecutadas

- `CoapClientToServerConnectorTest.java`

---

**EOF**

