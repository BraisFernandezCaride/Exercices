# Constrained Device Application (Connected Devices)

## Lab Module 09


### Description

1

Nuevo módulo CoapClientConnnector, cuya clase homónima implementa la interfaz IRequestResponseClient. Esta clase permite establecer comunicación con servidores CoAP utilizando la librería aiocoap, la cual se seleccionó entre las alternativas disponibles por su enfoque asincrónico y soporte actualizado.

Objetivos y funcionalidades implementadas
Implementación del conector CoAP:
Se creó la clase CoapClientConnector, responsable de gestionar solicitudes y respuestas CoAP desde el cliente, conforme a los métodos definidos en la interfaz IRequestResponseClient.

Uso de la librería aiocoap:
Se optó por utilizar la librería aiocoap por sus capacidades modernas de programación asincrónica con asyncio. Para ello, se añadieron los imports correspondientes y se estructuró la inicialización del cliente mediante Context.create_client_context() en un método asincrónico llamado _initClientContext.

Inicialización y configuración:
El constructor del CoapClientConnector extrae los parámetros de configuración (host, puerto) desde ConfigUtil, permitiendo una inicialización flexible del cliente y preparación del URI base para futuras solicitudes.

Métodos asincrónicos y asyncio:
Para permitir el uso de la funcionalidad de observación CoAP y solicitudes asincrónicas, se implementó el método _initClient() que invoca su equivalente asincrónico mediante asyncio.get_event_loop().run_until_complete(...).

Preparación para solicitudes y observación:
Se definieron los métodos esperados por la interfaz (sendGetRequest, sendPostRequeest, startObserver, etc.), los cuales por el momento solo registran su invocación en el log y retornan False. Esta implementación inicial servirá de base para desarrollos posteriores.

Gestión de recursos CoAP:
Se implementó el método auxiliar _createResourcePath() para construir dinámicamente las URIs a partir de instancias de ResourceNameEnum y parámetros adicionales.

Integración con DeviceDataManager:
En la clase DeviceDataManager, se añadió soporte condicional para CoAP mediante una nueva propiedad enableCoapClient, la cual se carga desde el archivo de configuración PiotConfig.props. Si está habilitada, se instancia y conecta automáticamente el CoapClientConnector, permitiendo su uso en el flujo de datos del dispositivo.

2


Soporte para solicitudes GET (CON y NON):

Se agregó el método sendGetRequest() con parámetros configurables para el tipo de mensaje (confirmado o no confirmado), nombre del recurso y timeout.

Se implementó este método usando las dos bibliotecas mencionadas:

CoAPthon3: Envío síncrono de solicitudes GET con manejo del token y tipo de mensaje.

aiocoap: Envío asincrónico de solicitudes GET utilizando asyncio para un flujo de operación no bloqueante.

Manejo de respuestas GET:

Se implementó el método _onGetResponse() para procesar las respuestas obtenidas de las solicitudes GET.

Este método permite la conversión de datos JSON recibidos a objetos ActuatorData, los cuales luego son procesados por dataMsgListener si está disponible.

Soporte para descubrimiento de recursos (Resource Discovery):

Se implementó el método sendDiscoveryRequest(), el cual realiza una solicitud GET al recurso estándar .well-known/core, permitiendo descubrir todos los recursos expuestos por el servidor CoAP.

Esta función reutiliza el método sendGetRequest() para mantener una estructura coherente de llamadas.


3

Se implementó el método sendPutRequest, el cual admite tanto mensajes confirmables (CON) como no confirmables (NON), y permite especificar el recurso objetivo, el contenido del mensaje y un tiempo de espera personalizado. La lógica de envío asincrónica se gestiona a través de _handlePutRequest, encargada de construir el mensaje CoAP con el tipo y carga útil adecuados, enviarlo al servidor y procesar la respuesta.

Adicionalmente, se definió el método _onPutResponse como función de retorno para manejar y registrar las respuestas del servidor luego de emitir una solicitud PUT.

