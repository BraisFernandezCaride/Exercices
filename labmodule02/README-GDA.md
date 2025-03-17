# Gateway Device Application (Connected Devices)

## Lab Module 02



### Description

NOTE: Include two full paragraphs describing your implementation approach by answering the questions listed below.

What does your implementation do? 

How does your implementation work?

1- Creación de un nuevo paquete y clase Java:

Se crea un paquete app dentro de la carpeta programmingtheiot\gda.
Se crea la clase GatewayDeviceApp.

Manejo de logs:
Se importa el marco de trabajo java.util.logging para gestionar logs, específicamente las clases Level y Logger.

Constructor:
Se agrega un constructor public GatewayDeviceApp(String[] args) que acepta un arreglo de argumentos de tipo String.

Métodos principales:
stopApp(int code): Este método detiene la aplicación y registra un mensaje informativo en los logs, indicando que la aplicación fue detenida. Si ocurre algún error, se captura con un bloque try/catch y se registra en los logs.

startApp(): Inicia la aplicación y también registra un mensaje informativo en los logs. Si ocurre un error, se captura en un bloque try/catch, se registra el error y luego se llama a stopApp(-1) para detener la aplicación con un código de error.

initConfig(String fileName): Este método se utiliza para inicializar la configuración. Actualmente, solo registra un mensaje en los logs, indicando que fue llamado, pero no realiza ninguna acción adicional.

parseArgs(String[] args): Este método se encarga de procesar los argumentos recibidos. Actualmente, simplemente registra un mensaje en los logs y llama a initConfig(null).

Método main:
Se agrega el método main para ejecutar la clase como una aplicación. Este método crea una instancia de GatewayDeviceApp, llama al método startApp(), espera 65 segundos y luego llama a stopApp(0) para detener la aplicación con un código de salida exitoso.

Pruebas:
Se deben ejecutar pruebas unitarias e integradas para asegurar que el código funcione correctamente. Se especifica la ejecución de pruebas como ConfigUtilTest y GatewayDeviceAppTest.



2- Creación del paquete y clase:

Se crea un paquete system dentro de la carpeta programmingtheiot\gda para alojar el nuevo módulo.
Se crea la clase SystemPerformanceManager.
Manejo de logs:

Se importa el marco de trabajo java.util.logging para gestionar los logs, específicamente las clases Level y Logger.
Se declara una instancia estática de logger para la clase.
Variables de clase:

Se define una variable de clase pollRate con un valor por defecto de ConfigConst.DEFAULT_POLL_CYCLES. Esta variable se usará para gestionar la tasa de sondeo del sistema.
Constructor:

El constructor public SystemPerformanceManager() establece el valor de la variable pollRate a partir de la configuración cargada por ConfigUtil. Si el valor es menor o igual a cero, se asigna el valor por defecto.
Métodos principales:

startManager(): Inicia el administrador de rendimiento del sistema y registra un mensaje informativo en los logs indicando que el administrador ha comenzado.
stopManager(): Detiene el administrador de rendimiento y registra un mensaje informativo en los logs indicando que el administrador ha sido detenido.
Pruebas:

Se debe ejecutar una prueba de integración llamada SystemPerformanceManagerTest, la cual verifica que los métodos startManager() y stopManager() funcionen correctamente y generen los logs esperados.



3- Creación del paquete y clase:

Se crea un paquete system dentro de la carpeta programmingtheiot\gda para alojar el nuevo módulo.
Se crea la clase SystemPerformanceManager.

Manejo de logs:
Se importa el marco de trabajo java.util.logging para gestionar los logs, específicamente las clases Level y Logger.
Se declara una instancia estática de logger para la clase.

Variables de clase:
Se define una variable de clase pollRate con un valor por defecto de ConfigConst.DEFAULT_POLL_CYCLES. Esta variable se usará para gestionar la tasa de sondeo del sistema.

Constructor:
El constructor public SystemPerformanceManager() establece el valor de la variable pollRate a partir de la configuración cargada por ConfigUtil. Si el valor es menor o igual a cero, se asigna el valor por defecto.

