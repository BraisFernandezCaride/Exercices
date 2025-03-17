# Constrained Device Application (Connected Devices)

## Lab Module 02


### Description

1- Creación de la Aplicación ConstrainedDeviceApp

Se crea una nueva aplicación en Python llamada ConstrainedDeviceApp dentro del paquete ./programmingtheiot/cda/app.
Estructura del Código
Se crea el paquete app dentro de programmingtheiot\cda.
Se define la clase ConstrainedDeviceApp dentro de un nuevo módulo del mismo nombre.
Se importa el módulo logging para la generación de logs.

Métodos Implementados
startApp(): Registra un mensaje indicando que la aplicación ha sido iniciada.
stopApp(): Registra un mensaje indicando que la aplicación ha sido detenida.

Función principal (main()):
Crea una instancia de ConstrainedDeviceApp.
Llama al método startApp(), espera 65 segundos y luego ejecuta stopApp().
Permite que la aplicación pueda ejecutarse como un script independiente.
Pruebas Implementadas

Pruebas Unitarias:
Se ejecutan desde ./src/test/programmingtheiot/programmingtheiot/part01/unit con ConfigUtilTest.

Pruebas de Integración:
Se ejecutan desde ./src/test/programmingtheiot/programmingtheiot/part01/integration con ConstrainedDeviceAppTest.
Se valida que la aplicación se inicie, ejecute y se detenga correctamente con mensajes de log esperados.

Finalidad de las Modificaciones
Estandarización y Modularidad: Facilitar la implementación y mantenimiento del código al seguir una estructura organizada dentro de programmingtheiot/cda/app.

2- Creación del Módulo SystemPerformanceManager

Se crea un nuevo módulo en Python llamado SystemPerformanceManager.py dentro del paquete ./programmingtheiot/cda/system.

Definición de la Clase SystemPerformanceManager

Se crea una clase con el mismo nombre dentro del módulo.
Se implementa un constructor sin parámetros, que inicializa las siguientes variables de clase:

self.pollRate: Obtiene un valor entero desde ConfigUtil, correspondiente al número de ciclos de sondeo (POLL_CYCLES_KEY).
self.locationID: Obtiene una propiedad de cadena desde ConfigUtil, correspondiente a la ubicación del dispositivo (DEVICE_LOCATION_ID_KEY).
self.dataMsgListener: Se inicializa como None, reservándose para futuras implementaciones.

Se valida que self.pollRate no sea menor o igual a 0; en tal caso, se asigna un valor por defecto (DEFAULT_POLL_CYCLES).

Implementación de Métodos de Control

startManager(): Registra un mensaje en el log indicando que el SystemPerformanceManager ha sido iniciado.
stopManager(): Registra un mensaje en el log indicando que el SystemPerformanceManager ha sido detenido.

Pruebas de Integración:
Se ejecutan desde ./src/test/programmingtheiot/part01/integration con SystemPerformanceManagerTest.

3- Integración de SystemPerformanceManager en ConstrainedDeviceApp

Se importa la clase SystemPerformanceManager en ConstrainedDeviceApp desde programmingtheiot.cda.system.SystemPerformanceManager.
Se crea una instancia de SystemPerformanceManager dentro del constructor de ConstrainedDeviceApp, asignándola a self.sysPerfMgr.
Modificación de los Métodos startApp() y stopApp()

En startApp(), se invoca self.sysPerfMgr.startManager(), asegurando que el administrador de rendimiento del sistema inicie junto con la aplicación.
En stopApp(), se invoca self.sysPerfMgr.stopManager(), garantizando que el administrador de rendimiento del sistema se detenga cuando la aplicación finalice.

Pruebas de Integración:
Se ejecutan desde ./src/test/programmingtheiot/part01/integration con ConstrainedDeviceAppTest.
Se valida que la inicialización, ejecución y detención de ConstrainedDeviceApp y SystemPerformanceManager se realicen correctamente, mostrando los logs esperados.

4- Creación del Módulo BaseSystemUtilTask

Se crea el archivo BaseSystemUtilTask.py dentro del paquete ./programmingtheiot/cda/system.
Se define la clase BaseSystemUtilTask dentro del módulo.

Implementación del Constructor
Se agrega un constructor que recibe los parámetros name (tipo str) y typeID (tipo int).
Se asignan valores predeterminados utilizando ConfigConst.NOT_SET para name y ConfigConst.DEFAULT_SENSOR_TYPE para typeID.
Se almacenan los valores en variables de clase (self.name y self.typeID).

Implementación de Métodos Getters
Se añaden los métodos getName() y getTypeID(), que simplemente devuelven self.name y self.typeID, respectivamente.

