# Especificación de requisitos

---

- **Sistema:** Hotel Innventario
- **Autor:** Jose Fernando Saucedo Balderas
- **Versión:** 1.3
- **Fecha de la última actualización:** 01/10/2026

---

## 1. Propósito y alcance

- **Propósito del documento:** Definir y detallar de manera formal las especificaciones de requisitos funcionales y no funcionales para el desarrollo del sistema "Hotel Innventario". Este documento sirve de referencia para desarrollar el sistema de forma adecuada y verificar los entregables del prototipo en Figma y los modelos de análisis.
- **Alcance del sistema:** Control digital del inventario operativo para las 12 habitaciones del hotel boutique. Abarca el seguimiento en tiempo real del estado y reposición de productos del minibar (alimentos y bebidas) y blancos (sábanas, toallas, cobijas, almohadas). Soporta roles para personal de recepción, personal de limpieza y el dueño del hotel, permitiendo el registro de revisiones físicas, consulta de estado por habitación, alertas de reposición, seguimiento de prendas en lavandería externa o dañadas, y el reporte consolidado para compras.
- **Fuera del alcance:** Control de reservaciones de habitaciones, cobro de tarifas de hospedaje, facturación electrónica (CFDI), gestión de nómina, órdenes de compra automáticas a proveedores y aplicaciones o interfaces orientadas hacia el uso directo de los huéspedes.

---

## 2. Usuarios y su contexto

| Usuario | Qué hace hoy sin el sistema | Qué espera del sistema |
| :--- | :--- | :--- |
| **Personal de recepción** | Revisa hojas físicas en papel entregadas manualmente o recibe fotos informales por mensajería instantánea para verificar si hubo consumos de minibar antes del check-out de un huésped. | Consultar en tiempo real los consumos reportados por limpieza para cobrarlos correctamente durante el check-out, y verificar que la dotación de blancos esté completa antes de asignar la habitación a un nuevo cliente. |
| **Personal de limpieza** | Registra a mano en hojas impresas la revisión física de blancos y productos de minibar al terminar la limpieza de cada habitación, teniendo que bajar corriendo a recepción para entregarlas. | Una interfaz móvil simple e intuitiva que le permita registrar faltantes, consumos y estados de blancos en un máximo de tres pasos sin complicaciones técnicas. |
| **Dueño del hotel** | Revisa manualmente libretas u hojas sueltas para estimar cuántos insumos hay disponibles en almacén o lavandería, sufriendo desabastos o mermas no justificadas. | Un reporte consolidado del inventario general en tiempo real para planear compras con datos reales y autorizar de forma exclusiva las bajas definitivas de blancos destruidos o perdidos. |

**Conflictos identificados entre usuarios:**
El personal de recepción requiere liberar las habitaciones lo más rápido posible para no hacer esperar a los clientes en check-in, mientras que el dueño exige que no se asigne ninguna habitación si carece de algún blanco básico. Asimismo, el personal de limpieza necesita indicar cuando una prenda no está en la habitación por encontrarse en lavandería externa o estar dañada, lo cual podría distorsionar la disponibilidad del stock si no se cuenta con estados intermedios que el dueño pueda auditar.

---

## 3. Requisitos funcionales

### 3.1 Resumen

| ID | Nombre | Prioridad | Origen |
| :--- | :--- | :--- | :--- |
| **RF-001** | Autenticación de usuarios por rol | Imprescindible | Regla de negocio / Visión del producto |
| **RF-002** | Gestión de dotación estándar | Imprescindible | Entrevista  |
| **RF-003** | Registro de conteo físico en minibar | Imprescindible | Entrevista / Contexto de limpieza |
| **RF-004** | Verificación y registro de blancos | Imprescindible | Entrevista / Contexto de limpieza |
| **RF-005** | Consulta de consumos para check-out | Imprescindible | Entrevista / Recepción |
| **RF-006** | Monitor del estado de habitaciones | Imprescindible | Visión del producto / Operación |
| **RF-007** | Confirmación de reposición de insumos | Importante | Visión del producto |
| **RF-008** | Clasificación y trazabilidad de blancos | Imprescindible | Regla de negocio / Auditoría |
| **RF-009** | Reporte de inventario consolidado general | Importante | Visión del producto / Dueño |
| **RF-010** | Autorización y auditoría de bajas por merma | Imprescindible | Regla de negocio / Dueño |

