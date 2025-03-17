# Gateway Device Application (Connected Devices)

## Lab Module 05

### Description

1-

BaseIotData
La clase BaseIotData actúa como la base para todas las clases de contenedores de datos dentro del proyecto. Define las propiedades comunes que deben ser compartidas por todas las subclases, como el nombre, el tipo de identificación, el código de estado y la marca de tiempo. Estas propiedades permiten que los datos se puedan manejar de manera uniforme en todo el sistema y asegurar que las clases hijas, como SensorData, ActuatorData y SystemPerformanceData, sigan una estructura coherente.

ActuatorData
La clase ActuatorData representa los datos asociados con los actuadores en el sistema. Cada objeto de esta clase contiene información como un comando (valor entero), un valor (número de tipo flotante), el estado del actuador (como cadena de texto) y un indicador (booleano) que indica si el actuador es una respuesta a una acción previa. La clase hereda de BaseIotData, lo que garantiza la consistencia en la estructura de datos y permite la serialización y deserialización de objetos en formato JSON. Los métodos getter y setter son utilizados para acceder y modificar los valores de estas propiedades, asegurando que los datos sean validados y actualizados de manera correcta.

SensorData
La clase SensorData representa los datos asociados con los sensores del sistema. Similar a la clase ActuatorData, contiene un valor (número flotante) que representa la medición obtenida por el sensor. Esta clase también hereda de BaseIotData, asegurando que las propiedades como el nombre, el tipo y la marca de tiempo sean gestionadas de forma coherente. Los métodos getter y setter permiten acceder y modificar el valor del sensor, y se asegura que los datos sean validados y actualizados en el momento en que se cambian.

SystemPerformanceData
La clase SystemPerformanceData se utiliza para representar el rendimiento del sistema, con propiedades que incluyen el uso de CPU, el uso de memoria y el uso de disco. Al igual que las otras clases, hereda de BaseIotData, pero en este caso se enfoca en los datos relacionados con la utilización de recursos del sistema. Se implementan métodos getter y setter para cada propiedad, y se incluye validación para asegurar que los valores estén dentro de los rangos permitidos. La clase tiene un propósito clave en el monitoreo y la gestión del rendimiento del sistema, y sus datos son esenciales para garantizar que los recursos se gestionen de manera eficiente.

Pruebas(unidad):
- ActuatorDataTest.java
- SensorDataTest.java
- SystemPerformanceDataTest.java


2-


Actualización de SystemPerformanceManager
La clase SystemPerformanceManager se encarga de gestionar la recolección y almacenamiento de los datos de rendimiento del sistema, incluyendo la utilización de la CPU y la memoria. Se ha realizado una serie de modificaciones para mejorar su funcionalidad y permitir una mejor integración con otros sistemas:

Variables de Instancia: Se han añadido dos nuevas variables a la clase SystemPerformanceManager:

locationID:
Un identificador de ubicación que se recupera desde el archivo de configuración. Este identificador ayuda a asociar los datos de rendimiento del sistema con una ubicación específica.
dataMsgListener:
Un listener de tipo IDataMessageListener que, en caso de estar configurado, será utilizado para invocar un callback cada vez que se cree una nueva instancia de SystemPerformanceData.
Estas variables son fundamentales para almacenar y transmitir la información relevante del rendimiento del sistema.

Recuperación del ID de Ubicación: En el constructor de SystemPerformanceManager, se ha añadido código para obtener el locationID desde el archivo de configuración. Esto asegura que los datos recopilados estén correctamente etiquetados con la ubicación correspondiente.

Manejo de Telemetría: En el método handleTelemetry(), se han actualizado las instrucciones para almacenar los valores de utilización de CPU y memoria en una nueva instancia de SystemPerformanceData. La información de rendimiento se recoge mediante tareas específicas (sysCpuUtilTask y sysMemUtilTask), y luego se asigna a las propiedades correspondientes de SystemPerformanceData.

Después de crear el objeto SystemPerformanceData y configurar sus propiedades, si el listener dataMsgListener está configurado, se invoca el método handleSystemPerformanceMessage(), pasando los datos de rendimiento del sistema. Esto permite que los datos sean procesados o enviados a otros sistemas, como una base de datos o una interfaz de usuario, para su visualización o almacenamiento.

Configuración del Listener:
Se ha actualizado el método setDataMessageListener() para permitir la configuración del listener. Este método asegura que, si se proporciona un listener válido, se configure correctamente para ser utilizado en futuras invocaciones dentro de handleTelemetry().

