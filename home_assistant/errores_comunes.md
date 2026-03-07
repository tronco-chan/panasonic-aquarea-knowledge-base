# ❌ Errores Comunes (Integración con Home Assistant)

A continuación, se detallan los problemas más frecuentes al configurar o utilizar la integración de Panasonic Aquarea en Home Assistant y sus respectivas soluciones.

## 📱 Error de Autenticación / Términos y Condiciones
Este es un error recurrente que impide a la integración conectar con la nube de Panasonic.

**Síntoma:**
La integración deja de funcionar repentinamente o falla al intentar configurarla, mostrando errores en los logs relacionados con la autenticación o problemas de conexión inesperados (referencia: [Issue #9](https://github.com/wpatrik14/home-assistant-aquarea/issues/9)).

**Solución:**
En ocasiones, Panasonic actualiza sus Términos y Condiciones de uso y bloquea el acceso de aplicaciones de terceros hasta que estos sean aceptados explícitamente por el usuario.
Para solucionarlo, sigue estos pasos:
1. **Actualiza la aplicación móvil** oficial de Panasonic (Panasonic Comfort Cloud) a la última versión disponible en tu tienda de aplicaciones.
2. Abre la aplicación e **inicia sesión** utilizando exactamente las **mismas credenciales** de la cuenta que estás usando en Home Assistant.
3. Si hay nuevos **Términos y Condiciones de uso**, la aplicación te pedirá que los aceptes. Confírmalos.
4. Una vez aceptados y con la sesión iniciada correctamente en el móvil, recarga la integración en Home Assistant o vuelve a configurarla.

---

## 🔌 Desconexiones Constantes o Intermitentes
Si la integración se desconecta a menudo, pierde entidades o los comandos no responden de forma consistente.

**Explicación y Solución:**
Para usar correctamente la integración en Home Assistant y asegurar la máxima estabilidad, es altamente **preferible tener una cuenta específica dedicada solo para uso de Home Assistant**.

* Cuando usas la misma cuenta simultáneamente en el móvil y en la integración, las peticiones pueden pisarse y causar bloqueos o desconexiones temporales por parte de los servidores de Panasonic.
* **Importante:** Esta cuenta dedicada debe crearse para la **aplicación del móvil (Panasonic Comfort Cloud)**, como un usuario normal al que luego le compartes el control de la máquina. **No** debe ser una cuenta de Aquarea Service Cloud (uso de instalador).

---

| | | |
|:---|:---:|---:|
| [← Automatizaciones](automatizaciones.md) | [📚 Volver al índice](../README.md) | |
