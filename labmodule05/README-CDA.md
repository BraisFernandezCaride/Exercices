# Constrained Device Application (Connected Devices)

## Lab Module 05


### Description

1-
Almacenamiento de datos de rendimiento:
Se actualizó el método handleTelemetry() para capturar los valores de utilización de CPU y memoria, almacenándolos en una instancia de SystemPerformanceData. Luego, estos valores se asocian con un identificador de ubicación antes de ser enviados al manejador de mensajes, si está configurado.

Invocación de callback al listener:
Si la instancia dataMsgListener está definida, se invoca su método correspondiente para manejar los datos de rendimiento del sistema, permitiendo la comunicación con otros componentes.

Configuración del listener de mensajes:
Se actualizó el método setDataMessageListener() para permitir la asignación de un listener, asegurando que SystemPerformanceManager pueda notificar eventos en el futuro.

Prueba(integración):
- SystemPerformanceManagerTest.py

2-
El propósito de esta tarea es completar e implementar la clase DataUtil, que se encargará de transformar objetos en formato JSON y convertir JSON en objetos. Esto es importante porque JSON es un formato muy usado para el intercambio de datos en aplicaciones, especialmente en sistemas IoT.

Dentro de DataUtil, hay seis métodos que debemos completar. Tres de ellos sirven para convertir objetos (ActuatorData, SensorData y SystemPerformanceData) en una cadena de texto con formato JSON. Los otros tres hacen lo contrario: toman una cadena en JSON y la transforman en un objeto del tipo correspondiente. Para que todo funcione correctamente, los nombres de los métodos y los parámetros deben coincidir con lo que esperan las pruebas unitarias.

Para convertir un objeto a JSON, usareemos la clase JsonDataEncoder, que ya está definida en el código. Su trabajo es simple: transformar un objeto en un diccionario para que la biblioteca json pueda manejarlo sin problemas. Básicamente, toma un objeto y devuelve su versión en forma de diccionario.

Cuando queremos pasar de JSON a un objeto, hay que seguir algunos pasos. Primero, debemos asegurarnos de que la cadena JSON está bien formateada. Esto incluye cambiar comillas y asegurarnos de que los valores booleanos estén escritos en minúsculas. Luego, usamos json.loads() para transformar la cadena JSON en un diccionario. Después, creamos un objeto vacío del tipo correcto y recorremos el diccionario para copiar cada valor en el objeto. Si encontramos una clave en el JSON que no existe en el objeto, registramos una advertencia en los logs.

Como el código para convertir datos es casi el mismo para los tres tipos de objetos (ActuatorData, SensorData y SystemPerformanceData), lo mejor es crear métodos privados dentro de DataUtil que hagan la parte repetitiva. Así, cada método público soolo tendrá que llamar a estos métodos internos con el tipo de dato adecuado. Esto hace que el código sea más ordenado y fácil de mantener.

Una vez que terminemos de programar DataUtil, es importante probarlo. Primero, ejecutamos DataUtilTest, que es una prueba unitaria que nos dirá si el código funciona como debería. Luego, cuando DataUtil esté listo en ambos módulos (CDA y GDA), ejecutamos las pruebas de integración. Es posible que fallen en el primer intento, pero si todo está bien, deberían funcionar en la segunda ejecución.


Prueba(unitaria):
- DataUtilTest.py

Prueba(integración):
- DataIntegrationTest.py

### Code Repository and Branch


URL: https://github.com/BraisFernandezCaride/programmingtheiot/tree/lab05



### Unit Tests Executed

NOTE: TA's will execute your unit tests. You only need to list each test case below
(e.g. ConfigUtilTest, DataUtilTest, etc). Be sure to include all previous tests, too,
since you need to ensure you haven't introduced regressions.

- DataUtillTest.py
 
  

### Integration Tests Executed

NOTE: TA's will execute most of your integration tests using their own environment, with
some exceptions (such as your cloud connectivity tests). In such cases, they'll review
your code to ensure it's correct. As for the tests you execute, you only need to list each
test case below (e.g. SensorSimAdapterManagerTest, DeviceDataManagerTest, etc.)

- SystemPerformanceManagerTest.py
- DataIntegrationTest.py
  

EOF.
