# Problemas conocidos actuales

## Error H23 en Panasonic Aquarea Serie L

Fallo detectado en la serie de bombas de calor Panasonic Aquarea L (R290), centrado en el error de desescarche **H23** y la gestión de la válvula de expansión electrónica (EEV).

> [!IMPORTANT]
>Solucion temporal por parte de Panasonic. Nueva placa base. A la espera de actualizacion de firmware.

## 📋 Descripción del Problema
El modelo Panasonic Serie L (lanzado a finales de 2023) presenta un comportamiento errático durante los ciclos de desescarche y cambios bruscos de carga térmica. El sistema sufre caídas de temperatura extremas en el refrigerante que activan los protocolos de seguridad.

### Síntomas Principales
* **Congelación en Franjas:** El evaporador exterior no se congela de forma uniforme, reduciendo el COP.
* **Desescarches Fantasma:** Ciclos de limpieza de hielo cuando la unidad está físicamente limpia.
* **Bloqueo H23:** Error crítico que detiene la máquina y requiere reinicio manual.

---

## 🔍 Análisis del Error H23

### Origen Técnico
El error se dispara cuando el **sensor de temperatura de la tubería de entrada (Pipe In)** registra valores inferiores a **-50 °C**. 

* **Causa raíz:** Se identifica un fallo en la lógica del **firmware** que controla la Válvula de Expansión Electrónica (EEV). La válvula no regula con la velocidad necesaria tras un desescarche o ante una demanda máxima de potencia (ej. cambio a ACS).
* **Frecuencia:** El problema es más agudo en unidades con alta carga térmica o cuando la unidad intenta recuperar temperatura tras desescarchar funcionando a frecuencias de compresor elevadas (>60-70 Hz).

### El "Bug" de los 30 Minutos
Se ha observado mediante monitorización que el control de la EEV funciona correctamente durante los primeros 30 minutos tras el arranque. El fallo de regulación tiende a manifestarse una vez superado este periodo de tiempo operativo.

---

## 🛠 Soluciones y Mitigación

### 1. Solución Oficial (Panasonic "Hotfix")
La respuesta actual de la marca consiste en la sustitución de la **Placa Electrónica Principal**.
> [!IMPORTANT]
> **Nota sobre el parche:** Los análisis sugieren que la nueva placa no corrige la regulación de la válvula de expansión, sino que **desactiva o reduce la sensibilidad del error H23**. La máquina deja de bloquearse, pero el funcionamiento térmico ineficiente puede persistir.

### 2. Medidas de Mitigación (Usuario/Instalador)
Hasta la llegada de un firmware definitivo que optimice la EEV, se recomiendan las siguientes acciones en situacion continuada de desescarches:

* **Modo Silencio Nivel 2:** Limita la frecuencia del compresor a ~35-40 Hz. Al evitar picos de potencia, la válvula de expansión trabaja en un rango más estable y se previenen las caídas a -50 °C.
* **Aislamiento del Sensor:** Mejorar el contacto térmico (pasta térmica) y el aislamiento físico del sensor de tubería para evitar lecturas falseadas por el flujo de aire frío.
* **Garantía de Caudal:** Asegurar que el circuito de calefacción permita un volumen de agua suficiente para realizar desescarches rápidos y menos agresivos.

---
---
# Errores comunes

# 🌡️ ¿Mi Aerotermia no calienta? Diagnóstico de Presión y Carga (R32)

Este documento sirve de guía para usuarios de aerotermia (especialmente unidades **Biblock**) que notan que su calefacción no alcanza la temperatura deseada a pesar de que la máquina parece estar trabajando al máximo.

## ❓ ¿Es este tu caso?
Si tu respuesta es **SÍ** a estos puntos, es muy probable que tu máquina tenga una **carga de gas insuficiente** o una fuga:

1.  **El "Techo de los 30°C":** Por mucho que subas la consigna (ej. a 45°C), el agua de impulsión se queda estancada en 29-31°C.
2.  **Esfuerzo Máximo:** El compresor de la unidad exterior se escucha a máximas revoluciones (100% de frecuencia, 70hz ~) de forma constante.
3.  **Presión "Hundida":** Si miras el cloud, la presión de alta se clava en torno a los **20 kgf/cm² (aprox. 19-20 bar)**.

---

## 📉 Explicación Técnica: La relación Presión-Temperatura
En sistemas con refrigerante **R32** o **R290**, el calor se transfiere al agua mediante la condensación del gas. Existe una relación física inamovible: **a mayor presión, mayor temperatura.**. En el caso del R32:

| Presión de Alta (kgf/cm²) | Temp. del Gas (Saturación) | Temp. Máx. del Agua (aprox) | Estado del Sistema |
| :--- | :--- | :--- | :--- |
| **20 kgf/cm²** | **~31.5 °C** | **29 - 30 °C** | ❌ **Falta Gas / Anomalía** |
| **26 kgf/cm²** | **~39.0 °C** | **36 - 38 °C** | ⚠️ Rendimiento Bajo |
| **32 kgf/cm²** | **~47.0 °C** | **43 - 45 °C** | ✅ **Funcionamiento Correcto** |

> **El diagnóstico es claro:** Si los datos del cloud marcan 20 kgf/cm², el gas dentro del intercambiador solo está a 31.5°C. Por leyes físicas, el agua nunca podrá calentarse por encima de esa cifra.

---

## 🔍 Comprobaciones de Usuario

### 1. El Salto Térmico ($\Delta T$)
Revisa en el panel de control la diferencia entre **Temperatura de Impulsión** (salida) y **Temperatura de Retorno** (entrada).
* Si la diferencia es muy pequeña (2-3°C) y la presión es baja, la máquina no tiene capacidad de "bombear" calor al circuito.

### 2. Rendimiento en kW
Si tu máquina es de **9 kW** y, estando al 100%, apenas genera **7 u 8 kW** de energía térmica, confirma que hay una pérdida de eficiencia por falta de masa de refrigerante.

---

## 📢 Guía para hablar con el Técnico
Para evitar respuestas genéricas como "es que fuera hace frío", utiliza datos precisos:

* **Paso 1:** Indica que el compresor está al **100% de frecuencia** pero la presión de alta no sube de **20 kgf/cm²**.
* **Paso 2:** Explica que, con esa presión, el **R32 condensa a 31°C**, lo que hace imposible alcanzar la consigna de calefacción.
* **Paso 3:** Solicita una revisión de la carga mediante **recuperación y pesaje con báscula**, ya que las presiones bajas con compresor a tope suelen indicar falta de refrigerante (fuga o carga insuficiente en la instalación inicial).

---


[<- Volver al inicio del repositorio](../README.md)
