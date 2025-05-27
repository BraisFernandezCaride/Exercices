# Gateway Device Application (Connected Devices)

## Lab Module 08



### Description

1
Se ha implementado una nueva clase Java denominada CoapServerGateway dentro del paquete programmingtheiot.gda.connection con el objetivo de proporcionar la funcionalidad de servidor CoAP utilizando la biblioteca open source Californiuum del proyecto Eclipse. Esta clase actúa como un adaptador entre el servidor CoAP y el sistema de gestión de datos del dispositivo (DeviceDataManager), permitiendo la integración de recursos locales accesibles mediante solicitudes GET, PUT, POST y DELETE.

Objetivos y Funcionalidad:
Inicialización del servidor CoAP:

Se agregó la configuración estática inicial requerida por las versiones de Californium superiores a 3.8.0 (CoapConfig.register() y UdpConfig.register()).

Se creó una instancia de CoapServer y se integró un mecanismo para inicializar recursos mediante el método initServer().

Inyección de dependencias y control del flujo de datos:

Se definió un constructor que acepta un objeto IDataMessageListener para habilitar la comunicación bidireccional entre el servidor CoAP y DeviceDataManager.

Gestión del ciclo de vida del servidor:

Se implementaron los métodos startServer() y stopServer() para controlar el inicio y la detención segura del servidor CoAP.

Se agregó un interceptor MessageTracer para registrar la actividad de mensajes entrantes/salientes con fines de depuración.

Integración con DeviceDataManager:

Se añadió un atributo booleano enableCoapServer en la configuración para permitir la activación condicional del servidor.

Se modificaron los métodos initManager(), startManager() y stopManager() para inicializar, arrancar y detener el servidor CoAP únicamente si dicha funcionalidad está habilitada.

Se estableció una instancia de CoapServerGateway dentro de DeviceDataManager, con la clase actuando como IDataMessageListener.



2
Se implementaron dos nuevas clases en Java dentro del paquete programmingtheiot.gda.connection.handlers:

UpdateSystemPerformanceResourceHandler

UpdateTelemetryResourceHandler

Estas clases fueron desarrolladas con el objetivo de manejar solicitudes CoAP del tipo PUT, permitiendo que el CDA (Constrained Device App) envíe datos de telemetría (SensorData) y de rendimiento del sistema (SystemPerformanceData) al GDA (Gateway Device App).

Ambas clases están basadas en el diseño de la clase GenericCoapResourceHandler, por lo que reutilizan su estructura básica, incluyendo la herencia desde CoapResource, la implementación de métodos para manejar solicitudes CoAP (GET, PUT, POST, DELETE), y la posibilidad de establecer un listener para manejar los datos entrantes a través del método setDataMessageListener(IDataMessageListener listener).

Objetivos Principales:
Permitir el procesamiento de mensajes PUT con datos en formato JSON desde el CDA.

Convertir los datos JSON recibidos en objetos SystemPerformanceData o SensorData, según corresponda.

Delegar el manejo de los datos al componente DeviceDataManager a través del IDataMessageListener.

Ofrecer respuestas apropiadas al cliente CoAP utilizando códigos estándar (CHANGED, BAD_REQUEST, etc.).


3
Se creó una nueva clase Java denominada GetActuatorCommandResourceHandler dentro del paquete programmingtheiot.gda.connection.handlers. Esta clase fue desarrollada con el objetivo de implementar un recurso CoAP observable que permita al GDA notificar al CDA comandos de actuación utilizando la especificación OBSERVE del protocolo CoAP.

El desarrollo de esta clase se basó en la clase GenericCoapResourceHandler, sirviendo como plantilla para mantener consistencia en la arquitectura del sistema. Entre las principales características y objetivos de esta implementación se encuentran:

Soporte para observabilidad CoAP: La clase extiende de CoapResource y se declara como observable mediante super.setObservable(true), permitiendo que el CDA se suscriba y reciba actualizaciones automáticas cuando cambien los datos del actuador.

Implementación de la interfaz IActuatorDataListener: Se sobrescribió el método onActuatorDataUpdate(ActuatorData data), el cual permite recibir datos actualizados desde el DeviceDataManager y notificar a todos los clientes conectados mediante super.changed().

Manejo de solicitudes GET: Se sobrescribió el método handleGET(CoapExchange context) para responder a las solicitudes GET del CDA. Esta implementación convierte los datos del actuador almacenados localmente en formato JSON y los envía al cliente como respuesta utilizando context.respond(ResponseCode.CONTENT, jsonData).

Inicialización de atributos clave: Se agregó una instancia de ActuatorData como variable de clase para almacenar y gestionar los datos de comando del actuador que serán enviados a los clientes observadores.

El propósito de estos cambios es establecer un mecanismo de comunicación reactivo entre el GDA y el CDA, facilitando la transmisión automática de comandos de actuación a través de CoAP una vez que el CDA se suscriba al recurso correspondiente.

Esta implementación sienta las bases para pruebas futuras e integración de funcionalidades más avanzadas relacionadas con el control de actuadores en entornos IoT distribuidos.


4

Se realizaron actualizaciones importantes en las clases CoapServerGateway y DeviceDataManager con el objetivo de mejorar la flexibilidad, modularidad y capacidad de integración del servidor CoAP en la infraestructura del proyecto. A continuación se describen los cambios más relevantes:

1. CoapServerGateway
Inicialización flexible de recursos: Se implementó la funcionalidad para permitir la creación e integración tanto de manejadores de recursos creados internamente como de aquellos proporcionados externamente (por ejemplo, desde DeviceDataManager).

Método initServer() implementado: Se agregó lógica para inicializar una instancia de CoapServer, invocar el método initDefaultResources() y registrar los recursos por defecto.

Método addResource() implementado: Se desarrolló un mecanismo para agregar dinámicamente recursos al árbol jerárquico de recursos del servidor, soportando nombres compuestos como "PIOT/ConstrainedDevice/SystemPerfMsg".

Manejo jerárquico de recursos CoAP: Se creó una estructura tipo árbol para representar recursos jerárquicos, lo que permite la expansión del servidor con nuevos nodos en tiempo de ejecución.

2. DeviceDataManager
Soporte para IActuatorDataListener: Se agregó la capacidad de registrar un listener del tipo IActuatorDataListener mediante el método setActuatorDataListener(String name, IActuatorDataListener listener).

Integración con lógica de análisis: Se modificó el método handleIncomingDataAnalysis(...) para llamar al listener registrado en caso de recibir datos de actuador, permitiendo una mejor separación de responsabilidades y la activación de respuestas automáticas.

3. Pruebas de Integración
Se implementó una prueba de integración (CoapServerGatewayTest) que valida la creación, inicialización y descubrimiento de recursos del servidor CoAP.

La prueba simula un entorno real utilizando el cliente CoAP de Californium, verificando la correcta visibilidad y respuesta de los recursos expuestos.


### Code Repository and Branch



URL: https://github.com/BraisFernandezCaride/programmingtheiotjava/tree/lab08.4




### Integration Tests Executed

- CoapClientToServerConnectorTest.java



EOF.
