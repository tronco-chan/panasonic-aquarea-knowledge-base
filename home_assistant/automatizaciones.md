# 🛠️ Control Avanzado de Aerotermia en Home Assistant

Este repositorio contiene un ecosistema de automatizaciones para optimizar sistemas de aerotermia (específicamente probado en **Panasonic Aquarea**) utilizando emisores de baja inercia como radiadores de aluminio y fancoils.

## 🏗️ Arquitectura de Control

El sistema no depende de una única regla, sino que utiliza una **arquitectura por capas** para equilibrar tres factores: Confort térmico, eficiencia energética y vida útil del compresor.

### 1. Capa de Seguridad y Estado (Automatización Maestra - Termostato)
Es el "cerebro" del sistema. Gestiona cuándo la máquina debe estar encendida o apagada. Todos los umbrales son configurables mediante helpers externos.
- **Control de Termostato**: Histéresis configurable (Encendido < `input_number.temp_encendido` / Apagado > `input_number.temp_apagado`).
- **Protección del Compresor**: Encadena el final del ciclo de calefacción con el inicio de la carga de ACS para evitar paradas y arranques innecesarios. El umbral de carga ACS se configura mediante `input_number.delta_acs`.
- **Anti-Cycling**: Si la temperatura exterior supera `input_number.temp_ext_boost`, aplica un boost de offset (+1) para aumentar el salto térmico y garantizar que la máquina pueda disipar su potencia mínima sin pararse.
- **Restricción Horaria (Opcional)**: Bloque comentado en el YAML que, si se activa, evita arranques en horas punta (L-V 10-14h / 18-22h). Existe tanto para el encendido de calefacción como para la carga de ACS.

### 2. Capa de Ajuste Dinámico (Regulación de Offset)
Actúa como un sintonizador fino mientras la máquina está en modo calefacción.
- **Lógica Incremental**: Cada hora evalúa la temperatura interior y suma o resta 1 punto al offset actual (rango -5 a +5).
- **Sin Hardcoding**: No fija valores absolutos, sino que lee el estado actual de la entidad `climate` y lo modifica dinámicamente. Los umbrales se calculan a partir de un helper externo (`input_number.consigna_temperatura`) con un margen de ±0.4°C.
- **Zona Muerta**: Mantiene un rango de confort de ±0.4°C alrededor de la consigna configurable donde no realiza cambios, permitiendo que la curva climática trabaje por sí sola.
- **Modo Vacaciones**: El toggle `input_boolean.modo_vacaciones` reduce la consigna efectiva en 2°C para ahorro energético automático.
- **Reducción Nocturna**: El toggle `input_boolean.reduccion_nocturna` reduce la consigna en 1°C durante las horas de sueño (configurables via `input_datetime`), recuperándola automáticamente a la hora de despertar.

---

## 📊 Flujo de Interacción Operativa


| Estado de la Casa | Estado de la Máquina | Acción Maestra | Acción Dinámica |
| :--- | :--- | :--- | :--- |
| **Fría (< temp_encendido)** | OFF | Cambia a HEAT (Offset 0 o +1) | *En espera* |
| **Confort (consigna ±0.4°C)** | HEAT | Mantiene estado | Sin cambios (Zona muerta) |
| **Calor (>consigna +0.4°C)** | HEAT | Mantiene (si Ext < 15°C) | Reduce Offset (-1) |
| **Exceso (> temp_apagado)** | HEAT | Cambia a OFF (+gestión ACS) | *En espera* |

---

## ⚙️ Configuración y Requisitos

### Entidades Necesarias

**Sensores y Entidades de Control:**
- `climate.pana_zone_1`: Entidad principal de la zona de calefacción.
- `water_heater.pana_tank`: Entidad de gestión del depósito ACS.
- `sensor.promedio_temperatura_int`: Sensor (o min/max/avg) de la temperatura interior.
- `sensor.promedio_temperatura_ext`: Sensor de temperatura exterior.

**Helpers — Termostato Maestro (`termostato.yaml`):**
- `input_number.temp_encendido`: Temperatura de encendido de calefacción (por defecto: 21.95°C).
- `input_number.temp_apagado`: Temperatura de apagado por exceso (por defecto: 23.5°C).
- `input_number.temp_ext_boost`: Temperatura exterior que activa el boost anti-cycling (por defecto: 18°C).
- `input_number.delta_acs`: Diferencia mínima entre temp. actual y objetivo del depósito ACS para activar la carga (por defecto: 5°C).

