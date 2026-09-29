# Especificación de requisitos

---

- **Sistema:** Hotel Innventario
- **Autor:** Jose Fernando Saucedo Balderas
- **Versión:** 1.0
- **Fecha de la última actualización:** 24/09/2026

---

## 1. Propósito y alcance

- **Propósito del documento:** Definir y detallar de manera formal las especificaciones de requisitos funcionales y no funcionales para el desarrollo del sistema "Hotel Innventario". Este documento sirve de referencia para poder desarrollar el sistema de forma adecuada y verificar los entregables del prototipo.
- **Alcance del sistema:** Control digital del inventario operativo para las 12 habitaciones del hotel boutique. Abarca el seguimiento en tiempo real del estado y reposición de productos del minibar (alimentos y bebidas) y blancos (sábanas, toallas, cobijas y almohadas). Soporta roles para personal de recepción, personal de limpieza y el dueño del hotel, permitiendo el registro de revisiones físicas, consulta de estado por habitación, alertas de reposición, seguimiento de prendas en lavandería externa o dañadas, y el reporte consolidado para compras.
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
| RF-001 | Registar inventario físico | Imprescindible | Entrevista / Visión del producto |
| RF-002 | Consultar estado por habitación | Imprescindible | Entrevista / Visión del producto |
| RF-003 | Mandar alertas de reposición | Importante | Visión del producto / Elicitación de requisitos |
| RF-004 | Registrar bajas por estado | Imprescindible | Entrevista / Regla de negocio |
| RF-005 | Consultar inventario consolidado | Importante | Visión del producto / Entrevista |

### 3.2 Fichas

#### RF-001 · Registro de inventario físico

| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema debe permitir al personal de limpieza registrar el inventario físico encontrado de blancos y productos de minibar al momento de revisar una habitación. |
| **Origen** | Entrevista y Visión del producto. |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | Dado que la camarera selecciona una habitación limpia, cuando ajusta los consumos de minibar y faltantes de blancos frente a la dotación estándar y presiona "Guardar", el sistema registra la revisión y notifica los cargos pendientes a recepción. Si falta algún campo obligatorio por seleccionar, el sistema muestra un mensaje de advertencia y no procesa el registro. |
| **Relacionado con** | RF-002, RF-003, RNF-USA-001, RNF-REN-001 |

#### RF-002 · Consulta de estado por habitación

| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema debe permitir al personal de recepción consultar en tiempo real el detalle de los artículos y consumos registrados en una habitación específica. |
| **Origen** | Entrevista con el personal de recepción (septiembre 2026). |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | Al ingresar el número de habitación desde la pantalla de recepción, el sistema despliega el desglose exacto de los productos consumidos del minibar listos para cobro y el estado actual de los blancos (Completo o Incompleto). |
| **Relacionado con** | RF-001, RNF-REN-001, RNF-SEG-001 |

#### RF-003 · Alerta de reposición

| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema debe generar automáticamente una lista de reposición cuando el inventario de una habitación esté por debajo de la dotación estándar establecida. |
| **Origen** | Visión del producto y validación con stakeholder. |
| **Prioridad** | Importante |
| **Criterio de aceptación** | Al registrarse un consumo de minibar o faltante de blancos en el registro de limpieza, la habitación cambia automáticamente a estado "Pendiente de reposición" y aparece listada en el panel de alertas de recepción y surtido con el detalle de los artículos faltantes. |
| **Relacionado con** | RF-001, RF-002 |

#### RF-004 · Registro de bajas por estado

| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema debe permitir cambiar el estado de un artículo de blancos a "En lavandería" o "Dañado" para actualizar el stock disponible sin eliminar el registro del artículo. |
| **Origen** | Regla de negocio identificada en entrevista y Visión del producto. |
| **Prioridad** | Imprescindible |
| **Criterio de aceptación** | Al marcar un blanco como "En lavandería" o "Dañado", el artículo descuenta su presencia en la habitación asignada pero mantiene su trazabilidad en el inventario global, impidiendo que el personal no autorizado lo elimine definitivamente del sistema. |
| **Relacionado con** | RF-005, RNF-SEG-001 |

#### RF-005 · Consulta de inventario consolidado

| Campo | Contenido |
| :--- | :--- |
| **Descripción** | El sistema debe permitir al perfil del dueño consultar el reporte de existencias totales del inventario general del hotel. |
| **Origen** | Visión del producto y entrevistas operativas. |
| **Prioridad** | Importante |
| **Criterio de aceptación** | Al ingresar con el rol de Dueño, la pantalla principal de administración muestra un resumen de existencias globales clasificadas por: en uso en habitaciones, en almacén central, en lavandería externa y marcados como dañados. |
| **Relacionado con** | RF-004, RNF-SEG-001 |

---

## 4. Requisitos no funcionales

### 4.1 Resumen

| ID | Atributo | Nombre | Prioridad | Origen |
| :--- | :--- | :--- | :--- | :--- |
| RNF-SEG-001 | Seguridad | Control de acceso basado en roles | Imprescindible | Visión del producto / Regla de negocio |
| RNF-USA-001 | Usabilidad | Eficiencia operativa en interfaz | Imprescindible | Contexto operativo de limpieza |
| RNF-REN-001 | Rendimiento | Tiempo de respuesta y sincronización | Imprescindible | Contexto operativo de check-out |