---

### 3.2 Fichas

#### RF-001 · Autenticación de usuarios por rol

| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema debe permitir el acceso mediante contraseña e identificar el rol del usuario (Personal de Limpieza, Recepción o Dueño del Hotel), desplegando únicamente las vistas y permisos correspondientes. |
| **Origen** | Regla de negocio y Visión del producto. |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | Al ingresar la contraseña y seleccionar el rol, el sistema redirige al usuario a su panel correspondiente. Si la contraseña es incorrecta o no coincide con el rol seleccionado, el sistema niega el acceso y muestra un mensaje de advertencia. |
| **Relacionado con** | RNF-SEG-001, RF-003, RF-005, RF-009 |

#### RF-002 · Gestión de dotación estándar

| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema debe permitir únicamente al rol de Dueño del hotel configurar y modificar la cantidad base estándar de insumos de minibar y prendas de blancos que debe tener cada una de las 12 habitaciones. |
| **Origen** | Entrevista con stakeholder y Visión del producto. |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | Al modificar las cantidades base desde el panel de administración y presionar "Guardar Cambios", el sistema actualiza la plantilla de referencia para todas las revisiones futuras. Si se intenta guardar un valor negativo, el sistema bloquea la acción. |
| **Relacionado con** | RF-003, RF-004, RF-009 |

#### RF-003 · Registro de conteo físico en minibar

| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema debe permitir al personal de limpieza registrar el número físico real de productos de minibar encontrados en una habitación utilizando botones de incremento y decremento (+ / -). |
| **Origen** | Entrevista y contexto operativo de limpieza. |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | Al ajustar las cantidades encontradas frente a la dotación esperada y presionar "Guardar Conteo", el sistema calcula automáticamente las diferencias, registra el consumo de la habitación y lo notifica a recepción. |
| **Relacionado con** | RF-005, RF-006, RNF-USA-001 |

#### RF-004 · Verificación y registro de blancos

| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema debe permitir al personal de limpieza verificar y reportar las piezas existentes de blancos (sábanas, toallas, fundas y cobijas) al finalizar la limpieza de un cuarto. |
| **Origen** | Entrevista y contexto operativo de limpieza. |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | Al confirmar las piezas de ropa de cama presentes en la habitación, el sistema registra el estado de los blancos y genera una alerta a recepción si faltan unidades requeridas. |
| **Relacionado con** | RF-006, RF-008, RNF-USA-001 |

#### RF-005 · Consulta de consumos para check-out

| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema debe calcular y mostrar al personal de recepción la lista detallada y el acumulado de productos consumidos del minibar por habitación para su cobro en caja durante la salida del huésped. |
| **Origen** | Entrevista con el personal de recepción. |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | Al seleccionar el número de habitación desde la pestaña de consumos, el sistema despliega el desglose exacto de los artículos faltantes listos para su cobro y permite marcarlos como cobrados/liquidados. |
| **Relacionado con** | RF-003, RNF-REN-001 |

#### RF-006 · Monitor del estado de habitaciones

| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema debe mostrar en tiempo real la disponibilidad de las 12 habitaciones mediante un semáforo visual de estados (Lista / Completa, Pendiente de revisión, Pendiente de reposición). |
| **Origen** | Visión del producto y necesidades operativas de recepción. |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | El mapa de habitaciones actualiza dinámicamente sus colores e indicadores en menos de 2 segundos tras cada guardado de limpieza o reposición de recepción. |
| **Relacionado con** | RF-003, RF-004, RF-007, RNF-REN-001 |

#### RF-007 · Confirmación de reposición de insumos

| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema debe permitir registrar el reabastecimiento de insumos o blancos faltantes en una habitación marcándola como resurtida. |
| **Origen** | Visión del producto. |
| **Prioridad** | Importante |
| **Criterio de aceptación** | Al confirmar que los productos o blancos faltantes ya fueron colocados en la habitación, el sistema borra la alerta activa y actualiza el estado del cuarto a "Completa / Lista para asignar". |
| **Relacionado con** | RF-006, RNF-REN-001 |