**Helpers — Offset Dinámico (`offset_dinamico.yaml`):**
- `input_number.consigna_temperatura`: Temperatura de consigna para el ajuste de offset (Mín: 18, Máx: 26, Paso: 0.1).
- `input_boolean.modo_vacaciones`: Toggle para activar el modo ahorro vacaciones (reduce la consigna en 2°C).
- `input_boolean.reduccion_nocturna`: Toggle para activar la reducción de temperatura nocturna (reduce la consigna en 1°C).
- `input_datetime.hora_dormir`: Hora de inicio de la reducción nocturna (por defecto 22:00, teniendo en consideración inercia de la casa).
- `input_datetime.hora_despertar`: Hora de fin de la reducción nocturna (por defecto 06:00, teniendo en consideración inercia de la casa).

### Instalación
1. Copia el contenido de las automatizaciones en tu archivo `automations.yaml`.
2. Sustituye los `device_id` y `entity_id` por los correspondientes a tu integración.
3. Asegúrate de que el trigger de la automatización dinámica no coincida en el mismo minuto que el chequeo de la maestra para evitar colisiones de órdenes.

## 💡 Notas sobre Emisores de Baja Inercia
Esta configuración está optimizada para **radiadores de aluminio y fancoils**. Al tener poca inercia, el sistema reacciona rápidamente a los cambios de offset, permitiendo que el ajuste dinámico horario sea muy efectivo para mantener una temperatura lineal sin los "dientes de sierra" típicos de los termostatos ON/OFF convencionales.

---
---


# 🛠️ Home Assistant: Automatización Maestra Aerotermia (ON/OFF + Gestión ACS)

Este repositorio contiene la lógica para el control principal de un sistema de aerotermia mediante Home Assistant. El objetivo es gestionar el encendido/apagado de la calefacción basándose en un termostato promedio y optimizar los ciclos del compresor integrando la demanda de ACS (Agua Caliente Sanitaria). Todos los umbrales son configurables mediante helpers externos.

## 🚀 Funcionalidades

- **Control de Termostato Inteligente**: Histéresis configurable mediante helpers (`input_number.temp_encendido` / `input_number.temp_apagado`). Sin valores hardcodeados.
- **Optimización Anti-Cycling**: Aplica un boost de offset (+1) cuando la temperatura exterior supera `input_number.temp_ext_boost` para evitar paradas prematuras por falta de disipación térmica en emisores de baja inercia (radiadores/fancoils).
- **Gestión de ACS Integrada**: Si se alcanza la temperatura de apagado pero el depósito de ACS necesita carga (delta configurable via `input_number.delta_acs`), la automatización encadena el proceso para evitar una parada y arranque innecesario del compresor.
- **Restricción Horaria (Opcional)**: Bloque comentado en el YAML. Si se descomentan, evita arranques de calefacción y/o carga de ACS en horas punta (L-V 10-14h / 18-22h).

## ⚙️ Helpers Necesarios

| Helper | Tipo | Valor por defecto | Descripción |
| :--- | :--- | :--- | :--- |
| `input_number.temp_encendido` | Número | 21.95°C | Temperatura interior que activa la calefacción |
| `input_number.temp_apagado` | Número | 23.5°C | Temperatura interior que apaga la calefacción |
| `input_number.temp_ext_boost` | Número | 18°C | Temp. exterior para activar boost anti-cycling |
| `input_number.delta_acs` | Número | 5°C | Diferencia mínima para activar carga de ACS |

## 📝 Instalación
1. Crea los helpers en Home Assistant (Configuración → Helpers).
2. Copia el código YAML en tu archivo `automations.yaml`.
3. Ajusta los `entity_id` según tu configuración local.
4. **Archivo**: [`termostato.yaml`](./termostato.yaml)

---
---


# 🌡️ Home Assistant: Ajuste Dinámico de Offset para Aerotermia

Automatización diseñada para realizar un "ajuste fino" de la curva de compensación de la aerotermia en tiempo real, basándose en la desviación de la temperatura interior respecto a una **consigna configurable** externamente.

## 🚀 Funcionalidades

- **Consigna Dinámica**: En lugar de umbrales fijos (hardcoded), la automatización lee el valor de `input_number.consigna_temperatura` como punto de referencia. Esto permite ajustar la temperatura objetivo sin modificar el código de la automatización.
- **Zona Muerta Relativa (±0.4°C)**: El sistema calcula dinámicamente los umbrales de actuación respecto a la consigna:
  - **Baja offset (-1)**: Cuando `promedio_temperatura_int` > `consigna + 0.4°C`.
  - **Sube offset (+1)**: Cuando `promedio_temperatura_int` < `consigna - 0.4°C`.
