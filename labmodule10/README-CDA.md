# Constrained Device Application (Connected Devices)

## Lab Module 10

### Descripción

#### 1. Conexiones seguras mediante TLS en `MqttClientConnector`

La clase `MqttClientConnector` fue modificada para habilitar el soporte de conexiones cifradas TLS con el broker MQTT. Para lograrlo, se agregaron nuevas propiedades al constructor, las cuales leen desde el archivo de configuración si TLS está activado (`ENABLE_CRYPT_KEY`) y cuál es la ruta del archivo de certificado (`CERT_FILE_KEY`).

Dentro del método `connectClient()`, se añadió lógica para detectar si TLS debe usarse. En ese caso, se ajusta el puerto del broker y se configura la conexión segura utilizando la librería estándar `ssl`.

Esta mejora incrementa la seguridad en la comunicación con el broker sin interferir con la funcionalidad existente cuando TLS no está habilitado.

**Pruebas realizadas:**

- `MqttClientConnectorTest.py` con la configuración por defecto (sin TLS), confirmando que la funcionalidad base se mantiene estable.

---

#### 2. Manejo de comandos `ActuatorData` en `DeviceDataManager`

La interfaz `IDataMessageListener` y la clase `DeviceDataManager` fueron extendidas para permitir la recepción y ejecución de comandos tipo `ActuatorData` enviados desde el GDA (Gateway Device Application).

Se definió un nuevo método `handleActuatorCommandMessage()` como parte del contrato de la interfaz. Esta función fue implementada en `DeviceDataManager`, donde los mensajes se validan y se reenvían al adaptador de actuadores (`actuatorAdapterMgr`) para su ejecución.

Con esta adición, el CDA es ahora capaz de reaccionar a comandos de control remoto, mejorando la capacidad de automatización del sistema.

**Pruebas realizadas:**

- `DeviceDataManagerCallbackTest.py` sin dependencias de red, confirmando la correcta recepción y manejo de comandos de actuación.

---

#### 3. Suscripción y manejo de comandos MQTT en `MqttClientConnector`

Se incorporó la capacidad de suscribirse a tópicos de comandos enviados desde el GDA mediante MQTT. Esta funcionalidad permite recibir instrucciones de actuación y redirigirlas hacia un listener registrado.

**Cambios principales:**

- Suscripción al tópico de comandos dentro del método `onConnect`.
- Implementación de `onActuatorCommandMessage()` para transformar mensajes MQTT en objetos `ActuatorData`.
- Inclusión del método `setDataMessageListener()` para establecer el listener adecuado.
- Eliminación de un posible bloqueo en `publishMessage()` comentando `msgInfo.wait_for_publish()`.

Este conjunto de mejoras facilita una interacción en tiempo real entre el GDA y el CDA mediante mensajes MQTT.

---

#### 4. Transmisión de datos desde `DeviceDataManager` hacia el GDA

El envío de datos al GDA se habilitó desde `DeviceDataManager`, permitiendo que información tanto de sensores como de rendimiento del sistema llegue al GDA utilizando MQTT o CoAP, según lo configurado.

**Actualizaciones destacadas:**

- Creación del método `_handleUpstreamTransmission()` encargado de transmitir los datos en formato JSON al GDA utilizando el protocolo definido.
- Integración de este nuevo método dentro de `handleSensorMessage()` y `handleSystemPerformanceMessage()`, centralizando la lógica de transmisión.
- Análisis continuo de datos de temperatura en `_handleSensorDataAnalysis()`, generando comandos de actuación cuando se superan umbrales definidos.

**Pruebas realizadas:**

- `testDeviceDataMgrTimedIntegration.py` con datos del emulador SenseHAT y un broker MQTT local, validando tanto la transmisión como la activación de eventos automáticos de actuación.

---

### Repositorio y Rama

**URL:**  
[https://github.com/BraisFernandezCaride/programmingtheiot/tree/lab10.4](https://github.com/BraisFernandezCaride/programmingtheiot/tree/lab10.4)

---

### Pruebas de Integración Ejecutadas

- `DeviceDataManagerWithCommsTest.py`  
- `DeviceDataManagerCallbackTest.py`  
- `MqttClientConnectorTest.py`

---

EOF.