#### RF-008 · Clasificación y trazabilidad de blancos

| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema debe permitir cambiar el estado de las prendas de blancos a clasificaciones como "En uso", "En lavandería externa" o "Dañado / Merma". |
| **Origen** | Regla de negocio e inspección de procesos de lavandería. |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | Al seleccionar un estado como "Lavandería" o "Dañado", el artículo descuenta su presencia de la habitación pero mantiene su registro rastreable en el inventario consolidado global. |
| **Relacionado con** | RF-004, RF-009, RF-010 |

#### RF-009 · Reporte de inventario consolidado general

| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema debe generar para el perfil de administración un resumen general de las existencias del hotel dividiendo el stock entre habitaciones, almacén central, lavandería y mermas. |
| **Origen** | Visión del producto y entrevista con el dueño. |
| **Prioridad** | Importante |
| **Criterio de aceptación** | Al ingresar con el rol de Dueño, el dashboard despliega los indicadores numéricos del stock total y barras de disponibilidad por categoría en tiempo real. |
| **Relacionado con** | RF-008, RF-010, RNF-SEG-001 |

#### RF-010 · Autorización y auditoría de bajas por merma

| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema debe permitir únicamente al usuario con rol de Dueño autorizar la baja definitiva de artículos marcados como mermas o destruidos, exigiendo la captura del motivo. |
| **Origen** | Regla de negocio y auditoría de activos del hotel. |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | Para autorizar una baja definitiva, el sistema exige ingresar una justificación en texto; de lo contrario, el botón de confirmación permanece inhabilitado. |
| **Relacionado con** | RF-008, RF-009, RNF-SEG-001 |

---

## 4. Requisitos no funcionales

### 4.1 Resumen

| ID | Atributo | Nombre | Prioridad | Origen |
| :--- | :--- | :--- | :--- | :--- |
| **RNF-SEG-001** | Seguridad | Control de acceso basado en roles | Imprescindible | Regla de negocio / Visión del producto |
| **RNF-USA-001** | Usabilidad | Eficiencia operativa en interfaz móvil | Imprescindible | Contexto operativo de limpieza |
| **RNF-REN-001** | Rendimiento | Tiempo de respuesta y sincronización | Imprescindible | Contexto operativo de check-out |

---

### 4.2 Fichas

#### RNF-SEG-001 · Control de acceso basado en roles

| Campo | Contenido |
| :--- | :--- |
| **Atributo de calidad** | Seguridad |
| **Descripción** | El sistema debe restringir el acceso a las funciones mediante autenticación, garantizando que el personal de limpieza solo pueda registrar revisiones de habitaciones, mientras que únicamente el usuario con rol de dueño debe tener permisos para consultar el inventario general consolidado y autorizar bajas por merma. |
| **Métrica** | 100% de los intentos de acceso a vistas consolidadas o eliminación de artículos realizados por usuarios con rol de Limpieza o Recepción deben ser bloqueados por la aplicación. |
| **Origen** | Regla de negocio y atributo de seguridad declarado en Visión del producto. |
| **Prioridad** | Imprescindible |
| **Por qué importa** | Evita la manipulación no autorizada del inventario global y previene la eliminación maliciosa o accidental de registros sobre pérdidas o mermas de insumos. |
| **Afecta a** | RF-001, RF-009, RF-010 |

#### RNF-USA-001 · Eficiencia operativa en interfaz móvil

| Campo | Contenido |
| :--- | :--- |
| **Atributo de calidad** | Usabilidad |
| **Descripción** | La interfaz de usuario debe permitir al personal de limpieza completar el registro de revisión de una habitación en un máximo de tres pantallas o pasos, siendo navegable desde dispositivos móviles sin requerir capacitación técnica formal previa. |
| **Métrica** | Medición de la tarea "Registrar revisión": completada en $\le 3$ pantallas desde la selección de la habitación hasta la confirmación final por usuarios de prueba sin capacitación. |
| **Origen** | Necesidades del personal operativo e inspección en campo. |
| **Prioridad** | Imprescindible |
| **Por qué importa** | La revisión ocurre durante el flujo ágil de limpieza. Si la interfaz es compleja, el personal abandonará el sistema móvil y volverá al uso de registros impresos en papel. |
| **Afecta a** | RF-003, RF-004 |

