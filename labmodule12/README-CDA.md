

## Lab Module 12 - Semester Project - CDA Components



### Description

#  Integración del Sensor de Humo en el CDA (Constrained Device Application)

##  Objetivo

El objetivo de esta implementación es extender el sistema del *Constrained Device Application (CDA)* con soporte para un nuevo tipo de sensor: el **sensor de humo**. Esta mejora forma parte del Lab 12 del proyecto *Programming the Internet of Things*, donde se busca simular o emular sensores ambientales en un entorno IoT distribuido.

---

##  Archivos Creados

Para habilitar la integración del sensor de humo, se crearon y modificaron los siguientes archivos:

### 1. `SmokeSensorSimTask.py`
- **Ubicación**: `programmingtheiot/cda/sim/`
- **Funcionalidad**: Clase encargada de simular un sensor de humo, generando valores dentro de un rango configurable.
- **Salida**: Simula lecturas porcentuales de concentración de humo en el ambiente.

### 2. `SmokeSensorEmulatorTask.py`
- **Ubicación**: `programmingtheiot/cda/emulated/`
- **Funcionalidad**: Emulador del sensor de humo para el modo de ejecución emulado, generando valores en tiempo real mediante lógica programada.

### 3. Modificación en `SensorDataGenerator.py`
- **Ubicación**: `programmingtheiot/cda/sim/`
- **Funcionalidad añadida**: Se incluyó un nuevo método para generar datasets diarios simulados de humo.

### 4. Modificación en `ConfigConst.py`
- **Ubicación**: `programmingtheiot/common/`
- **Constante añadida**: `SMOKE_SENSOR_TYPE`
- **Propósito**: Representar e identificar el tipo de sensor de humo en el sistema.

---

##  Cambios en Código Existente

### Archivo: `SensorAdapterManager.py`
- **Ubicación**: `programmingtheiot/cda/app/`

#### Cambios realizados:
- Se importó el nuevo módulo `SmokeSensorSimTask`.
- En el método `_initEnvironmentalSensorTasks`:
  - Se añadió la obtención de los valores de configuración para el humo (`smoke.simFloor` y `smoke.simCeiling`).
  - Se generó un dataset de simulación usando `SensorDataGenerator`.
  - Se creó una instancia de `SmokeSensorSimTask` para el modo simulado.
  - Se agregó la carga e instancia de `SmokeSensorEmulatorTask` para el modo emulado.
- En el método `handleTelemetry`:
  - Se generó y manejó una lectura del sensor de humo como parte del flujo de telemetría, enviándola mediante el `IDataMessageListener`.

---

### Code Repository and Branch


URL: https://github.com/BraisFernandezCaride/programmingtheiot/tree/lab12






EOF.
