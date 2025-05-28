# Lab Module 09

## Description

### 1. Nuevo módulo `CoapClientConnector`

Se introdujo el nuevo módulo `CoapClientConnector`, cuya clase homónima implementa la interfaz `IRequestResponseClient`. Esta clase establece comunicación con servidores CoAP mediante la librería `aiocoap`, seleccionada por su enfoque asincrónico moderno y soporte actualizado.

**Objetivos y funcionalidades implementadas:**

- **Implementación del conector CoAP:**  
  Se creó la clase `CoapClientConnector`, encargada de gestionar solicitudes y respuestas CoAP del lado del cliente, siguiendo los métodos definidos en la interfaz `IRequestResponseClient`.

- **Uso de `aiocoap`:**  
  Se eligió esta librería por su compatibilidad con programación asincrónica usando `asyncio`. Para ello, se incorporaron los `imports` necesarios y se estructuró la inicialización del cliente mediante el método asincrónico `_initClientContext`, que utiliza `Context.create_client_context()`.

- **Inicialización y configuración:**  
  El constructor extrae parámetros (host, puerto) desde `ConfigUtil`, permitiendo una configuración flexible y construcción del URI base para futuras solicitudes.

- **Métodos asincrónicos con `asyncio`:**  
  Se implementó `_initClient()` para invocar el método asincrónico de inicialización usando `asyncio.get_event_loop().run_until_complete(...)`, habilitando observación CoAP y solicitudes no bloqueantes.

- **Preparación para solicitudes y observación:**  
  Se definieron los métodos esperados por la interfaz (`sendGetRequest`, `sendPostRequest`, `startObserver`, etc.). Actualmente, estos métodos solo registran su invocación en el log y retornan `False`, sirviendo como base para futuras extensiones.

- **Gestión de recursos CoAP:**  
  Se incluyó el método auxiliar `_createResourcePath()` para construir dinámicamente URIs usando `ResourceNameEnum` y parámetros adicionales.

- **Integración con `DeviceDataManager`:**  
  Se añadió soporte condicional para CoAP mediante la propiedad `enableCoapClient`, cargada desde el archivo de configuración `PiotConfig.props`. Si está habilitada, se instancia automáticamente `CoapClientConnector`, integrándose en el flujo de datos del dispositivo.

---

### 2. Soporte para solicitudes GET (CON y NON)

- **Método `sendGetRequest(...)`:**  
  Permite configurar el tipo de mensaje (confirmado o no), nombre del recurso y timeout.

- **Implementaciones:**
  - *CoAPthon3:* Solicitudes GET síncronas con manejo de token y tipo.
  - *aiocoap:* Solicitudes GET asincrónicas usando `asyncio`.

- **Manejo de respuestas:**  
  Se desarrolló `_onGetResponse()` para procesar las respuestas. Convierte datos JSON en objetos `ActuatorData` y los canaliza al `dataMsgListener`, si está disponible.

- **Descubrimiento de recursos:**  
  El método `sendDiscoveryRequest()` realiza una solicitud GET al recurso `.well-known/core` para obtener los recursos disponibles. Reutiliza `sendGetRequest()` para mantener coherencia.

---

### 3. Soporte para solicitudes PUT

- **Método `sendPutRequest(...)`:**  
  Soporta mensajes CON y NON, permitiendo definir recurso, contenido y timeout. Utiliza `_handlePutRequest()` para construir, enviar y procesar el mensaje CoAP.

- **Respuesta a PUT:**  
  Se añadió `_onPutResponse()` como callback para manejar y registrar respuestas.

- **Resultado:**  
  Mejora la capacidad del cliente CoAP para operaciones bidireccionales típicas en arquitecturas IoT.

---

### 4. Soporte para solicitudes POST

- **Objetivo:**  
  Añadir funcionalidad POST, en modalidad CON y NON, usando la interfaz `IRequestResponseHandler`.

- **Compatibilidad:**  
  Soporte para `CoAPthon3` y `aiocoap`.

- **Método `sendPostRequest(...)`:**  
  Construye la ruta del recurso y payload. Envía el mensaje de forma asincrónica con `asyncio` si se usa `aiocoap`.

- **Callback de respuesta:**  
  `_onPostResponse(...)` valida la respuesta del servidor y registra su contenido.

- **Propósito:**  
  Permitir almacenamiento de datos (e.g. sensores) en el servidor en situaciones donde PUT no sea adecuado.

---

### 5. Soporte para solicitudes DELETE

- **Método `sendDeleteRequest(...)`:**  
  Genera dinámicamente la ruta del recurso y realiza una solicitud DELETE asincrónica con `aiocoap`.

- **Gestión de envío:**  
  `_handleDeleteRequest(...)` construye y envía el mensaje, codifica el payload y maneja la respuesta.

- **Callback:**  
  `_onDeleteResponse(...)` procesa la respuesta del servidor, registrándola para su monitoreo.

---

### 6. Soporte para OBSERVE (observación de recursos)

**Objetivos y funcionalidades:**

- **Métodos `startObserver()` y `stopObserver()`:**  
  Permiten suscribirse y cancelar observación de recursos CoAP, recibiendo actualizaciones automáticas en tiempo real.

- **Compatibilidad con múltiples bibliotecas.**

- **Gestión interna de observaciones activas:**  
  Se utilizó un diccionario que asocia recursos observados con sus callbacks para asegurar correcta gestión de datos recibidos.

- **Manejo de respuestas observadas:**  
  Se creó la clase `HandleActuatorEvent`, que canaliza los datos observados mediante un `IDataMessageListener`.

- **Implementación asincrónica:**  
  Utiliza `asyncio` para mejorar rendimiento al observar múltiples recursos.

- **Cancelación explícita:**  
  `stopObserver()` garantiza la finalización de observaciones, evitando tráfico innecesario y errores.

---

## Code Repository and Branch

**URL:**  
[https://github.com/BraisFernandezCaride/programmingtheiot/tree/lab09.6](https://github.com/BraisFernandezCaride/programmingtheiot/tree/lab09.6)

---

## Integration Tests Executed

- `CoapClientConnectorTest.py`

**EOF.**