Estos cambios mejoran la capacidad de interacción del cliente CoAP con el servidor, permitiendo una comunicación bidireccional más completa y ajustada a las operaciones típicas de una arquitectura IoT.

4

Añadir soporte a solicitudes POST, tanto en modalidad confirmada (CON) como no confirmada (NON).Esta implementación se realizó mediante el uso de las definiciones de método existentes dentro de la interfaz IRequestResponseHandler.

Para lograrlo, se integró compatibilidad con dos bibliotecas de CoAP en Python: CoAPthon3 y aiocoap, permitiendo así que el conector pueda realizar operaciones POST usando cualquiera de las dos tecnologías según sea requerido.

Se añadió el método sendPostRequest(...) que construye la ruta del recurso y prepara el payload correspondiente. Dependiendo de la configuración, el mensaje se envía como CON o NON. La implementación incluye el uso de métodos asincrónicos cuando se utiliza aiocoap, aprovechando asyncio para manejar la solicitud y recibir la respuesta del servidor.

Además, se incorporó una función de callback llamada _onPostResponse(...) para procesar las respuestas del servidor a las solicitudes POST. Esta función verifica si la respuesta es válida y registra el contenido recibido para facilitar la depuración o análisis posterior.

El objetivo principal de esta modificación es ampliar la funcionalidad del cliente CoAP, permitiéndole almacenar información (como datos de sensores) en el servidor mediante el método POST, en escenarios donde PUT no sea apropiado o permitido.


5

El objetivo era incorporar soporte al envío de solicitudes Delete. Esto permite al cliente CoAP enviar datos al servidor utilizando este método HTTP, en formato confirmado (CON) o no confirmado (NON), según se requiera.

Se implementó el método sendDeleteRequest(...), que construye dinámicamente la ruta del recurso y realiza una solicitud Delete asincrónica utilizando la biblioteca aiocoap. Para gestionar esta operación, se añadió una función auxiliar _handleDeleteRequest(...), encargada de crear el mensaje, codificar el payload y manejar la respuesta del servidor de forma asincrónica.

También se incluyó un método de callback _onDeleteResponse(...) que permite procesar y registrar la respuesta recibida, facilitando así el monitoreo y trazabilidad de las interacciones POST.

6


Incorporar soporte a solicitudes OBSERVE en el protocolo CoAP, permitiendo al cliente observar recursos y recibir actualizaciones en tiempo real cuando ocurren cambios en dichos recursos.

Objetivos y funcionalidades implementadas:
Soporte para observación CoAP:

Se agregaron los métodos startObserver() y stopObserver() para iniciar y detener la observación de recursos CoAP.

Estos métodos permiten a los clientes suscribirse a cambios en recursos específicos y reaccionar automáticamente a las actualizaciones enviadas por el servidor.

Compatibilidad con múltiples bibliotecas:

Gestión de observaciones activas:

Se introdujo un mecanismo de seguimiento interno para las solicitudes de observación activas mediante un diccionario que asocia el recurso observado con su manejador de callback correspondiente.

Este sistema garantiza la correcta asociación entre los datos recibidos y los componentes de la aplicación encargados de su procesamiento.

Manejo de respuestas observadas:

Se desarrolló una clase auxiliar HandleActuatorEvent para procesar respuestas de tipo ActuatorData, utilizando un listener (IDataMessageListener) para canalizar los datos observados hacia los componentes pertinentes del sistema.

Implementación asincrónica con aiocoap:

Se implementó el manejo de observaciones utilizando programación asíncrona con asyncio, lo cual permite mejorar el rendimiento y la eficiencia en escenarios con múltiples recursos observados.

Cancelación explícita de observaciones:

Se garantiza que las observaciones sean canceladas correctamente por el cliente utilizando stopObserver(), evitando tráfico innecesario y posibles errores por intentos del servidor de seguir enviando datos a clientes no interesados.




### Code Repository and Branch



URL: https://github.com/BraisFernandezCaride/programmingtheiot/tree/lab09.6



### Integration Tests Executed


- CoapClientConnectorTest.py
  

EOF.