Tarea para Utilización de Disco:
Se ha añadido una nueva tarea para realizar un seguimiento de la utilización del disco, similar a cómo se gestionan las tareas de CPU y memoria. Esta tarea permite obtener información sobre el uso del espacio de almacenamiento en disco, y sus valores también se incorporan en la clase SystemPerformanceData. Aunque la implementación puede variar dependiendo de los detalles del sistema, el seguimiento de la utilización del disco agrega otra capa de información importante para el monitoreo del rendimiento del sistema.

Objetivo:
El objetivo principal de estas modificaciones es asegurar que SystemPerformanceManager pueda gestionar de manera eficiente los datos de rendimiento del sistema, incluyendo CPU, memoria y disco. La creación y actualización de un objeto SystemPerformanceData permite centralizar la información relacionada con el rendimiento en un solo lugar, mientras que la implementación de un callback mediante IDataMessageListener facilita la integración con otros módulos o sistemas, que pueden procesar esta información de manera asíncrona.

Prueba(integración):
- SystemPerformanceManagerTest.java


3-

La clase DataUtil se encarga de la conversión de objetos Java a JSON y viceversa, utilizando la biblioteca Gson. Su principal propósito es facilitar la serialización y deserialización de objetos como ActuatorData, SensorData y SystemPerformanceData, permitiendo su fácil intercambio entre diferentes partes del sistema o con servicios externos que trabajen con JSON.

Objetivo y Funcionalidad
La clase DataUtil ofrece métodos públicos para convertir objetos de los tipos mencionados a formato JSON, y también para convertir cadenas JSON de vuelta a instancias de esos objetos. Esto es útil cuando se requiere almacenar, transmitir o procesar estos datos en sistemas que utilizan JSON como formato de intercambio.

Metodología
Conversión a JSON: Utilizando Gson, los objetos de los tipos ActuatorData, SensorData y SystemPerformanceData se convierten en cadenas JSON a través de los métodos correspondientes. Este proceso asegura que los datos puedan ser fácilmente exportados o enviados a otros sistemas.

Deserialización desde JSON: Los métodos de deserialización toman una cadena JSON y la convierten en un objeto correspondiente (por ejemplo, de JSON a ActuatorData). Este proceso permite que los datos se puedan recibir desde otras fuentes y ser utiliizados dentro del sistema.

Patrón Singleton
La clase DataUtil sigue el patrón Singleton, asegurando que solo exista una instancia de la clase a lo largo del ciclo de vida del sistema, lo que facilita el manejo centralizado de las conversiones entre objetos y JSON.

4-

La clase DeviceDataManager es el núcleo del sistema de gestión de dispositivos, encargada de manejar la lógica central para la recopilación y procesamiento de datos dentro de la aplicación. Se encarga de la administración de conexiones, la gestión de la configuración y la integración con otros componentes del sistema, como la gestión de dispositivos y la monitorización del rendimiento del sistema.

Objetivo y Funcionalidad
El propósito principal de DeviceDataManager es orquestar y controlar las interacciones entre diferentes componentes, como los sistemas de rendimiento y los clientes de comunicación (MQTT, CoAP, Cloud, etc.). También implementa el manejo de mensajes recibidos desde diferentes fuentes y transmite datos hacia otros sistemas cuando sea necesario.

Metodología:
Inicialización de Conexiones: En el constructor de la clase, se utiliza la utilidad ConfigUtil para leer las configuraciones de habilitación de conexiones desde un archivo de configuración. Dependiendo de las configuraciones, se inicializan diferentes servicios de comunicación y de gestión de dispositivos.

Métodos startManager() y stopManager(): Estos métodos se encargan de iniciar y detener los diversos servicios y conexiones de forma controlada. En el caso de conexiones que requieren estados (como MQTT o CoAP), se realizan las comprobaciones necesarias y se gestionan las conexiones según corresponda.

Interfaz IDataMessageListener: La clase implementa esta interfaz para recibir y procesar mensajes de datos de otros sistemas. Incluye métodos específicos para manejar mensajes de tipo ActuatorData, SensorData y SystemPerformanceData. Además, cada uno de estos métodos de manejo de datos verifica los errores y realiza acciones como el análisis de los datos o la preparación para su transmisión.

Estructura de la Clase:
Variables de Clase: Se definen varias variables de instancia para controlar los distintos servicios habilitados, como MQTT, CoAP, Cloud y persistencia. También se gestionan instancias de clases de conexión y de administración del rendimiento del sistema.

Métodos de Configuración:

