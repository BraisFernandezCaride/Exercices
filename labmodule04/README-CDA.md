# Constrained Device Application (Connected Devices)

## Lab Module 04



### Description

1-

Descripción:
Se han creado módulos específicos para la simulación de sensores, cada uno derivado de la clase base BaseSensorSimTask. Estos módulos imitan el comportamiento de sensores reales al obtener datos a través del emulador y almacenarlos en objetos SensorData para su posterior procesamiento.

1. HumiditySensorEmulatorTask:
Este módulo emula un sensor de humedad utilizando SenseHAT. Se encarga de obtener valores de humedad simulados y almacenarlos en una estructura de datos para su uso en el sistema.

La clase hereda de BaseSensorSimTask y se inicializa con los parámetros adecuados (name y typeID definidos en ConfigConst).
Durante la inicialización, se carga el valor de enableEmulation desde el archivo de configuración para determinar si el sensor operará en modo emulado o intentará comunicarse con el hardware físico.
Se sobrescribe el método generateTelemetry(), el cual obtiene el valor de humedad simulado desde self.sh.environ.humidity, lo almacena en un objeto SensorData y lo actualiza en self.latestSensorData.


2. PressureSensorEmulatorTask:
Este módulo es responsable de simular un sensor de presión, siguiendo una estructura similar al HumiditySensorEmulatorTask.

La clase se deriva de BaseSensorSimTask y utiliza SenseHAT para generar datos simulados.
La variable enableEmulation se carga desde el archivo de configuración, permitiendo alternar entre el modo emulado y el hardware real.
El método generateTelemetry() recupera la presión simulada de self.sh.environ.pressure, la almacena en un objeto SensorData y la actualiza en self.latestSensorData.


3. TemperatureSensorEmulatorTask:
Esta clase permite la simulación de un sensor de temperatura, operando bajo los mismos principios que las clases anteriores.

Implementación
Se hereda de BaseSensorSimTask y se configura con los parámetros específicos de temperatura.
Se carga la configuración para determinar si se debe operar en modo emulado.
En generateTelemetry(), se obtiene la temperatura desde self.sh.environ.temperature, se almacena en un objeto SensorData y se actualiza self.latestSensorData.

Pruebas (integración)
- HumidityEmulatorTaskTest.py
- PressureEmulatorTaskTest.py
- TemperatureEmulatorTaskTest.py

2-

El propósito principal de esta implenentación es crear tareas emuladoras para diferentes actuadores (como humidificador, HVAC y pantalla LED) que simulen su comportamiento en un entorno virtual, sin necesidad de hardware físico. Estas tareas emuladoras heredan de una clase base y utilizan bibliotecas de emulación como SenseHAT para simular los dispositivos en un Raspberry Pi.

1. HumidifierEmulatorTask:
Finalidad: Simula el comportamiento de un humidificador utilizando el SenseHAT para mostrar mensajes en una pantalla LED cuando se activa o desactiva.
Funcionamiento:
El constructor obtiene la configuración para saber si se debe emular o interactuar con el hardware físico.
El método _activateActuator muestra un mensaje en la pantalla del SenseHAT con la temperatura cuando el humidificador está activado.
El método _deactivateActuator borra el mensaje de la pantalla al desactivar el actuador.

2. HvacEmulatorTask:
Finalidad: Simula el comportamiento de un sistema HVAC (calefacción, ventilación y aire acondicionado), mostrando la temperatura en la pantalla del SenseHAT.
Funcionamiento:
Similar al HumidifierEmulatorTask, el constructor obtiene la configuración para saber si se debe activar el modo emulación.
El método _activateActuator muestra la temperatura en la pantalla LED cuando el HVAC está activado.
El método _deactivateActuator borra la pantalla del SenseHAT al desactivar el HVAC.

3. LedDisplayEmulatorTask:
Finalidad: Simula una pantalla LED que se puede encender y apagar, mostrando un mensaje en la pantalla cuando se activa.
Funcionamiento:
Al activar el actuador, el método _activateActuator desplaza un texto en la pantalla del SenseHAT.
Al desactivarlo, el método _deactivateActuator limpia la pantalla del SenseHAT.

