# Gateway Device Application (Connected Devices)

## Lab Module 01



### Description


Para la realización de la primera práctica solo se ha instalado las dependencias necesarias en el equipo para poder trabajar en las prácticas posteriores sobre los repositorios de github. Lo primero se han clonado los reposiorios de github para poder trabajaar en ellos. Fué necesario istalar la consola wsl en el sistema operativo de windows para poder instalar las dependencias. Para poder trabajar adecuadamente se creó un entorno virtual en el que se intalaron las dependencias. En el caso concreto del proyecto en java se necesitó descargar un comndo de maven para poder ejecutar adecuadamente los test. Para ello se utilizó el siguiente comando en la propia consola de visual studio: mvn install -DskipTests. El cual nos permite ejecutar todos los test de java a excepción de unos pocos paara que no deen error.

### Code Repository and Branch


URL: https://github.com/BraisFernandezCaride/programmingtheiotjava



### Unit Tests Executed

NOTE: TA's will execute your unit tests. You only need to list each test case below
(e.g. ConfigUtilTest, DataUtilTest, etc). Be sure to include all previous tests, too,
since you need to ensure you haven't introduced regressions.

- ConfigUtilTest.java
- ResourceNameTest.java
- SystemCpuUtilTaskTest.java
- SystemMemUtilTaskTest.java

### Integration Tests Executed

NOTE: TA's will execute most of your integration tests using their own environment, with
some exceptions (such as your cloud connectivity tests). In such cases, they'll review
your code to ensure it's correct. As for the tests you execute, you only need to list each
test case below (e.g. SensorSimAdapterManagerTest, DeviceDataManagerTest, etc.)

- GatewatDeviceAppTest.java
- SystemPerformanceManagerTest.java

EOF.