#### RNF-REN-001 · Tiempo de respuesta y sincronización

| Campo | Contenido |
| :--- | :--- |
| **Atributo de calidad** | Rendimiento / Disponibilidad |
| **Descripción** | El sistema debe procesar y reflejar los registros de consumos y cambios de estado en el módulo de recepción en un tiempo no mayor a 2 segundos bajo condiciones normales de red, asegurando información actualizada durante el check-out. |
| **Métrica** | Tiempo de latencia $\le 2.0$ segundos transcurridos desde que se presiona "Guardar" en la aplicación móvil hasta que los datos están visibles en la pantalla de recepción. |
| **Origen** | Análisis de la transacción crítica de check-out en recepción. |
| **Prioridad** | Imprescindible |
| **Por qué importa** | Si el tiempo de sincronización excede este límite, el cliente abandona la recepción antes de que los consumos sean visualizados, provocando fugas de ingresos no cobrados. |
| **Afecta a** | RF-005, RF-006, RF-007 |

---

## 5. Casos de uso

### 5.1 Caso de Uso Escrito Completo

#### CU-03 · Registrar la revisión física de una habitación

* **Actor principal:** Personal de limpieza
* **Objetivo:** Reportar el conteo físico real de insumos de minibar y prendas de blancos encontrados al terminar de limpiar una habitación.
* **Precondición:** El personal de limpieza inició sesión en la aplicación móvil y la habitación seleccionada se encuentra en estado "Pendiente de revisión".
* **Escenario principal:**
  1. El personal de limpieza selecciona la habitación a revisar en el mapa general de habitaciones.
  2. El sistema carga la lista predeterminada de artículos de minibar y blancos según la dotación estándar asignada.
  3. En la pestaña *Minibar*, el usuario ajusta mediante los controles (+ / -) la cantidad exacta de productos encontrados físicamente en el cuarto.
  4. El sistema calcula automáticamente las diferencias y muestra las etiquetas de faltante/consumo (ej. *"Faltante: 1 (Consumo)"*).
  5. El usuario cambia a la pestaña *Blancos*, verifica las piezas existentes y presiona el botón "Guardar Conteo y Notificar a Recepción".
  6. El sistema guarda el registro con fecha, hora y usuario, y actualiza el estado de la habitación a "Pendiente de reposición" (si hay faltantes) o "Completa / Lista".
* **Flujos alternos:**
  * **3a. La habitación está completa:** El usuario verifica que no falta nada, no modifica ninguna cantidad, presiona "Guardar" y el sistema marca la habitación como "Lista para asignar" de forma inmediata.
  * **5a. Se detecta una prenda dañada o sucia:** En la pestaña *Blancos*, el usuario selecciona el botón de estado Dañado o Lavandería en la prenda correspondiente antes de guardar; el sistema descuenta la pieza de la habitación y actualiza el registro global.
  * **5b. Interrupción por falta de conexión a red:** El sistema guarda el registro localmente en el dispositivo y reintenta la sincronización en segundo plano mostrando el mensaje: *"Registro guardado offline. Sincronizando..."*.
* **Postcondición:** Se registran los consumos de la habitación, actualizando el mapa de recepción en tiempo real y emitiendo alertas de reposición si existen faltantes.
* **Requisitos que realiza:** RF-003, RF-004, RF-006, RNF-USA-001, RNF-REN-001

---

### 5.2 Resumen de Casos de Uso

