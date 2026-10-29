# Diagramas de Arquitectura (C4 Model) - Hotel Innventario

## 1. Diagrama de Contexto (Nivel 1)
```mermaid
graph TD
    Limpieza[Personal de Limpieza<br/>Revisa habitaciones y reporta faltantes]
    Recepcion[Recepción<br/>Gestiona surtido y consulta consumos]
    Dueno[Dueño del Hotel<br/>Configura dotaciones y autoriza mermas]

    Sistema[Sistema Hotel Innventario<br/>Control interno de inventario, consumos y mermas]

    Limpieza -->|Inspecciona y registra faltantes| Sistema
    Recepcion -->|Surtido de minibar y consulta de consumos| Sistema
    Dueno -->|Gestiona reglas de inventario y autoriza mermas| Sistema
```
---

## 2. Diagrama de Contenedores (Nivel 2)
```mermaid
graph TD
    UserLimpieza[Personal de Limpieza]
    UserRecepcion[Recepción]
    UserDueno[Dueño]

    subgraph Hotel Innventario System
        WebApp[Aplicación Web / PWA<br/>Figma Design / Frontend<br/>HTTPS]
        API[Servicio Core Backend<br/>API REST / JSON]
        DB[(Base de Datos Relacional<br/>PostgreSQL / Supabase)]
    end

    UserLimpieza -->|HTTPS| WebApp
    UserRecepcion -->|HTTPS| WebApp
    UserDueno -->|HTTPS| WebApp

    WebApp -->|HTTP / JSON REST| API
    API -->|SQL / TCP| DB
```
---

## 3. Diagrama de Componentes (Nivel 3 - Contenedor Core Backend)
```mermaid
graph TD
    subgraph API [Contenedor Core Backend]
        ModHabitaciones[Módulo de Habitaciones e Inspección<br/>Gestiona estados y revisiones]
        ModInventario[Módulo de Inventario y Catálogo<br/>Gestiona artículos, dotaciones y mermas]
        ModConsumos[Módulo de Consumos y Cuentas<br/>Calcula totales a cobrar]
        ModOperacion[Módulo de Operación y Surtido<br/>Gestión de alertas y tareas de resurtido]
    end

    DB[(Base de Datos)]

    ModHabitaciones -->|Consulta dotaciones| ModInventario
    ModHabitaciones -->|Notifica consumos| ModConsumos
    ModHabitaciones -->|Emite alertas de reposición| ModOperacion

    ModOperacion -->|Actualiza stock operativo| ModInventario

    ModHabitaciones -->|Persistencia SQL| DB
    ModInventario -->|Persistencia SQL| DB
    ModConsumos -->|Persistencia SQL| DB
    ModOperacion -->|Persistencia SQL| DB
```