Definición de Método Abstracto getTelemetryValue()
Se agrega el método getTelemetryValue() como una plantilla con pass, lo que indica que será implementado por subclases en futuras modificaciones.

Pruebas Implementadas
No se requieren pruebas para esta base de clase, ya que su funcionalidad será utilizada en clases derivadas.


5- Creación del Módulo SystemCpuUtilTask

Se crea el archivo SystemCpuUtilTask.py dentro del paquete ./programmingtheiot/cda/system.
Se define la clase SystemCpuUtilTask, que extiende BaseSystemUtilTask.

Importación de Librerías Necesarias
Se importan los módulos logging y psutil para la gestión de logs y la obtención del uso de CPU.
Se importa ConfigConst para utilizar constantes de configuración.
Se importa la clase BaseSystemUtilTask desde programmingtheiot.cda.system.

Implementación del Constructor
Se inicializa SystemCpuUtilTask llamando al constructor de la clase base con super().
Se establecen name y typeID con ConfigConst.CPU_UTIL_NAME y ConfigConst.CPU_UTIL_TYPE, respectivamente.

Implementación del Método getTelemetryValue()
Se sobrescribe el método getTelemetryValue(), retornando psutil.cpu_percent(), que obtiene el porcentaje de uso de CPU.


Pruebas Unitarias:
Se ejecuta SystemCpuUtilTaskTest desde ./src/test/programmingtheiot/part01/unit.
Se verifica que testGetTelemetryValue() pase correctamente, confirmando que la obtención del uso de CPU es funcional.


6- Creación del Módulo SystemMemUtilTask

Se crea el archivo SystemMemUtilTask.py dentro del paquete ./programmingtheiot/cda/system.
Se define la clase SystemMemUtilTask, que extiende BaseSystemUtilTask.
Importación de Librerías Necesarias

Se importan los módulos logging y psutil para la gestión de logs y la obtención del uso de memoria.
Se importa ConfigConst para utilizar constantes de configuración.
Se importa la clase BaseSystemUtilTask desde programmingtheiot.cda.system.
Implementación del Constructor

Se inicializa SystemMemUtilTask llamando al constructor de la clase base con super().
Se establecen name y typeID con ConfigConst.MEM_UTIL_NAME y ConfigConst.MEM_UTIL_TYPE, respectivamente.
Implementación del Método getTelemetryValue()

Se sobrescribe el método getTelemetryValue(), retornando psutil.virtual_memory().percent, que obtiene el porcentaje de uso de la memoria RAM.
Pruebas Implementadas

Pruebas Unitarias:
Se ejecuta SystemMemUtilTaskTest desde ./src/test/programmingtheiot/part01/unit.
Se verifica que testGetTelemetryValue() pase correctamente, confirmando que la obtención del uso de memoria es funcional.


7- Importaciones Necesarias
Se importa apscheduler.schedulers.background.BackgroundScheduler para programar tareas en intervalos regulares.
Se importan SystemCpuUtilTask y SystemMemUtilTask para obtener datos de uso del sistema.
Se importan logging y ConfigConst para el manejo de logs y configuración.

-  Constructor de SystemPerformanceManager
Se obtiene la configuración del sistema desde ConfigUtil:
pollRate: Intervalo de muestreo de telemetría (si es menor o igual a 0, se usa el valor por defecto).
locationID: Identificador de ubicación del dispositivo.
Se crea una instancia de BackgroundScheduler y se programa la ejecución periódica de handleTelemetry().
Se inicializan las tareas SystemCpuUtilTask y SystemMemUtilTask.

- Implementación del Método handleTelemetry()
Obtiene el uso de CPU y memoria utilizando las clases correspondientes.
Registra los valores mediante logging.debug().

- Métodos startManager() y stopManager()
startManager()
Inicia el BackgroundScheduler si aún no está en ejecución.
stopManager()
Detiene el BackgroundScheduler de forma segura.

### Code Repository and Branch


URL: https://github.com/BraisFernandezCaride/programmingtheiot/tree/labmodule02



### Unit Tests Executed

NOTE: TA's will execute your unit tests. You only need to list each test case below
(e.g. ConfigUtilTest, DataUtilTest, etc). Be sure to include all previous tests, too,
since you need to ensure you haven't introduced regressions.

- ConfigUtilTest.py
- SystemCpuUtilTaskTest.py
- SystemMemUtilTaskTest.py

### Integration Tests Executed

NOTE: TA's will execute most of your integration tests using their own environment, with
some exceptions (such as your cloud connectivity tests). In such cases, they'll review
your code to ensure it's correct. As for the tests you execute, you only need to list each
test case below (e.g. SensorSimAdapterManagerTest, DeviceDataManagerTest, etc.)

- ConstrainedDeviceAppTest.py
- SystemPerformanceManagerTest.py
  

EOF.
