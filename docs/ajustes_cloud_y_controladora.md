# Explicación de Ajustes: Panasonic Smart Cloud & Controladora

A continuación se detallan los parámetros visibles en la configuración de instalador/usuario avanzado, explicando su **comportamiento real**.

## 🔥 Zone 1 (Calefacción/Refrigeración)

### Operation: Heating
Define el modo de trabajo. Normalmente trabajaremos con **Curva de Compensación** (Water Temperature: Compensation Curve) en lugar de temperatura fija (Direct), para maximizar la eficiencia.

### **Outdoor temperature for heating OFF** (¡Ojo: Traducción Confusa!)
* **Valor en imagen:** `19 °C`
* **Realidad:** Este parámetro está mal traducido en muchas versiones. Realmente define la **Temperatura exterior para HABILITAR la calefacción (Heating ON)**.
* **Funcionamiento:**
  * Si la temperatura exterior baja a 19°C, la máquina se *habilita* (si estaba en espera).
  * La máquina se **APAGA** (corte de calefacción por verano/calor) cuando la temperatura exterior supera el valor configurado **+3°C** (Hysteresis fija, hardcodeada). No es posible modificar este +3.
  * *Ejemplo:* Con 19°C configurados, cortará calefacción cuando fuera haga 22°C.

### **DeltaT for heating ON**
* **Valor en imagen:** `5 °C`
* **Realidad:** Define el diferencial de temperatura objetivo entre la Ida (impulsión) y el Retorno.
* **Comportamiento:**
  * La máquina ajustará la velocidad de la bomba de agua para intentar mantener esta separación de 5 grados.
  * **Importante:** El incumplimiento de este Delta **NO genera paradas** en la máquina. Es un objetivo de regulación de caudal, no un disparador de seguridad/corte. La máquina intentará cumplirlo, pero si no llega, seguirá funcionando.

### Water Temperature: Compensation Curve (Curva de Compensación)
Básicamente, le dice a la máquina: *"cuanto más frío haga fuera, más caliente debe estar el agua de la calefacción"*.

**Ajustes de temperatura de agua (Eje Y(vertical)):**
* **35°C:** Temperatura máxima de impulsión cuando hace mucho frío (pico de invierno).
* **25°C:** Temperatura mínima de impulsión cuando el clima es suave.

**Ajustes de temperatura exterior (Eje X(horizontal)):**
* **5°C:** Punto de "frío intenso". Si fuera hace 5°C o menos, la máquina impulsa a 35°C.
* **15°C:** Punto de "clima suave". Si fuera hace 15°C o más, la máquina baja a 25°C.

*La línea diagonal calcula automáticamente la temperatura intermedia. Si fuera hace 10°C (mitad), impulsará a 30°C.*

---

## ⚡ Heater (Resistencia de Apoyo)

Estos ajustes gestionan la **Resistencia Eléctrica de Apoyo** (Backup Heater), un componente interno (generalmente de 3kW, 6kW o 9kW) que ayuda al compresor cuando este no es capaz de alcanzar la temperatura objetivo por sí solo.

Entender estos parámetros es vital para evitar sustos en la factura eléctrica, ya que queremos que la resistencia funcione lo mínimo indispensable.

### 1. Condición Ambiental (El permiso)
* **Outdoor temperature for heater ON:**
  * **Qué es:** El "portero" de la resistencia. Define la temperatura exterior mínima necesaria para **habilitar** el uso de la resistencia.
  * **Funcionamiento:** Si configuras `-5°C`, la resistencia estará **bloqueada** siempre que fuera haga más de -5°C, sin importar cuánto le cueste a la máquina calentar el agua. Solo si la temperatura exterior baja de ese umbral, la resistencia *podría* entrar (si se cumplen las condiciones de agua abajo descritas).

### 2. Condición Temporal (La paciencia)
* **Heater ON delay time:**
  * **Qué es:** Un temporizador de espera o "tiempo de gracia".
  * **Funcionamiento:** Una vez que se cumplen todas las condiciones para encender la resistencia (hace frío fuera y el agua está fría), la máquina espera este tiempo antes de encenderla realmente.
  * **Objetivo:** Darle una oportunidad al compresor de recuperar la temperatura por sí mismo sin "tirar de lo fácil" (y caro). Un tiempo más largo favorece la eficiencia; un tiempo muy corto prioriza el confort rápido.

