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