| ID | Caso de Uso | Actor Principal | Requisitos que Realiza |
| :--- | :--- | :--- | :--- |
| **CU-01** | Autenticar usuario por rol | Todos los roles | RF-001, RNF-SEG-001 |
| **CU-02** | Definir la dotación estándar por habitación | Dueño del hotel | RF-002 |
| **CU-03** | Registrar la revisión física de una habitación | Personal de limpieza | RF-003, RF-004, RF-006, RNF-USA-001, RNF-REN-001 |
| **CU-04** | Consultar consumos para check-out | Recepcionista | RF-005, RNF-REN-001 |
| **CU-05** | Consultar el estado del inventario por habitación | Recepcionista / Limpieza | RF-005, RF-006 |
| **CU-06** | Registrar la reposición de insumos | Personal de limpieza / Recepción | RF-006, RF-007, RNF-REN-001 |
| **CU-07** | Registrar el cambio de estado en blancos | Personal de limpieza | RF-008 |
| **CU-08** | Consultar el inventario consolidado general | Dueño del hotel | RF-009, RNF-SEG-001 |
| **CU-09** | Autorizar la baja por merma | Dueño del hotel | RF-010, RNF-SEG-001 |

---

## 6. Trazabilidad

| Requisito | Origen | Caso de uso | Elemento del prototipo |
| :--- | :--- | :--- | :--- |
| **RF-001** | Regla de negocio / Visión | CU-01 Autenticar usuario por rol | Pantalla de Login unificada por roles |
| **RF-002** | Entrevista / Stakeholder | CU-02 Definir dotación estándar | Panel de administración de Dotación Estándar |
| **RF-003** | Entrevista / Limpieza | CU-03 Registrar revisión física | Formulario móvil de revisión (Pestaña Minibar) |
| **RF-004** | Entrevista / Limpieza | CU-03 Registrar revisión física | Formulario móvil de revisión (Pestaña Blancos) |
| **RF-005** | Entrevista / Recepción | CU-04 Consultar consumos check-out | Módulo de Consumos por Habitación (Recepción) |
| **RF-006** | Visión del producto | CU-05 Consultar estado / CU-06 Reposición | Monitor general de habitaciones (Semáforo) |
| **RF-007** | Visión del producto | CU-06 Registrar reposición | Pantalla de Refill Minibar y Confirmación |
| **RF-008** | Regla de negocio | CU-07 Registrar cambio estado blancos | Selectores de estado (`En uso`, `Lavandería`, `Dañado`) |
| **RF-009** | Visión del producto / Dueño | CU-08 Consultar inventario consolidado | Dashboard Consolidado Ejecutivo |
| **RF-010** | Regla de negocio / Dueño | CU-09 Autorizar baja por merma | Módulo de Autorización de Bajas por Merma |
| **RNF-SEG-001** | Regla de negocio | CU-01, CU-08, CU-09 | Middleware de autenticación y vistas por rol |
| **RNF-USA-001** | Contexto de limpieza | CU-03 Registrar revisión física | Layout móvil simplificado en $\le 3$ pasos |
| **RNF-REN-001** | Transacción check-out | CU-03, CU-04, CU-06 | Componente de sincronización ($\le 2$s) |

---

## 7. Registro de cambios

| Fecha | Requisito | Qué cambió | Por qué |
| :--- | :--- | :--- | :--- |
| 24/09/2026 | Todos | Creación de la versión 1.0 del documento | Consolidación de especificaciones iniciales para la semana 8. |
| 01/10/2026 | Secciones 3, 5 y 6 | Actualización a versión 1.3: Estructuración formal de las fichas de los 10 RF, inclusión del CU-03 redactado y sincronización de matriz de trazabilidad con Figma. | Refinamiento y preparación para entrega formal de prototipo y documentación. |

---

## Antes de entregar

- [x] Todos los requisitos tienen identificador único y ninguno está repetido
- [x] Cada requisito expresa una sola idea
- [x] Cada requisito funcional tiene criterio de aceptación comprobable
- [x] Cada requisito no funcional tiene una métrica, no solo un adjetivo
- [x] El campo Origen distingue lo confirmado por el cliente de lo que sigo suponiendo
- [x] Hay al menos un requisito no funcional por cada atributo de calidad que impone mi tipo de sistema
- [x] Ningún requisito impone una solución técnica
- [x] Todos los requisitos caben dentro del alcance declarado
- [x] La tabla de trazabilidad está completa
- [x] Mi dupla revisó el documento y su revisión está registrada
- [x] Borré los ejemplos y las instrucciones en cursiva
