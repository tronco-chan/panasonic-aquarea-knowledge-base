# panasonic-aquarea-knowledge-base

Este repositorio es una guía comunitaria sobre el funcionamiento real, ajustes óptimos e integración domótica de la aerotermia **Panasonic Aquarea (Serie L R290 y similares)**.

![imagen_oficial_aquarea](https://cdn.aircon.panasonic.eu/uploads/__/clima_groups/2025/01%20AQUAREA/PANASONIC_HP_HYDRAULIC_2.jpg)

## 🎯 Objetivo
El manual oficial es extenso, pero a veces confuso o con traducciones inexactas. Aquí explicamos **cómo funciona la máquina en la realidad**, no solo en la teoría, y compartimos configuraciones para **Home Assistant** y **Heishamon**.

## 🚀 Contenido

### 📖 Documentación General
* **[Registro Inicial: App y Cloud](docs/1-registro_inicial_app_y_cloud.md):** Cómo obtener acceso de instalador y configurar tu cuenta.
* **[Ajustes del Cloud y Controladora](docs/ajustes_cloud_y_controladora.md):** Qué hace realmente cada botón (Desmintiendo el manual).
* **[Lógica de Funcionamiento](docs/logica_de_funcionamiento.md):** Curvas de compensación, DeltaT y gestión de ACS. ¿Qué genera una parada?
* **[Errores y Troubleshooting](docs/errores_y_troubleshooting.md):** Problemas conocidos (H23, falta de gas) y guías de diagnóstico.

### 🏠 Home Assistant
* **[Configuración Inicial de la Integración](home_assistant/1-configuracion_inicial_integracion.md):** Integración cloud oficial y comunidad.
* **[Automatizaciones](home_assistant/automatizaciones.md):** Control maestro (termostato) y ajuste dinámico de offset.

### 🔌 Heishamon
* **[Configuración Inicial](heishamon/1-configuracion_inicial.md):** soonTM


## ⚠️ Disclaimer
>[!WARNING]
>Esta información se basa en pruebas empíricas y el manual de servicio. Modifica los parámetros bajo tu propia responsabilidad.
