# ☁️ Registro y Acceso al Cloud (Usuario e Instalador)

Con la migración de las nuevas aplicaciones de Panasonic, el proceso de obtener privilegios de "Instalador" ha cambiado. Ya no se puede activar directamente desde la web como antes; ahora el flujo de aprobación es a la inversa.

---

## 📱 1. Requisitos previos
* Tener instalada la App **Panasonic Comfort Cloud** en el móvil.
* Tener una cuenta de usuario estándar ya creada y funcionando.
* **Importante:** Si realizas gestiones desde el navegador del móvil, actívalo siempre en **"Modo Escritorio" (Modo ordenador)**, de lo contrario los formularios de Panasonic no cargan correctamente y dan error.

---

## 🛠️ 2. Cómo obtener acceso de Instalador (Service Cloud)

Si quieres acceder a los ajustes avanzados que hemos explicado en este repo, necesitas que tu usuario sea "Administrador/Instalador" de tu propia máquina. El procedimiento actual es el siguiente:

1. **Solicitud desde la App:** Desde la aplicación móvil, busca el apartado de gestión/empresa y busca el nombre de tu "empresa" (o la que hayas creado para ti). Debes enviar una **solicitud de administración**.
PROTIP: si usas gmail, puedes reutilizar el mismo email añadiendo tags, por ejemplo -> app movil -> email@gmail.com. cloud -> email+admincloud@gmail.com. ambos emails son el mismo y te llegaran igual.
2. **Aprobación desde la Web:**
   * Entra en el portal de [Panasonic Pro Club / Service Cloud](https://aquarea-service.panasonic.com/).
   * Loguéate con tus credenciales.
   * Verás una notificación o mensaje de aprobación pendiente. 
   * **Acéptate a ti mismo** como administrador.
[!TIP] PROTIP: Reutiliza tu correo de Gmail con alias Si quieres separar tu cuenta de usuario normal de la de instalador pero usar un solo email, aprovecha los tags de Gmail. Panasonic los detecta como correos distintos, pero los mensajes te llegarán a la misma bandeja:
* App móvil (Usuario): tuemail@gmail.com
* Cloud (Instalador): tuemail+admincloud@gmail.com
3. **Confirmación:** Una vez aceptada la solicitud en la web, tu usuario tendrá permisos totales para ver y editar los parámetros avanzados desde el navegador.

---

## ⚠️ Problemas comunes

### El enlace de aprobación da error
Si al clicar en el enlace de aprobación que llega por email o aparece en el Service Cloud te sale un error de "página no encontrada" o similar:
* Asegúrate de cerrar sesión en todas las pestañas de Panasonic.
* Limpia caché o abre el enlace en **Incógnito**.
* Recuerda lo dicho arriba: si estás en el móvil, fuerza el **"Modo Ordenador"**.

### No aparece mi máquina en el Service Cloud
La máquina debe estar primero registrada en la App de usuario normal (`Comfort Cloud`). Una vez que la ves en el móvil, es cuando puedes iniciar el proceso de "vincularla" al Service Cloud mediante la solicitud de administración.

---

## 🔑 Diferencia entre Apps
* **Panasonic Comfort Cloud:** Uso diario (encender, apagar, subir/bajar temperatura de consigna).
* **Aquarea Service Cloud (Web):** Uso técnico (configurar curvas, ver históricos de errores, ajustar DeltaT y resistencias).

---

[<- Volver al inicio del repositorio](../README.md)