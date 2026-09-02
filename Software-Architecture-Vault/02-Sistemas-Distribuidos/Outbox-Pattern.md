---
title: Transactional Outbox Pattern
category: Sistemas Distribuidos
tags: [patrones, microservicios, consistencia-eventual, transaccionalidad]
updated: 2026-09-01
---

## 1. Problema y Contexto
Garantizar la publicación atómica de eventos/mensajes hacia un broker cuando se modifica el estado de una base de datos local, evitando inconsistencias por fallos de red o caídas del servicio (Dual-Write problem).

## 2. Componentes y Flujo
1. **Base de Datos Local:** Guarda la entidad de negocio y el evento en la tabla `Outbox` dentro de la misma transacción ACID.
2. **Relay / Message Dispatcher:** Proceso en segundo plano (polling o Change Data Capture - CDC con Debezium) que lee eventos no procesados.
3. **Message Broker:** Destino final del evento (RabbitMQ, Kafka, Azure Service Bus).

## 3. Trade-offs
| Ventajas (Pros) | Desventajas / Costos (Cons) |
|---|---|
| Garantiza entrega *al menos una vez* (At-least-once). | Requiere consumidores idempotentes. |
| Evita transacciones distribuidas complejas (2PC). | Latencia agregada entre persistencia y publicación. |

## 4. Consideraciones Técnicas
- **Concurrencia:** Asegurar bloqueos a nivel de fila (`SELECT ... FOR UPDATE SKIP LOCKED` o transacciones cortas) al leer la tabla de eventos.
- **Idempotencia:** El consumidor debe rastrear identificadores únicos de mensajes procesados.