- **Modo Vacaciones** (`input_boolean.modo_vacaciones`): Cuando está activado, la consigna efectiva se **reduce en 2°C**, permitiendo ahorrar energía sin modificar el valor base del helper. Al desactivarlo, la consigna original se recupera instantáneamente.
- **Reducción Nocturna** (`input_boolean.reduccion_nocturna`): Cuando está activado, la automatización reduce automáticamente la consigna en **1°C** durante las horas de sueño. Las horas se configuran mediante `input_datetime.hora_dormir` e `input_datetime.hora_despertar`. Al llegar la hora de despertar, la consigna se recupera sin intervención.
- **Acumulable**: Ambas reducciones se suman. Si `modo_vacaciones` y `reduccion_nocturna` están activos simultáneamente de noche, la consigna baja un total de 3°C.
- **Modificación Incremental**: Suma o resta 1 punto al offset actual (`climate.set_temperature`) de forma progresiva, nunca valores absolutos.
- **Límites de Seguridad**: El offset se mantiene estrictamente dentro del rango permitido por la máquina (de -5 a +5).
- **Condición de Temperatura Exterior**: Controla el límite inferior del offset en la reducción:
  - **Exterior < 15°C**: permite reducir el offset hasta -5.
  - **Exterior ≥ 15°C**: permite reducir el offset solo si es positivo (> 0), con un suelo de 0. Nunca se permite ir a negativo con temperaturas exteriores templadas, ya que generaría problemas de disipación.
- **Respeto al Ciclo ACS**: Se pausa automáticamente durante las horas de producción de ACS (16:00–19:59) para no interferir en la prioridad de la máquina.
- **Reintentos con Protección**: El valor objetivo del offset se calcula **una sola vez** antes de aplicarse. Si la escritura falla, se reintenta hasta 5 veces con esperas de 1 minuto entre intentos, sin riesgo de incrementos acumulativos erróneos.

## 📊 Ejemplo de Funcionamiento

Con `consigna_temperatura = 22.5°C`:

| Modo Vacaciones | Reducción Nocturna (noche) | Consigna efectiva | Baja offset si temp > | Sube offset si temp < |
| :--- | :--- | :--- | :--- | :--- |
| OFF | OFF | 22.5°C | 22.9°C | 22.1°C |
| ON | OFF | 20.5°C | 20.9°C | 20.1°C |
| OFF | ON (noche) | 21.5°C | 21.9°C | 21.1°C |
| ON | ON (noche) | 19.5°C | 19.9°C | 19.1°C |
| OFF | ON (día) | 22.5°C | 22.9°C | 22.1°C |

## ⚙️ Helpers Necesarios

| Helper | Tipo | Configuración |
| :--- | :--- | :--- |
| `input_number.consigna_temperatura` | Número | Mín: 18, Máx: 26, Paso: 0.1 |
| `input_boolean.modo_vacaciones` | Conmutador (Toggle) | — |
| `input_boolean.reduccion_nocturna` | Conmutador (Toggle) | — |
| `input_datetime.hora_dormir` | Hora | Por defecto: 22:00 |
| `input_datetime.hora_despertar` | Hora | Por defecto: 06:00 |

## 💡 ¿Por qué usar esta automatización?

Incluso con una curva de calefacción perfectamente ajustada, factores externos como el viento, la radiación solar o la ocupación de la vivienda pueden variar la demanda térmica. Esta lógica actúa como un "termostato inteligente" que corrige la curva de impulsión suavemente. La consigna externa permite adaptar el comportamiento sin editar la automatización, el modo vacaciones ofrece ahorro energético con un toggle, y la reducción nocturna permite dejar enfriar la casa de noche y recuperar la temperatura a la hora de despertar de forma completamente automática.

## 📝 Configuración

- **Trigger**: Se ejecuta cada hora (minuto :05). Se recomienda que no coincida con otras automatizaciones maestras para permitir la estabilización de los sensores.
- **Condiciones previas**: Solo actúa si `climate.pana_zone_1` está en modo `heat` y fuera de las horas punta ACS (16–19h).
- **Archivo**: [`offset_dinamico.yaml`](./offset_dinamico.yaml)

---

| | | |
|:---|:---:|---:|
| [← Configuración Inicial](1-configuracion_inicial_integracion.md) | [📚 Volver al índice](../README.md) | |