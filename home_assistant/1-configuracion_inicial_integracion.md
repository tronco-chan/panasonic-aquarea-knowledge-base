# 🏠 Integración con Home Assistant (Panasonic Aquarea Cloud)

Configurar la aerotermia Panasonic en Home Assistant tiene su "truco". Las integraciones oficiales o las más antiguas de HACS suelen fallar debido a los recientes cambios en la seguridad de Panasonic. 

A continuación, se detalla el procedimiento exacto para conseguir una conexión estable.

## ⚠️ Requisitos Previos Obligatorios

1. **Cuenta Dedicada:** Es **altamente recomendable** (casi obligatorio para evitar desconexiones) crear una segunda cuenta de usuario en la app de Panasonic Comfort Cloud exclusiva para usar en Home Assistant. No utilices la misma cuenta que llevas logueada en tu teléfono personal.
2. **HACS Instalado:** Debes tener funcionando el [Home Assistant Community Store](https://hacs.xyz/).
3. **2FA Activo:** Es **obligatorio** tener activada la autenticación en dos pasos (vía SMS) en tu cuenta de Panasonic Comfort Cloud. Si no lo haces, la integración no podrá loguearse.
4. **Paciencia y Reinicios:** Sigue los pasos en orden y reinicia cuando se indica.

---

## 🚀 Guía de Instalación Paso a Paso

### 1. Preparar la base (Instalación de la integración "vieja")
Aunque parece contradictorio, necesitamos instalar primero la integración clásica para preparar el terreno:
* Ve a **HACS** -> **Integraciones**.
* Busca e instala: `Panasonic Aquarea Smart Cloud` (de @cjaliaga). `https://github.com/cjaliaga/home-assistant-aquarea/`
* **Reinicia Home Assistant.**

### 2. Intento de autenticación inicial
* Ve a **Ajustes** -> **Dispositivos y Servicios** -> **Añadir Integración**.
* Busca "Panasonic Aquarea Smart Cloud".
* Introduce tu email y contraseña de Panasonic.
* **Nota:** No te preocupes si falla o si da error de autenticación en este punto. El objetivo es que el sistema reconozca los componentes base.

### 3. Instalación de la integración definitiva (aioaquarea)
Ahora instalaremos el repositorio que realmente funciona con el nuevo sistema de Panasonic:
* Ve a **HACS** -> **Integraciones**.
* Haz clic en los **tres puntos** (esquina superior derecha) y selecciona **Repositorios personalizados**.
* En el campo **URL**, pega: `https://github.com/wpatrik14/aioaquarea`
* En **Tipo**, selecciona `Integración`.
* Haz clic en **Añadir** y luego instálala cuando aparezca en la lista.
* **Reinicia Home Assistant.**

### 4. Configuración final y 2FA
* Una vez reiniciado, ve de nuevo a **Ajustes** -> **Dispositivos y Servicios**.
* Busca la integración de Panasonic (si ya estaba el cuadro, dale a "Configurar", si no, búscala de nuevo).
* Introduce tus credenciales.
* Mantén el móvil a mano: **Te llegará un SMS con un código de verificación**. Introdúcelo en el formulario de Home Assistant.

---

## 💡 Notas de Funcionamiento

* **Sincronización:** La nube de Panasonic tiene un retardo (delay). Los cambios que hagas en Home Assistant pueden tardar entre 10 y 30 segundos en verse reflejados en la máquina real.
* **Entidades:** Tras la instalación, dispondrás de entidades para:
    * Controlar la temperatura de consigna (Zone 1 / Zone 2).
    * Cambiar modos (Calor / Frío / ACS).
    * Controlar el modo *Powerful* y silencio.

---

| | | |
|:---|:---:|---:|
| | [📚 Volver al índice](../README.md) | [Automatizaciones →](automatizaciones.md) |