El constructor obtiene los valores de configuración y configura las instancias de los servicios correspondientes.
El método initManager() se encarga de instanciar los objetos de gestión y conexión según las configuraciones definidas.
Procesamiento de Datos: A través de la implementación de IDataMessageListener, la clase recibe diferentes tipos de mensajes (por ejemplo, comandos de actuadores o datos de sensores), los procesa y, si es necesario, los transmite a otros sistemas utilizando los mecanismos de comunicación adecuados.

Flujo de Trabajo y Comunicación:
Recopilación de Datos: Los datos de sensores, actuadores y del rendimiento del sistema se gestionan a través de sus respectivos métodos y se preparan para su posterior análisis o transmisión.

Transmisión de Datos: Los datos procesados se preparan para ser transmitidos a otros sistemas en el futuro, utilizando las funciones de comunicación que se implementarán en módulos posteriores.

Prueba(integración):
- DeviceDataManagerNoCommsTest.java


5-

Este proceso tiene como objetivo integrar la gestión de datos de dispositivos en la aplicación GatewayDeviceApp mediante el uso de DeviceDataManager. A continuación, se describen los pasos necesarios para lograr esta integración.

1. Crear una instancia de DeviceDataManager
Dentro de la clase GatewayDeviceApp, se debe crear una instancia de DeviceDataManager. Esta instancia será utilizada para gestionar la administración de los dispositivos en la aplicación.

Ubicación: Debe declararse como una variable de clase en GatewayDeviceApp.
2. Modificar el Constructor de GatewayDeviceApp
En el constructor de GatewayDeviceApp, se debe inicializar la instancia de DeviceDataManager. Esta instancia será responsable de manejar la recopilación y procesamiento de los datos de los dispositivos.

Acción: Asegúrate de que la instancia de DeviceDataManager se cree al inicio de la ejecucción de la aplicación.
3. Modificar el Método startApp()
En el método startApp(), se debe invocar el método startManager() de la instancia de DeviceDataManager. Este método es responsable de iniciar la gestión de los datos de los dispositivos cuando se inicie la aplicación.

Acción: Llama al método startManager() de DeviceDataManager dentro de startApp() para activar la gestión de datos.
4. Modificar el Método stopApp()
En el método stopApp(), se debe invocar el método stopManager() de DeviceDataManager. Esto garantizará que la gestión de los dispositivos se detenga cuando se cierre la aplicación.

Acción: Llama al método stopManager() de DeviceDataManager dentro de stopApp() para detener la gestión de datos al cerrar la aplicación.
5. Actualizar el Método startManager() en DeviceDataManager
En la clase DeviceDataManager, se debe modificar el método startManager() para que también inicie el SystemPerformanceManager. Esto se debe hacer solo si la instancia de SystemPerformanceManager está inicializada correctamente.

Acción: Asegúrate de que startManager() en DeviceDataManager inicie el SystemPerformanceManager, si está disponible.
6. Actualizar el Método stopManager() en DeviceDataManager
De manera similar, el método stopManager() de DeviceDataManager debe detener el SystemPerformanceManager cuando sea apropiado. Al igual que con el inicio, esta acción debe llevarse a cabo solo si la instancia de SystemPerformanceManager está inicializada.

Prueba(integración):
- GatewayDeviceAppTest.java




### Code Repository and Branch

NOTE: Be sure to include the branch (e.g. https://github.com/programming-the-iot/python-components/tree/alpha001).

URL: https://github.com/BraisFernandezCaride/programmingtheiotjava/tree/lab05

### UML Design Diagram(s)

NOTE: Include one or more UML designs representing your solution. It's expected each
diagram you provide will look similar to, but not the same as, its counterpart in the
book [Programming the IoT](https://learning.oreilly.com/library/view/programming-the-internet/9781492081401/).


### Unit Tests Executed

NOTE: TA's will execute your unit tests. You only need to list each test case below
(e.g. ConfigUtilTest, DataUtilTest, etc). Be sure to include all previous tests, too,
since you need to ensure you haven't introduced regressions.

- ActuatorDataTest.java
- SensorDataTest.java
- SystemPerformanceDataTest.java
  

### Integration Tests Executed

NOTE: TA's will execute most of your integration tests using their own environment, with
some exceptions (such as your cloud connectivity tests). In such cases, they'll review
your code to ensure it's correct. As for the tests you execute, you only need to list each
test case below (e.g. SensorSimAdapterManagerTest, DeviceDataManagerTest, etc.)

- SystemPerformanceManagerTest.java
- DeviceDataManagerNoCommsTest.java
- GatewayDeviceAppTest.java

EOF.
