# Análisis de impacto de SC-001

---

## Solicitud que se analiza

Referencia a la solicitud: `sc-001-alertas-existencias-bajas.md`, solicitada por Sebastián Mayorga Galicia (en el papel de dupla de revisión del proyecto Hotel Innventario) el 2026-11-17.

## Traducción a términos del sistema

Lo pedido ("identificar y avisar al personal responsable cuando las existencias de un artículo estén por agotarse, estableciendo un mínimo por artículo") significa en **Hotel Innventario** que la clase `Articulo` requiere un nuevo atributo entero `+Int stockMinimo` junto con la operación `+actualizarStockMinimo(nuevoMinimo) Void` para configurar su umbral de reposición. Asimismo, la clase `InventarioStock` debe incorporar la operación `+verificarStockBajo() Boolean` (que evalúa si `cantidadDisponible <= articulo.stockMinimo` cada vez que se descuentan existencias por revisión o baja por `Merma`) y `+obtenerArticulosStockBajo() List` dentro del **Módulo Inventario y Catálogo** para que *Recepción* y el *Dueño* consulten los artículos en alerta sin necesidad de servicios externos de mensajería.

## Impacto directo

| Artefacto | Unidad | Qué hay que cambiar |
|---|---|---|
| Requisito RF-xx | 2 | Ampliar la gestión de catálogo e inventario (RF de administración de artículos y consultas de stock) para permitir definir un umbral mínimo por artículo y listar productos cuyo saldo sea menor o igual a dicho umbral. |
| Caso de uso | 2 | Actualizar el caso de uso de Gestión de Catálogo/Inventario para incluir la configuración de `stockMinimo` y la consulta de existencias bajas, además del flujo de Revisión de Habitación al descontar stock. |
| Diagrama de clases | 4 | En `Articulo`: agregar `+Int stockMinimo` y `+actualizarStockMinimo(nuevoMinimo) Void`. En `InventarioStock`: agregar `+verificarStockBajo() Boolean` y `+obtenerArticulosStockBajo() List`. |
| Diagrama de secuencia | 4 | En el diagrama de secuencia del caso de uso *Registrar Revisión y Conteo Físico*, agregar una auto-llamada `verificarStockBajo()` en `InventarioStock` inmediatamente después de `descontarStock(cantidad)` dentro de la transacción ACID. |

## Impacto indirecto

| Artefacto | Unidad | Por qué se ve afectado |
|---|---|---|
| Registro de decisión ADR-xx | 3 | Sin impacto. La solución se resuelve como una consulta/indicador interno dentro de la aplicación y la base de datos relacional existente, manteniendo la decisión de aislamiento sin sistemas externos del ADR-001. |
| Diagrama de contenedores C4 | 3 | Sin impacto en contenedores ni tecnologías desplegables (sigue siendo App Cliente + Backend Monolito Modular + PostgreSQL); únicamente a nivel de componentes internos (C4 Nivel 3), el *Módulo Inventario y Catálogo* expone la consulta de alertas hacia los roles *Recepción* y *Dueño*. |
| Máquina de estados | 4 | Sin impacto. El ciclo de vida operativo de `Habitacion` (`Lista`, `Ocupada`, `PendienteRevision`, `PendienteReposicion`, `Mantenimiento`) no cambia; una habitación con faltantes sigue pasando a `PendienteReposicion` independientemente de si el almacén tiene stock bajo. |

## Lo que queda sin tocar

* **El ciclo de vida y estados de `Habitacion`:** No se crean nuevos estados ni transiciones en el Patrón State.
* **La lógica de cobro y cuentas (`Consumo`):** El cálculo de cargos por artículos faltantes en habitación al huésped no depende del nivel de stock mínimo en bodega.
* **La arquitectura de despliegue y modelo de datos transaccional:** No se agregan colas de mensajería, notificaciones push externas (WhatsApp/correo) ni integraciones con proveedores.
* **Los módulos de *Habitaciones e Inspección*, *Consumos y Cuentas* y *Operación y Surtido*** en su estructura base.

## Riesgos

* **Concurrencia al descontar inventario (Probabilidad baja):** Si dos camaristas finalizan revisiones simultáneamente sobre artículos compartidos, una lectura no bloqueada podría omitir la transición al umbral mínimo. *Mitigación:* Ejecutar `verificarStockBajo()` dentro de la misma transacción ACID donde ocurre `descontarStock(cantidad)`.
* **Umbrales mal configurados o en cero (Probabilidad media):** Si el *Dueño* da de alta un artículo nuevo sin definir un `stockMinimo` realista (dejándolo en `0`), el sistema nunca avisará antes de que el producto se agote por completo. *Mitigación:* Establecer un valor predeterminado mayor a cero al registrar artículos en el catálogo.
* **Riesgo de no hacerlo:** El personal seguirá dependiendo de revisiones visuales periódicas en bodega, exponiendo al hotel a quedarse sin insumos críticos (amenidades, sábanas o bebidas de minibar) en días de alta ocupación.

## Alternativas consideradas

1. **Opción A — Evaluación dinámica por umbral en base de datos y vista de alertas en la app (Elegida):** Agregar `stockMinimo` en `Articulo` y calcular dinámicamente qué artículos cumplen `cantidadDisponible <= stockMinimo` al consultar o descontar inventario. Implica cambios mínimos en el modelo de datos, cero infraestructura adicional y recuperación automática en cuanto se registra un reabastecimiento (`incrementarStock`). Deja pendiente únicamente el envío de alertas fuera de la aplicación (como correos o mensajes).
2. **Opción B — Entidad persistente `AlertaStock` con notificaciones push/externas:** Crear una tabla nueva de alertas históricas y conectar un servicio de notificaciones externas para avisar al teléfono del *Dueño*. Implica agregar una nueva entidad al diagrama de clases, gestionar el ciclo de vida de cada alerta (abierta/atendida) y romper la decisión del ADR-001 de mantener el sistema 100% autocontenido, elevando innecesariamente la complejidad para un hotel de 12 habitaciones.

## Estimación de esfuerzo

| Concepto | Esfuerzo |
|---|---|
| Cambios de diseño y documentación | 2 horas |
| Implementación | 4 horas |
| Pruebas | 2 horas |
| **Total** | **8 horas** |

**De dónde sale este número.** Base: *Inventario y Catálogo* ya tiene `incrementarStock`, `descontarStock` y `reubicarExistencias`, y tanto PostgreSQL como las vistas ya están definidas. Agregar una columna, una consulta filtrada y un indicador visual equivale a medio día (4 h) de implementación; más 2 h de pruebas unitarias y de integración y 2 h de diseño (ajustar requisitos, casos de uso y diagramas) = 8 h. No incluye correos, SMS ni órdenes de compra automáticas.

## Recomendación

**Aceptar.** Se recomienda aprobar e incorporar la `SC-001` en el diseño de **Hotel Innventario**, ya que aporta un valor operativo alto para prevenir desabasto en el hotel con un costo de diseño e implementación bajo (8 horas), sin comprometer la arquitectura de monolito modular ni las decisiones establecidas en los ADR.

---
