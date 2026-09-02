---
title: 'CQRS: Patrón de Segregación de Responsabilidades en .NET 10 y React'
category: 01-SemiSenior/Fundamentos-y-Patrones
tags:
  - dotnet10
  - csharp
  - react
  - vite
  - cqrs
  - architecture
updated: '2026-09-02T03:55:57.997Z'
created: '2026-09-02T03:55:57.997Z'
status: Approved
complexity: Semi-Senior
---

# CQRS: Patrón de Segregación de Responsabilidades [Semi-Senior]

## 1. Contexto General & Definición del Concepto
CQRS (Command Query Responsibility Segregation) propone separar las operaciones de lectura (Query) de las de escritura (Command). En sistemas tradicionales, utilizamos el mismo modelo de datos para ambas, lo que genera acoplamiento y cuellos de botella al escalar.

## 2. Forma de Aplicación, Buenas Prácticas & Ventajas
- **Commands:** Mutan el estado del sistema y no devuelven datos de lectura pesados.
- **Queries:** Solo leen y retornan DTOs optimizados para la vista sin efectos secundarios.

## 3. Implementación C# .NET 10 y React
Se implementan handlers independientes con MediatR y hooks en React.
