# ADR 001: Selección de Arquitectura Monolito Modular

* **Estado:** Aceptado
* **Fecha:** 2026-10-29
* **Autor:** Jose Fernando Saucedo Balderas

## 1. Contexto
El sistema **Hotel Innventario** está diseñado para gestionar el inventario operativo (minibar y blancos), revisiones y consumos de un hotel boutique de 12 habitaciones. El proyecto es desarrollado por un solo desarrollador y no requiere integraciones con sistemas externos de pago o correo electrónico. Es fundamental garantizar tiempos de respuesta rápidos (sincronización en <= 2 segundos) y mantener una estructura de código organizada que permita el crecimiento futuro del sistema sin añadir sobrecarga en la infraestructura ni en el despliegue.

## 2. Decisión
Se ha decidido adoptar una arquitectura de **Monolito Modular**. Toda la aplicación se desplegará como una única unidad ejecutable, pero internamente el código estará estructurado de manera estricta en cuatro módulos funcionalmente delimitados:
1. Módulo de Habitaciones e Inspección
2. Módulo de Inventario y Catálogo
3. Módulo de Consumos y Cuentas
4. Módulo de Operación y Surtido

## 3. Alternativas Consideradas
* **Monolito Tradicional (No modular):** Fácil de desarrollar inicialmente por una sola persona, pero con alto riesgo de acoplamiento excesivo (código espagueti), lo que dificultaría el mantenimiento y la evolución del sistema.
* **Microservicios:** Permitiría desplegar servicios independientemente, pero introduce una complejidad técnica innecesaria para el alcance actual (comunicación por red, latencia, orquestación de despliegues y gestión de múltiples bases de datos) que no se justifica para un solo desarrollador y un único hotel.

## 4. Consecuencias

### Positivas:
* **Simplicidad de Despliegue:** Se mantiene una sola unidad de despliegue y una única base de datos, ideal para la capacidad de desarrollo de una persona.
* **Aislamiento y Mantenibilidad:** Las fronteras claras entre módulos facilitan la localización de errores y el desarrollo de nuevas características.
* **Preparado para el Futuro:** Si en el futuro un módulo específico (como *Habitaciones e Inspección*) sufre alta latencia o demanda en horas pico, podrá extraerse como un microservicio independiente con un esfuerzo de refactorización mínimo.

### Negativas / Riesgos:
* Requiere disciplina estricta durante el desarrollo para no romper las fronteras entre módulos mediante dependencias directas o llamadas circulares no autorizadas.

## 5. Sustento / Justificación
La decisión se justifica directamente en la **Ley de Conway** y las restricciones del proyecto: al ser desarrollado por un solo desarrollador, una arquitectura de microservicios añadiría fricción operativa sin aportar valor real. El monolito modular satisface el requisito no funcional de rendimiento (`RNF-REN-001`) al procesar todas las llamadas intermodulares en memoria local, eliminando latencias de red y garantizando la sincronización en menos de 2 segundos.
