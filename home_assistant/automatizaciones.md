# 🛠️ Control Avanzado de Aerotermia en Home Assistant

Este repositorio contiene un ecosistema de automatizaciones para optimizar sistemas de aerotermia (específicamente probado en **Panasonic Aquarea**) utilizando emisores de baja inercia como radiadores de aluminio y fancoils.

## 🏗️ Arquitectura de Control

El sistema no depende de una única regla, sino que utiliza una **arquitectura por capas** para equilibrar tres factores: Confort térmico, eficiencia energética y vida útil del compresor.

### 1. Capa de Seguridad y Estado (Automatización Maestra - Termostato)
Es el "cerebro" del sistema. Gestiona cuándo la máquina debe estar encendida o apagada.
- **Control de Termostato**: Histéresis de precisión (Encendido < 21.95°C / Apagado > 23.5°C).
- **Protección del Compresor**: Encadena el final del ciclo de ACS con el inicio de la calefacción para evitar paradas y arranques innecesarios.
- **Anti-Cycling**: Si la temperatura exterior es elevada (>18°C), aplica un boost de offset (+1) para aumentar el salto térmico y garantizar que la máquina pueda disipar su potencia mínima sin pararse.

### 2. Capa de Ajuste Dinámico (Regulación de Offset)
Actúa como un sintonizador fino mientras la máquina está en modo calefacción.
- **Lógica Incremental**: Cada hora evalúa la temperatura interior y suma o resta 1 punto al offset actual (rango -5 a +5).
- **Sin Hardcoding**: No fija valores absolutos, sino que lee el estado actual de la entidad `climate` y lo modifica dinámicamente.
- **Zona Muerta**: Mantiene un rango de confort entre 22.5°C y 23°C donde no realiza cambios, permitiendo que la curva climática trabaje por sí sola.

---

## 📊 Flujo de Interacción Operativa


| Estado de la Casa | Estado de la Máquina | Acción Maestra | Acción Dinámica |
| :--- | :--- | :--- | :--- |
| **Fría (<21.95°C)** | OFF | Cambia a HEAT (Offset 0 o +1) | *En espera* |
| **Confort (22.5 - 23°C)** | HEAT | Mantiene estado | Sin cambios (Zona muerta) |
| **Calor (>23°C)** | HEAT | Mantiene (si Ext < 10°C) | Reduce Offset (-1) |
| **Exceso (>23.5°C)** | HEAT | Cambia a OFF | *En espera* |

---

## ⚙️ Configuración y Requisitos

### Entidades Necesarias
- `climate.pana_zone_1`: Entidad principal de la zona de calefacción.
- `water_heater.pana_tank`: Entidad de gestión del depósito ACS.
- `sensor.promedio_temperatura_int`: Sensor (o min/max/avg) de la temperatura interior.
- `sensor.promedio_temperatura_ext`: Sensor de temperatura exterior.

### Instalación
1. Copia el contenido de las automatizaciones en tu archivo `automations.yaml`.
2. Sustituye los `device_id` y `entity_id` por los correspondientes a tu integración.
3. Asegúrate de que el trigger de la automatización dinámica no coincida en el mismo minuto que el chequeo de la maestra para evitar colisiones de órdenes.

## 💡 Notas sobre Emisores de Baja Inercia
Esta configuración está optimizada para **radiadores de aluminio y fancoils**. Al tener poca inercia, el sistema reacciona rápidamente a los cambios de offset, permitiendo que el ajuste dinámico horario sea muy efectivo para mantener una temperatura lineal sin los "dientes de sierra" típicos de los termostatos ON/OFF convencionales.

---
---


# 🛠️ Home Assistant: Automatización Maestra Aerotermia (ON/OFF + Gestión ACS)

Este repositorio contiene la lógica para el control principal de un sistema de aerotermia mediante Home Assistant. El objetivo es gestionar el encendido/apagado de la calefacción basándose en un termostato promedio y optimizar los ciclos del compresor integrando la demanda de ACS (Agua Caliente Sanitaria).

## 🚀 Funcionalidades
- **Control de Termostato Inteligente**: Histéresis configurada para encendido a los < 21.95°C y apagado a los > 23.5°C.
- **Optimización Anti-Cycling**: Implementa un "Boost" de offset (+1) cuando la temperatura exterior supera los 18°C para evitar paradas prematuras por falta de disipación térmica en emisores de baja inercia (radiadores/fancoils).
- **Gestión de ACS Integrada**: Si se alcanza la temperatura de confort en casa, hasta el punto de generar un apagado, pero el depósito de ACS necesita carga, la automatización encadena el proceso para evitar una parada y arranque innecesario del compresor.
- **Restricción Horaria**: Evita arranques exclusivamente en horas punta para optimizar el coste energético.

## ⚙️ Requisitos
- Integración de Aerotermia que exponga entidades `climate` y `water_heater`.
- Sensor de temperatura promedio interior y exterior.

## 📝 Instalación
1. Copia el código YAML en tu archivo `automations.yaml`.
2. Ajusta los `device_id` y `entity_id` según tu configuración local.

---
---


# 🌡️ Home Assistant: Ajuste Dinámico de Offset para Aerotermia

Automatización diseñada para realizar un "ajuste fino" de la curva de compensación de la aerotermia en tiempo real, basándose en la desviación de la temperatura interior respecto al punto de confort.

## 🚀 Funcionalidades
- **Compensación en Tiempo Real**: Analiza cada hora si la temperatura interior se desvía del rango óptimo (22.5°C - 23°C).
- **Modificación Incremental**: Suma o resta 1 punto al offset actual (`climate.set_temperature`) en lugar de usar valores fijos (hardcoded).
- **Límites de Seguridad**: El offset se mantiene estrictamente dentro del rango permitido por la máquina (de -5 a +5).
- **Condición de Temperatura Exterior**: Solo reduce el offset si la temperatura exterior es inferior a 10°C, delegando el apagado por calor exterior a la automatización maestra.
- **Respeto al Ciclo ACS**: Se pausa automáticamente durante las horas de producción de ACS para no interferir en la prioridad de la máquina.

## 💡 ¿Por qué usar esta automatización?
Incluso con una curva de calefacción perfectamente ajustada, factores externos como el viento, la radiación solar o la ocupación de la vivienda pueden variar la demanda térmica. Esta lógica actúa como un "termostato inteligente" que corrige la curva de impulsión suavemente.

## 📝 Configuración
El trigger está configurado para ejecutarse cada hora. Se recomienda que el minuto de ejecución no coincida exactamente con otras automatizaciones maestras para permitir la estabilización de los sensores.