### 4.2 Fichas

#### RNF-SEG-001 · Control de acceso basado en roles

| Campo | Contenido |
| :--- | :--- |
| **Atributo de calidad** | Seguridad |
| **Descripción** | El sistema debe restringir el acceso a las funciones mediante autenticación, garantizando que el personal de limpieza solo pueda registrar y modificar revisiones de habitaciones, mientras que únicamente el usuario con rol de dueño debe tener permisos para consultar el inventario general consolidado y autorizar bajas de artículos. |
| **Métrica** | 100% de los intentos de acceso a vistas consolidadas o eliminación de artículos realizados por usuarios con rol de Limpieza o Recepción deben ser bloqueados por la aplicación. |
| **Origen** | Regla de negocio y atributo de seguridad declarado en Visión del producto. |
| **Prioridad** | Imprescindible |
| **Por qué importa** | Evita la manipulación no autorizada del inventario global y previene la eliminación maliciosa o accidental de registros sobre pérdidas o mermas de insumos. |
| **Afecta a** | RF-001, RF-002, RF-004, RF-005 |

#### RNF-USA-001 · Eficiencia operativa en interfaz

| Campo | Contenido |
| :--- | :--- |
| **Atributo de calidad** | Usabilidad |
| **Descripción** | La interfaz de usuario debe permitir al personal de limpieza completar el registro de revisión de una habitación en un máximo de tres pasos o pantallas, siendo navegable desde dispositivos móviles sin requerir capacitación técnica formal previa. |
| **Métrica** | Medición de la tarea "Registrar revisión": completada en $\le 3$ pantallas desde el inicio de la acción hasta la confirmación final por usuarios de prueba sin capacitación. |
| **Origen** | Necesidades del personal operativo e inspección en campo. |
| **Prioridad** | Imprescindible |
| **Por qué importa** | La revisión ocurre durante el flujo ágil de limpieza. Si la interfaz es compleja, el personal abandonará el sistema móvil y volverá al uso de registros impresos en papel. |
| **Afecta a** | RF-001 |

#### RNF-REN-001 · Tiempo de respuesta y sincronización

| Campo | Contenido |
| :--- | :--- |
| **Atributo de calidad** | Rendimiento / Disponibilidad |
| **Descripción** | El sistema debe procesar y reflejar los registros de consumos y cambios de estado en la base de datos central en un tiempo no mayor a 2 segundos bajo condiciones normales de red, asegurando que recepción disponga de la información actualizada durante el proceso de check-out. |
| **Métrica** | Tiempo de latencia $\le 2.0$ segundos transcurridos desde que se presiona "Guardar" en la aplicación móvil hasta que los datos están visibles en el módulo de recepción. |
| **Origen** | Análisis de la transacción crítica de check-out en recepción. |
| **Prioridad** | Imprescindible |
| **Por qué importa** | Si el tiempo de sincronización excede este límite, el cliente abandona la recepción antes de que los consumos sean visualizados, provocando fugas de ingresos no cobrados. |
| **Afecta a** | RF-001, RF-002 |

---

## 5. Casos de uso

*Los casos de uso detallados se incorporarán formalmente en la semana 7 tras el modelado y refinamiento de interacciones. A continuación se resume la vinculación proyectada:*

* **CU-01 · Registrar revisión de habitación:** Realiza **RF-001**, **RF-003** y cumple con **RNF-USA-001**.
* **CU-02 · Reportar cambio de estado de blancos:** Realiza **RF-004**.
* **CU-03 · Consultar consumos para Check-Out:** Realiza **RF-002** y cumple con **RNF-REN-001**.
* **CU-04 · Consultar inventario consolidado:** Realiza **RF-005** y cumple con **RNF-SEG-001**.

---

## 6. Trazabilidad

| Requisito | Origen | Caso de uso | Elemento del prototipo |
| :--- | :--- | :--- | :--- |
| **RF-001** | Entrevista / Visión del producto | CU-01 Registrar revisión de habitación | Pantalla móvil de revisión de habitación |
| **RF-002** | Entrevista / Módulo Recepción | CU-03 Consultar consumos para Check-Out | Vista de mapa y detalle de habitación |
| **RF-003** | Visión del producto | CU-01 Registrar revisión / CU-03 Consultar | Módulo de alertas de reposición |
| **RF-004** | Entrevista / Regla de negocio | CU-02 Reportar cambio de estado de blancos | Menú desplegable de estado de prenda |
| **RF-005** | Visión del producto / Entrevista | CU-04 Consultar inventario consolidado | Dashboard administrador (Dueño) |
| **RNF-SEG-001** | Regla de negocio / Visión del producto | Todos los Casos de Uso | Pantalla de Login / Middleware de roles |
| **RNF-USA-001** | Contexto operativo de limpieza | CU-01 Registrar revisión de habitación | Layout simplificado de 3 pasos (móvil) |
| **RNF-REN-001** | Transacción crítica de Check-out | CU-01 y CU-03 | Componente de sincronización en tiempo real |

---

## 7. Registro de cambios

| Fecha | Requisito | Qué cambió | Por qué |
| :--- | :--- | :--- | :--- |
| 24/09/2026 | Todos | Creación de la versión 1.0 del documento | Consolidación de especificaciones para la semana 8. |

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