Métodos principales:
startManager(): Inicia el administrador de rendimiento del sistema y registra un mensaje informativo en los logs indicando que el administrador ha comenzado.
stopManager(): Detiene el administrador de rendimiento y registra un mensaje informativo en los logs indicando que el administrador ha sido detenido.

Pruebas:
Se debe ejecutar una prueba de integración llamada SystemPerformanceManagerTest, la cual verifica que los métodos startManager() y stopManager() funcionen correctamente y generen los logs esperados.

Finalidad de las modificaciones:
La finalidad de esta modificación es crear un módulo que gestione el rendimiento del sistema en términos de la tasa de sondeo o polling. Este módulo permite iniciar y detener el proceso, además de registrar las acciones en los logs para un fácil monitoreo.
Se establece un mecanismo de configuración para que la tasa de sondeo pueda ajustarse dinámicamente a partir de un archivo de configuración.


4- Se crea una clase java llamada BaseSystemUtilTask

Se añaden dos variables de clase:
name: de tipo String, que por defecto tiene el valor ConfigConst.NOT_SET.
typeID: de tipo int, que por defecto tiene el valor ConfigConst.DEFAULT_TYPE.

Constructor:
Se agrega un constructor public BaseSystemUtilTask(String name, int typeID) que acepta dos parámetros: name (nombre de la tarea) y typeID (ID de tipo de tarea). Si name no es null, se asigna a la variable this.name; typeID siempre se asigna a la variable this.typeID.

Se agregan dos métodos getter:
getName(): Retorna el valor de la variable name.
getTypeID(): Retorna el valor de la variable typeID.

Método abstracto:
Se define un método abstracto getTelemetryValue() que debe ser implementado por las subclases. Este método tiene como propósito devolver el valor de la utilización (utilization value) del sistema, pero su implementación específica depende de la subclase que lo herede.

Finalidad de las modificaciones:
La finalidad de esta modificación es proporcionar una clase base con propiedades y métodos comunes que otras clases más específicas puedan heredar. El método getTelemetryValue() es abstracto, lo que obliga a las subclases a definir cómo obtener el valor de la utilización correspondiente, permitiendo una flexibilidad en la implementación de tareas relacionadas con la utilización del sistema.


5- Creación de la clase SystemCpuUtilTask:

Se crea la clase SystemCpuUtilTask dentro del paquete programmingtheiot.gda.system.
Esta clase extiende BaseSystemUtilTask, por lo que hereda los métodos y propiedades de la clase base.
Importación de bibliotecas:

Se importan las clases necesarias para obtener la información sobre la utilización de la CPU:
ManagementFactory y OperatingSystemMXBean para obtener información sobre el sistema operativo.
Logger (opcional) para registrar información de depuración.
ConfigConst para acceder a las constantes de configuración.
Sobrescritura del método getTelemetryValue():

Se sobrescribe el método getTelemetryValue() que ahora obtiene la utilización de la CPU utilizando ManagementFactory.getOperatingSystemMXBean().getSystemLoadAverage(). Este método devuelve el valor promedio de carga del sistema, que se convierte a tipo float y se devuelve como la utilización de la CPU.
Implementación del método getTelemetryValue()

Constructor:
Se añade el constructor SystemCpuUtilTask(), que llama al constructor de la clase base BaseSystemUtilTask con valores por defecto para name y typeID.

Pruebas de unidad: 
Se debe ejecutar una prueba unitaria en ./src/test/java/programmingtheiot/part01/unit llamada SystemCpuUtilTaskTest. Esta prueba verificará el correcto funcionamiento del método getTelemetryValue().


6- Creación de la clase SystemMemUtilTask:

Se crea la clase SystemMemUtilTask dentro del paquete programmingtheiot.gda.system.
Esta clase extiende de BaseSystemUtilTask, heredando así los métodos y propiedades de la clase base.

Importación de bibliotecas:
Se importan las clases necesarias para obtener la información sobre la memoria:
ManagementFactory y MemoryUsage para obtener detalles sobre el uso de la memoria en la JVM.
Logger (opcional) para registrar información de depuración.
ConfigConst para acceder a las constantes de configuración.

