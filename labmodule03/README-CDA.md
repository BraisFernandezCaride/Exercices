# Constrained Device Application (Connected Devices)

## Lab Module 03

Be sure to implement all the PIOT-CDA-* issues (requirements) listed at [PIOT-INF-03-001 - Lab Module 03](https://github.com/orgs/programming-the-iot/projects/1#column-10488379).

### Description

1-

Esta modificación introduce y actualiza módulos en Python para manejar datos de sensores, actuadores y rendimiento del sistema en una arquitectura IoT. Se crean tres clases principales (SensorData, ActuatorData y SystemPerformanceData), todas derivadas de BaseIotData, asegurando una estructura común para manejar y manipular estos datos.

Descripción:

BaseIotData (Clase Base)
Proporciona una estructura base para los datos en la arquitectura IoT.
Maneja atributos como nombre, tipo de dato, código de estado y marcas de tiempo.
Proporciona métodos de acceso para manipular estos atributos.

ActuatorData:
Representa comandos de actuadores con soporte para valores numéricos y datos de estado en texto.

Métodos clave:
get/setValue(): Para obtener y modificar el valor del actuador.
get/setCommand(): Para definir comandos específicos.
get/setStateData(): Para manejar datos de estado (por ejemplo, mensajes LED).
setAsResponse(): Marca el dato como una respuesta a un comando.
_handleUpdateData(): Copia datos desde otra instancia de ActuatorData.

SensorData:
Representa datos de sensores, con soporte para valores numéricos.

Métodos clave:
get/setValue(): Accede y modifica el valor del sensor.
_handleUpdateData(): Copia datos de otra instancia de SensorData.

SystemPerformanceData:
Representa métricas de rendimiento del sistema (CPU y memoria).

Métodos clave:
get/setCpuUtilization(): Obtiene y actualiza la utilización de CPU.
get/setMemoryUtilization(): Obtiene y actualiza la utilización de memoria.
_handleUpdateData(): Copia datos de otra instancia de SystemPerformanceData.

Pruebas que se ejecutaron (unitarias): 
- ActuatorDataTest.py
- SensorDataTest.py
- SystemPerformanceDataTest.py


2-

Esta modificación edita el módulo BaseSensorSimTask, que sirve como base para la simulación de sensores en el sistema IoT. Su función principal es generar datos de sensores, ya sea a partir de un conjunto de datos predefinido (SensorDataSet) o generando valores aleatorios dentro de un rango definido.

Principales cambios
Se definen constantes para valores mínimos y máximos de los datos generados.
Se ajusta el constructor para manejar parámetros como nombre, tipo de sensor y conjunto de datos.
Se implementa la generación de telemetría (generateTelemetry()), permitiendo dos modos:
Aleatorio: Si no hay un conjunto de datos, genera valores aleatorios dentro de los límites establecidos.
Secuencial: Si hay un conjunto de datos, toma valores en orden hasta reiniciar el índice cuando llega al final.
Se añaden métodos para obtener el nombre y tipo del sensor, así como el último valor generado.

No se realizan test

3- 

Se introducen nuevos módulos de simulación de sensores (HumiditySensorSimTask, PressureSensorSimTask y TemperatureSensorSimTask), los cuales heredan de BaseSensorSimTask. Su propósito es modelar el comportamiento de sensores ambientales mediante generación de datos simulados.

Principales cambios:
Se crean tres módulos de sensores simulados, cada uno con su propia clase:
HumiditySensorSimTask: Simula un sensor de humedad.
PressureSensorSimTask: Simula un sensor de presión atmosférica.
TemperatureSensorSimTask: Simula un sensor de temperatura.

Cada clase:
Se inicializa con valores mínimos y máximos específicos, definidos en SensorDataGenerator.
Puede utilizar un conjunto de datos (dataSet) o generar valores aleatorios dentro del rango permitido.
Hereda la funcionalidad de BaseSensorSimTask, por lo que la implementación es mínima.

Pruebas(unitarias): 
HumiditySensorSimTaskTest.py
PressureSensorSimTaskTest.py
TemperatureSensorSimTaskTest.py

4-

Se crea BaseActuatorSimTask, que servirá como clase base para los actuadores simulados. Su propósito es manejar comandos de activación y desactivación, almacenar el último estado del actuador y generar respuestas estructuradas.

Principales cambios:
Se crea el módulo BaseActuatorSimTask dentro de ./programmingtheiot/cda/sim/.

Constructor:
Almacena información como nombre, tipo de actuador y un nombre simplificado para logs.
Guarda los últimos valores de comando (lastKnownCommand) y estado (lastKnownValue).

Métodos principales:
_activateActuator(): Simula la activación del actuador, registrando logs.
_deactivateActuator(): Simula la desactivación del actuador, registrando logs.
updateActuator():
Procesa los comandos recibidos (ON o OFF).
Evita repetir comandos idénticos al último ejecutado.
Genera una respuesta con el estado actualizado del actuador.

No se ejecutan test

5- 

Esta tarea consiste en la creación de módulos para actuadores simulados, derivando de BaseActuatorSimTask. Los nuevos módulos HumidifierActuatorSimTask y HvacActuatorSimTask tendrán implementaciones simples, siguiendo el patrón del actuador base.

Cambios clave:

Creación de los módulos:
HumidifierActuatorSimTask
HvacActuatorSimTask

Cada módulo:
Hereda de BaseActuatorSimTask.
Define su nombre y tipo con constantes de ConfigConst.
Incluye un simpleName opcional para mejorar los logs.

Personalización:
Se pueden sobrescribir _activateActuator() y _deactivateActuator(), aunque no es necesario.

Pruebas(unitarias):
HumidifierActuatorSimTaskTest
HvacActuatorSimTaskTest

6-

La implementación de SensorAdapterManager es una clase en Python que se encarga de gestionar simuladores de sensores ambientales (humedad, presión y temperatura)


SensorAdapterManager es responsable de:

Inicializar y configurar simuladores de sensores según los valores definidos en el archivo de configuración.
Manejar un programador de tareas (APScheduler) para recopilar datos de telemetría en intervalos regulares.
Proveer métodos de inicio y detención del proceso de telemetría.
Interactuar con un listener de mensajes de datos para enviar la telemetría generada.
Inicialización (__init__ method)
Al instanciar SensorAdapterManager, el constructor:

Carga los valores de configuración relevantes utilizando ConfigUtil.
Determina si se deben utilizar emuladores o simuladores de sensores (self.useEmulator).
Define la frecuencia de muestreo (self.pollRate).
Recupera el identificador de ubicación (self.locationID).
Crea un programador (APScheduler) para manejar la recopilación periódica de datos.
Inicializa las variables de los adaptadores de sensores (self.humidityAdapter, self.pressureAdapter, self.tempAdapter).
Llama a _initEnvironmentalSensorTasks() para configurar las tareas de los sensores.
Si self.useEmulator es True, los datos provendrán de emuladores en lugar de simuladores.

Inicialización de Sensores (_initEnvironmentalSensorTasks method):

Obtiene valores de piso y techo de las variables ambientales (humedad, presión y temperatura) desde la configuración.
Crea conjuntos de datos simulados utilizando un generador de datos (SensorDataGenerator).
Inicializa los simuladores de sensores (HumiditySensorSimTask, PressureSensorSimTask, TemperatureSensorSimTask), pasándoles los datos generados.
Si self.useEmulator es False, se activan los simuladores en base a los valores de configuración.


La clase usa APScheduler para programar la recopilación de datos:

Llama a handleTelemetry() cada self.pollRate segundos.
Se inicia con self.scheduler.start().
Se detiene con self.scheduler.shutdown().
El programador usa parámetros como coalesce=True para evitar la acumulación de tareas en caso de retrasos y misfire_grace_time=15 para tolerar pequeños retrasos en la ejecución.

Manejo de Telemetría (handleTelemetry method):

Se generan nuevos valores de telemetría para humedad, presión y temperatura (self.humidityAdapter.generateTelemetry(), etc.).
Se asigna self.locationID a cada conjunto de datos.
Se registran los datos generados en el log para depuración.
Si self.dataMsgListener está configurado, se envían los datos al listener para su procesamiento.
Gestión del Listener (setDataMessageListener method)
Este método permite asignar un IDataMessageListener externo para recibir y procesar los datos generados.
Solo se asigna si el listener es válido (!= None).
Control del Administrador
startManager() inicia el programador si no está corriendo.
stopManager() lo detiene de forma segura.
Ambos métodos registran mensajes en el log para indicar el estado del administrador.

Prueba(unitaria):
SensorAdapterManagerTest.py

7-

ActuatorAdapterManager, es responsable de gestionar actuadores (como humidificadores, HVAC y pantallas LED) en un sistema, y adaptarla a las configuraciones disponibles en un entorno simulado o real.

Funciones:

Inicialización de configuraciones: Al crear una instancia de la clase, se leen varios parámetros de configuración, como si se deben usar simuladores o emuladores, y la ID de la ubicación del dispositivo. Esto permite adaptar la clase al entorno en el que se está ejecutando el sistema.

Simulación de actuadores: Si no se utiliza un emulador, la clase inicializa simuladores de actuadores (como un humidificador o HVAC), para que los actuadores puedan "simular" el comportamiento del sistema sin necesidad de hardware real.

Recepción de comandos de actuación: La clase también gestiona los comandos de actuación que recibe. Al recibir un comando, verifica si la ID de la ubicación coincide con la configuración del dispositivo. Si es válida, procesa el comando y actualiza el actuador correspondiente (por ejemplo, encender o apagar un humidificador).

Interacción con otros componentes: La clase permite establecer un listener para recibir mensajes de datos, lo cual es útil si otros componentes del sistema necesitan recibir notificaciones de los cambios en los actuadores.

Prueba(integración):
ActuatorAdapterManagerTest.py

8-




### Unit Tests Executed

NOTE: TA's will execute your unit tests. You only need to list each test case below
(e.g. ConfigUtilTest, DataUtilTest, etc). Be sure to include all previous tests, too,
since you need to ensure you haven't introduced regressions.

- ActuatorDataTest.py
- SensorDataTest.py
- SystemPerformanceDataTest.py
- HumiditySensorSimTaskTest.py
- PressureSensorSimTaskTest.py
- TemperatureSensorSimTaskTest.py
- HumidifierActuatorSimTaskTest.py
- HvacActuatorSimTaskTest.py
- DataUtilTest.py

### Integration Tests Executed

NOTE: TA's will execute most of your integration tests using their own environment, with
some exceptions (such as your cloud connectivity tests). In such cases, they'll review
your code to ensure it's correct. As for the tests you execute, you only need to list each
test case below (e.g. SensorSimAdapterManagerTest, DeviceDataManagerTest, etc.)

- SensorAdapterManagerTest.py
- ActuatorAdapterManagerTest
- DeviceDataManagerNoCommsTest.py
- ConstrainedDeviceAppTest

EOF.
