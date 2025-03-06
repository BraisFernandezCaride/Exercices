# Constrained Device Application (Connected Devices)

## Lab Module 02

Be sure to implement all the PIOT-CDA-* issues (requirements) listed at [PIOT-INF-02-001 - Lab Module 02](https://github.com/orgs/programming-the-iot/projects/1#column-9974938).

### Description

NOTE: Include two full paragraphs describing your implementation approach by answering the questions listed below.

What does your implementation do? 

How does your implementation work?

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
Se ejecutan desde ./src/test/python/programmingtheiot/part01/unit con ConfigUtilTest.
Pruebas de Integración:
Se ejecutan desde ./src/test/python/programmingtheiot/part01/integration con ConstrainedDeviceAppTest.
Se valida que la aplicación se inicie, ejecute y se detenga correctamente con mensajes de log esperados.

### Code Repository and Branch

NOTE: Be sure to include the branch (e.g. https://github.com/programming-the-iot/python-components/tree/alpha001).

URL: 

### UML Design Diagram(s)

NOTE: Include one or more UML designs representing your solution. It's expected each
diagram you provide will look similar to, but not the same as, its counterpart in the
book [Programming the IoT](https://learning.oreilly.com/library/view/programming-the-internet/9781492081401/).


### Unit Tests Executed

NOTE: TA's will execute your unit tests. You only need to list each test case below
(e.g. ConfigUtilTest, DataUtilTest, etc). Be sure to include all previous tests, too,
since you need to ensure you haven't introduced regressions.

- ConfigUtilTest.py
- 
- 

### Integration Tests Executed

NOTE: TA's will execute most of your integration tests using their own environment, with
some exceptions (such as your cloud connectivity tests). In such cases, they'll review
your code to ensure it's correct. As for the tests you execute, you only need to list each
test case below (e.g. SensorSimAdapterManagerTest, DeviceDataManagerTest, etc.)

- ConstrainedDeviceAppTest.py
- 
- 

EOF.