Sobrescritura del método getTelemetryValue():
Se sobrescribe el método getTelemetryValue() que ahora obtiene la utilización de la memoria de la JVM. Para ello, se utiliza ManagementFactory.getMemoryMXBean().getHeapMemoryUsage() para obtener el uso de memoria del heap.
Se calcula el porcentaje de memoria utilizada dividiendo la memoria usada (memUsage.getUsed()) entre la memoria máxima (memUsage.getMax()), multiplicado por 100 para obtener un valor en porcentaje.
Se registra el valor calculado utilizando el logger y luego se retorna el valor como un float.

Constructor:
Se añade el constructor SystemMemUtilTask(), que llama al constructor de la clase base BaseSystemUtilTask con valores por defecto para name y typeID.

Pruebas de unidad:
Se debe ejecutar una prueba unitaria en ./src/test/java/programmingtheiot/part01/unit llamada SystemMemUtilTaskTest. Esta prueba verificará el correcto funcionamiento del método getTelemetryValue(). Los valores de la utilización de la memoria deben estar entre 0.0% y 100.0%.


7- Importación de bibliotecas necesarias: Se importan clases para trabajar con programación concurrente, específicamente para crear un servicio de ejecución programada. Las clases Executors, ScheduledExecutorService, ScheduledFuture, y TimeUnit permiten ejecutar tareas de forma periódica.

Definición de variables de instancia: Se añaden miembros en la clase SystemPerformanceManager para:

schedExecSvc: el servicio de ejecución programada.
sysCpuUtilTask y sysMemUtilTask: instancias de las tareas de monitoreo de CPU y memoria, respectivamente.
taskRunner: una tarea que invoca el método handleTelemetry() de forma programada.
isStarted: una bandera que indica si el administrador ya está en ejecución.

Método handleTelemetry(): En este método se invocan los métodos getTelemetryValue() de las clases SystemCpuUtilTask y SystemMemUtilTask para obtener los valores de utilización de la CPU y la memoria. Luego, estos valores se registran utilizando un logger. Es importante notar que el uso de los métodos puede no ser compatible con todos los sistemas operativos, por lo que puede ser necesario manejar valores de retorno negativos si no se puede obtener la información.

Configuración en el constructor de SystemPerformanceManager:
Se inicializan las instancias de SystemCpuUtilTask y SystemMemUtilTask.
Se configura el servicio de ejecución programada (schedExecSvc) para que ejecute la tarea de monitoreo a intervalos regulares definidos por el pollRate.

Método startManager(): En este método se verifica si el gestor ya está en ejecución. Si no lo está, se inicia el servicio programado para ejecutar la tarea taskRunner a intervalos regulares.

Método stopManager(): Este método detiene el servicio programado y marca al gestor como detenido.

Pruebas de integración:
El trabajo realizado en este ejercicio está destinado a ser probado en un escenario de integración. Específicamente, la prueba de integración se encuentra en ./src/test/java/programmingtheiot/part01/integration y se debe ejecutar la prueba GatewayDeviceAppTest.

### Code Repository and Branch

NOTE: Be sure to include the branch (e.g. https://github.com/programming-the-iot/python-components/tree/alpha001).

URL: https://github.com/BraisFernandezCaride/programmingtheiotjava/tree/labmodule02



### Unit Tests Executed

NOTE: TA's will execute your unit tests. You only need to list each test case below
(e.g. ConfigUtilTest, DataUtilTest, etc). Be sure to include all previous tests, too,
since you need to ensure you haven't introduced regressions.

- ConfigUtilTest.java
- SystemCpuUtilTaskTest.java
- SystemMemUtilTaskTest.java

### Integration Tests Executed

NOTE: TA's will execute most of your integration tests using their own environment, with
some exceptions (such as your cloud connectivity tests). In such cases, they'll review
your code to ensure it's correct. As for the tests you execute, you only need to list each
test case below (e.g. SensorSimAdapterManagerTest, DeviceDataManagerTest, etc.)

- GatewayDeviceAppTest.java
- SystemPerformanceManagerTest.java


EOF.
