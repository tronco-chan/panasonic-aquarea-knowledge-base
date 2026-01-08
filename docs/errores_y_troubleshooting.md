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

# Errores comunes

SoonTM


---

[<- Volver al inicio del repositorio](../README.md)
