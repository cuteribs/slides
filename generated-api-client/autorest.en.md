---
marp: true
theme: default
author: 'Eric Liu'
title: 'Generated Api Client/SDK'
paginate: true
size: 16:9
class: invert
footer: 'By [Eric Liu](mailto:li.eric.liu@dnv.com) @ DNV May 2025'
backgroundColor: #10204A
color: #ffffff
---
<!-- 
-->
# Stop Coding API Access by Hand

 _Automating API Client/SDK Generation with Swagger/OpenAPI_

---

<!-- 
header: 'Stop Coding API Access by Hand'
-->

## Agenda

1. **The PITFALLS of hand coding API access**
2. **The MAGIC of API client generation**
3. **Demo Project Overview**
4. **Configuration and Development Process**
5. **Core Advantages of Dapr**
6. **Case Comparison**
7. **Summary and Q&A**

---

<!--
header: 'Applying Dapr in .NET Microservices: From Configuration to Practice - 1. What is Dapr?'
-->

## 1. What is Dapr?

Dapr (Distributed Application Runtime) is an open-source, portable microservices runtime that provides building blocks for distributed systems:
- Service-to-service invocation
- State management
- Pub/Sub
- Observability
- Secrets management
- And 10+ other capabilities...
---

<!--
header: 'Applying Dapr in .NET Microservices: From Configuration to Practice - 2. Why Choose Dapr + .NET?'
-->

## 2. Why Choose Dapr + .NET?

### Perfect Combination
- **.NET Ecosystem**: Native microservices support with ASP.NET Core
- **Dapr Enhancement**: Decouples infrastructure, allowing focus on business logic
- **Development Efficiency**: Reduces 40%+ of boilerplate code (demonstrated later)

### Typical Scenarios
- Multi-language service mixed deployment
- Complex event-driven architecture
- Rapid implementation of resilience patterns (retry/circuit breaker)

---

<!--
header: 'Applying Dapr in .NET Microservices: From Configuration to Practice - 3. Demo Project Overview'
-->

## 3. Demo Project Overview

### Project Structure
```bash
└── src/
    ├── order (ASP.NET Core)
    ├── product (ASP.NET Core)
    └── web (ASP.NET Core)
└── components/  # Dapr component configuration
    ├── pubsub.yaml
    ├── configstore.yaml
    └── statestore.yaml
```

---

### Functional Flow
- `product` retrieves database connection via Dapr (`configstore`)
- User browses products → `web` calls  `product` via Dapr (`service Invocation`)
- User creates/cancels orders → `web` calls `order` via Dapr (`pubsub`)
- `order` starts  `order actor` via Dapr (`actor`) to execute order creation/cancellation tasks
- `order actor` creates/cancels orders via Dapr (`statestore`)
- `order` notifies `web` via Dapr (`pubsub`) 

---

<!--
header: 'Applying Dapr in .NET Microservices: From Configuration to Practice - 4. Configuration and Development Process'
-->

## 4. Configuration and Development Process (Live Demo)

### Step 1: Install Dapr
```bash
# # Quick initialization using Docker
dapr init
```

### Step 2: Add .NET SDK Packages
```bash
dotnet add package Dapr.AspNetCore
dotnet add package Dapr.Extensions.Configuration
dotnet add package Dapr.Actors.AspNetCore
```

### Service Registration (Program.cs)
```csharp
services.AddDaprClient();	// Inject DaprClient
```

---

```csharp
// Register Dapr Actor and enable in ASP.NET Core
services.AddActors(o => o.Actors.RegisterActor<OrderActor>());
app.MapActorsHandlers();
```

```csharp
// Load Dapr Configuration
configuration.AddDaprConfigurationStore(
    "cofigstore", 
    [], 
    new DaprClientBuilder().Build(), 
    TimeSpan.FromSeconds(5)
);
```

```csharp
// Enable Dapr pubsub
app.UseCloudEvents();
app.MapSubscribeHandler();
```

---

### Step 4: Implement Service Invocation
```csharp
// OrderController.cs
app.MapGet("/products", async (DaprClient client) =>
{
	var items = await client.InvokeMethodAsync<ProductItem[]>(
        HttpMethod.Get, 
        "product", 
        "/list"
    );
    return items;
});
```

### Step 5: Run the Service
``` bash
# 启动 OrderService 并附加 Dapr Sidecar
dapr run --app-id orderservice --app-port 5000 dotnet run
```

---

<!--
header: 'Applying Dapr in .NET Microservices: From Configuration to Practice - 5. Core Advantages of Dapr'
-->

## 5. Core Advantages of Dapr
| Traditional Implementation | Dapr Implementation | Benefits |
| --- | --- | --- |
| Manual service discovery | Automatic service discovery | 30% less network layer code |
| Direct dependency on Redis/Kafka | Abstracted via components | Zero code changes for infrastructure switching |
| Custom retry/circuit breaker | Built-in resilience policies | Improved system stability |
| Dispersed monitoring configuration | Unified observability endpoint | 50%+ debugging efficiency improvement |

---

<!--
header: 'Applying Dapr in .NET Microservices: From Configuration to Practice - 6. Case Comparison'
-->

## Case Comparison: Inventory Deduction Logic

### Traditional Implementation
```csharp
// Direct dependency on Redis client
var redis = ConnectionMultiplexer.Connect("localhost");
var db = redis.GetDatabase();
db.StringSet(orderId, "pending");
```

### Dapr Implementation
```csharp
// Abstracted via Dapr state management
await _daprClient.SaveStateAsync(
    "statestore", 
    orderId.ToString(), 
    "pending"
);
```

---

### Advantages
- No need to manage connection strings (configured via components)
- Seamless switching between state stores (Redis/CosmosDB/SQLServer...)
- Automatic concurrency control (ETag)

---

<!--
header: 'Applying Dapr in .NET Microservices: From Configuration to Practice - 7. Summary and Q&A'
-->

## 7. Summary and Q&A

### Key Takeaways
- ✅ Decouple business logic from infrastructure via Sidecar pattern
- ✅ Accelerate distributed system development with standardized building blocks
- ✅ Portability: Easily switch between cloud platforms/local environments

### Next Steps
- Explore Dapr's Actor model
- Try deploying with Kubernetes
- Integrate monitoring metrics (Prometheus + Grafana)

---

### Common Questions:
- What is the performance overhead of Dapr? → Sidecar adds ~2ms latency, optimizable with gRPC
- Is Docker mandatory? → Recommended for development, supports K8s/VM in production
- How to debug Dapr applications? → Use Dapr Dashboard and Zipkin for tracing