Pruebas(integración):
- HumidifierEmulatorTaskTest.py
- HvacEmulatorTaskTest.py
- LedDisplayEmulatorTaskTest.py


3-

Se realizaron varios cambios en la clase SensorAdapterManager para agregar la funcionalidad del emulador de SenseHAT. A continuación, se describen las modificaciones clave:

Importación de Módulos: Se añadió la importación del módulo import_module desde importlib, que es fundamental para cargar dinámicamente las tareas del emulador durante la ejecución, solo cuando se habilita el emulador.

Configuración del Emulador:

En el constructor de la clase, se actualizó el procesamiento de la propiedad de configuración self.useEmulator, la cual determina si se debe utilizar el emulador o no. Esta propiedad se obtiene desde el archivo de configguración usando el ConfigUtil y la clave ENABLE_EMULATOR_KEY.
Si self.useEmulator es True, se carga dinámicamente el emulador de los sensores (temperatura, humedad y presión).
Carga Dinámica de Tareas del Emulador:

Se utilizó import_module para cargar las tareas del emulador de teemperatura, humedad y presión. Cada uno de estos módulos emuladores se importa y se asigna a la variable correspondiente (self.tempAdapter, self.humidityAdapter, self.pressureAdapter).
Esto asegura que las tareas del emulador solo se carguen en tiempo de ejecución si el emulador está habilitado.
Inicialización de Sensores Ambientales:

Se modificó el método _initEnvironmentalSensorTasks() para inicializar los sensores simulados si el emulador no está habilitado. Este método crea instancias de las tareas de simulación utilizando datos generados aleatoriamente para cada sensor.
Si el emulador está habilitado, en lugar de crear simuladores de sensores, se instancian los emuladores reales para cada tipo de sensor.

Prueba(integración):
- SensorEmulatorManagerTest.py

4-

Configuración del uso del emulador:
Se añadió una variable self.useEmulator para determinar si se debe habilitar la funcionalidad del emulador. Este valor puede ser leído desde un archivo de configuración. Si self.useEmulator es True, se procede a cargar los actuadores emulados dinámicamente.

Carga dinámica de los actuadores emulados:
Si el emulador está habilitado (self.useEmulator = True), se cargan los actuadores emulados de manera dinámica en el constructor de la clase. Esto se hace mediante la carga de los módulos de los actuadores (por ejemplo, HVAC, humedad, etc.) en tiempo de ejecución, en lugar de cargarlos estáticamente al inicio. La idea es que solo se carguen cuando sea necesario y si el emulador está activado.

Método de inicialización de actuadores:
Se creó o actualizó un método de inicialización de actuadores (_initEnvironmentalActuationTasks) para configurar correctamente los actuadores de acuerdo a si se está utilizando el emulador o no. Si self.useEmulator es True, se instancian los actuadores del emulador; de lo contrario, se instancian actuadores simulados.

Prueba(integración):
- ActuatorEmulatorManagerTest.py




### Code Repository and Branch


URL: https://github.com/BraisFernandezCaride/programmingtheiot/tree/lab04



### Unit Tests Executed

NOTE: TA's will execute your unit tests. You only need to list each test case below
(e.g. ConfigUtilTest, DataUtilTest, etc). Be sure to include all previous tests, too,
since you need to ensure you haven't introduced regressions.

- None

### Integration Tests Executed

NOTE: TA's will execute most of your integration tests using their own environment, with
some exceptions (such as your cloud connectivity tests). In such cases, they'll review
your code to ensure it's correct. As for the tests you execute, you only need to list each
test case below (e.g. SensorSimAdapterManagerTest, DeviceDataManagerTest, etc.)

- HumidityEmulatorTaskTest.py
- PressureEmulatorTaskTest.py
- TemperatureEmulatorTaskTest.py
- HumidifierEmulatorTaskTest.py
- HvacEmulatorTaskTest.py
- LedDisplayEmulatorTaskTest.py
- SensorEmulatorManagerTest.py
- ActuatorEmulatorManagerTest.py

EOF.
