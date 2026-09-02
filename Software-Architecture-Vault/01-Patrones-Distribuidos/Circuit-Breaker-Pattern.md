---
title: "Patrón Circuit Breaker & Resilience con Polly en .NET 10"
category: "Patrones Distribuidos"
tags: [resilience, polly, circuit-breaker, microservices, dotnet10]
updated: "2026-09-02T01:34:29.363Z"
created: "2026-09-02T01:34:29.363Z"
status: "Approved"
complexity: "Senior"
related: ["Transactional-Outbox-Pattern", "CQRS-y-Event-Sourcing"]
---

# 🛡️ Patrón Circuit Breaker (Cortacircuitos)

## 1. Problema y Contexto
En arquitecturas distribuidas y microservicios, las llamadas remotas pueden fallar por sobrecarga, latencia transitoria o caídas completas del servicio downstream. Reintentar agresivamente puede provocar un **efecto en cascada (cascading failure)** y agotar los hilos del consumidor.

## 2. Estados del Circuit Breaker
1. **Closed (Cerrado):** El flujo opera con normalidad. Si la tasa de fallos supera el umbral configurado (ej: 50% de errores en 10s), transiciona a *Open*.
2. **Open (Abierto):** Todas las solicitudes fallan de inmediato con una excepción o fallback (Fast Fail), protegiendo al servicio destino.
3. **Half-Open (Semi-Abierto):** Tras un periodo de enfriamiento (Break Duration), se permite un número controlado de solicitudes de prueba. Si tienen éxito, vuelve a *Closed*; si fallan, retorna a *Open*.

```csharp
// Configuración moderna en .NET 10 con Microsoft.Extensions.Http.Resilience
builder.Services.AddHttpClient("PaymentService")
    .AddStandardResilienceHandler(options =>
    {
        options.CircuitBreaker.SamplingDuration = TimeSpan.FromSeconds(10);
        options.CircuitBreaker.FailureRatio = 0.5;
        options.CircuitBreaker.MinimumThroughput = 8;
        options.CircuitBreaker.BreakDuration = TimeSpan.FromSeconds(30);
    });
```

## 3. Trade-offs y Consideraciones
- **Pros:** Aislamiento de fallos, recuperación automática, degradación elegante con respuestas en caché.
- **Cons:** Requiere diseñar fallbacks coherentes en la capa de negocio y monitorizar métricas de estado (Prometheus/Grafana).