### 3. Condiciones de Disparo (El gatillo por temperatura de agua)
Estos dos ajustes definen cuándo entra y sale la resistencia basándose en la desviación real de la temperatura del agua respecto a lo que le has pedido (Consigna).

* **Heater ON DeltaT (of target temp):**
  * **Qué es:** El umbral de desviación para **ENCENDER**.
  * **Funcionamiento:** Define cuántos grados por debajo de la consigna debe caer la temperatura real de impulsión para activar la resistencia.
  * *Ejemplo:* Si tu consigna es 35°C y configuras `-4°C`, la resistencia se activará si el agua que sale de la máquina baja a **31°C** (y ya ha pasado el tiempo de *delay*).

* **Heater OFF DeltaT (of target temp):**
  * **Qué es:** El umbral de aproximación para **APAGAR**.
  * **Funcionamiento:** Define a qué distancia de la consigna se debe apagar la resistencia porque el agua ya está suficientemente caliente y el compresor puede terminar el trabajo.
  * *Ejemplo:* Siguiendo el caso anterior (consigna 35°C), si configuras `-1°C`, la resistencia se apagará en cuanto el agua recupere y llegue a **34°C**, dejando ese último grado de esfuerzo solo al compresor.

---

**💡 Resumen de la lógica:**
Para que la resistencia se encienda, tienen que alinearse los 3 astros:
1. Hacer más frío fuera que el *Outdoor temp*.
2. Que el agua esté más fría que el *ON DeltaT*.
3. Que pase el tiempo del *Delay*.

---

## 💧 Tank (Agua Caliente Sanitaria - ACS)

Gestión del depósito de agua caliente. Aquí es vital entender la priorización de tiempos.

### **Tank Heat up time (Maximum)** y **Room Operation time (Maximum)**
Estos dos ajustes definen el ciclo de trabajo cuando hay demanda simultánea (necesitas ducharte y necesitas calentar la casa a la vez).

* **Tank heat up time (max):** Tiempo máximo seguido que la máquina dedicará a calentar el agua del tanque antes de parar para atender la calefacción (si esta lo pide).
* **Room operation time (max):** Tiempo maximo que la máquina dedicará a la calefacción antes de volver a intentar calentar el ACS.

**El ciclo:**
Si defines 30 min y 30 min, la máquina hará: 30m ACS -> 30m Calefacción -> 30m ACS... así hasta llegar a la consigna de ACS.
*Consejo:* Si pones un tiempo muy alto al ACS (ej: 5 horas), la casa podría enfriarse excesivamente si el tanque tarda mucho en calentar.

### Tank Re-heat Temp
Diferencial para volver a calentar. Si la consigna es 50°C y este valor es -8°C, el ACS saltará cuando el depósito baje a 42°C.

### Sterilization
Configuración de la legionela. Define temperatura y tiempo (ej: 65°C durante 10 min). Suele requerir el uso de la resistencia de apoyo para alcanzar esas temperaturas altas.

---


## 🚀 Modos Especiales (Powerful & Forced)

### Calefacción/ACS Forzada
En teoría, estos modos le dicen a la máquina: *"Ahora que estás activa, haz **SOLO** esto que te pido"*. Actúan como un selector de prioridad absoluta.
* *Ejemplo:* Si tienes Calefacción y ACS encendidos, y el ACS salta por temperatura, la máquina se pondría con el agua caliente. Si activas **Calefacción Forzada**, le dices: "Quieto parado, vuelve a calefaccion e ignora el ACS".
* *Nota:* Según manual, esto debería funcionar así, pero el comportamiento para forzar el uso intencionado de resistencias es complejo.

### Powerful Mode
Fuerza a la máquina a ignorar el COP (eficiencia) y entregar el **100% de potencia** disponible.
* En el caso de usar curvas de compensación, ignora el cálculo exterior y pone como consigna la **temperatura máxima** definida en la curva.
* Útil para calentamientos rápidos al llegar a una casa fría, a costa de un mayor consumo eléctrico.