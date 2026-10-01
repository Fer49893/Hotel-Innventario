# Caso de Uso Escrito — Hotel Innventario

---

### CU-03 · Registrar la revisión física de una habitación

* **Actor principal:**
  `Personal de limpieza`

* **Objetivo:**
  Reportar el conteo físico real de insumos de minibar y prendas de blancos encontrados al terminar de limpiar una habitación.

* **Precondición:**
  El personal de limpieza inició sesión en la aplicación móvil y la habitación seleccionada se encuentra en estado "Pendiente de revisión".

* **Escenario principal:**
  1. El personal de limpieza selecciona la habitación a revisar en el mapa general de habitaciones.
  2. El sistema carga la lista predeterminada de artículos de minibar y blancos según la dotación estándar asignada.
  3. En la pestaña *Minibar*, el usuario ajusta mediante los controles (+ / -) la cantidad exacta de productos encontrados físicamente en el cuarto.
  4. El sistema calcula automáticamente las diferencias y muestra las etiquetas de faltante/consumo (ej. *"Faltante: 1 (Consumo)"*).
  5. El usuario cambia a la pestaña *Blancos*, verifica las piezas existentes y presiona el botón "Guardar Conteo y Notificar a Recepción".
  6. El sistema guarda el registro con fecha, hora y usuario, y actualiza el estado de la habitación a "Pendiente de reposición" (si hay faltantes) o "Completa / Lista".

* **Flujos alternos:**
  * **3a. La habitación está completa:** El usuario verifica que no falta nada, no modifica ninguna cantidad, presiona "Guardar" y el sistema marca la habitación como "Lista para asignar" de forma inmediata.
  * **5a. Se detecta una prenda dañada o sucia:** En la pestaña *Blancos*, el usuario selecciona el botón de estado `Dañado` o `Lavandería` en la prenda correspondiente antes de guardar; el sistema descuenta la pieza de la habitación y actualiza el registro global.
  
* **Postcondición:**
  Se registran los consumos de la habitación, actualizando el mapa de recepción en tiempo real y emitiendo alertas de reposición si existen faltantes.

* **Requisitos que realiza:**
  RF-003, RF-004, RF-006, RNF-USA-001, RNF-REN-001
