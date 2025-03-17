# Constrained Device Application (Connected Devices)

## Lab Module 01


### Description
Para la realización de la primera práctica solo se ha instalado las dependencias necesarias en el equipo para poder trabajar en las prácticas posteriores sobre los repositorios de github.
Lo primero se han clonado los reposiorios de github para poder trabajaar en ellos.
Fué necesario istalar la consola wsl en el sistema operativo de windows para poder instalar las dependencias.
Para poder trabajar adecuadamente se creó un entorno virtual en el que se intalaron las dependencias.
En el caso concreto del proyecto en python se necesitó transladar el path hacia dentro de la carpeta lo cual al pricipio dió muchos problemas. Ya que la guía proporcionada por laa asignatura no estaba actualizada y no funcionaaba adecuadamente. Finalmente la solución fué utilizar el siguiente comando en la consola con wsl: export PYTHONPATH=$PYTHONPATH:/home/brais/programmingtheiot/programmingtheiot/src/main/python:/home/brais/programmingtheiot/programmingtheiot/src/test/python
El cual translada el path a las dos carpetas de python a la de main y a la de test.

### Code Repository and Branch

NOTE: Be sure to include the branch (e.g. https://github.com/programming-the-iot/python-components/tree/alpha001).

URL: https://github.com/BraisFernandezCaride/programmingtheiot/tree/default



### Unit Tests Executed

NOTE: TA's will execute your unit tests. You only need to list each test case below
(e.g. ConfigUtilTest, DataUtilTest, etc). Be sure to include all previous tests, too,
since you need to ensure you haven't introduced regressions.

Los test se han ejecutado en la versión defaut del código con el fin de comprobar que se encontaban instaladas todas las dependencias necesarias y estaba bien exportado el pythonpath

- ConfigUtilTest.py
- SystemCpuUtillTaskTest.py
- SystemMemUtillTaskTest.py

### Integration Tests Executed

NOTE: TA's will execute most of your integration tests using their own environment, with
some exceptions (such as your cloud connectivity tests). In such cases, they'll review
your code to ensure it's correct. As for the tests you execute, you only need to list each
test case below (e.g. SensorSimAdapterManagerTest, DeviceDataManagerTest, etc.)

Los test se han ejecutado en la versión defaut del código con el fin de comprobar que se encontaban instaladas todas las dependencias necesarias y estaba bien exportado el pythonpath

- ConstrainedDeviceAppTest.py
- SystemPerformanceManagerTest.py

EOF.
