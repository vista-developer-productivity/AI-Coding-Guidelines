# Observability Standards

## Tech Radar Alignment

| Tool | Status | Use For |
|------|--------|---------|
| **OpenTelemetry** | ⚠️ Trial | Standard instrumentation for traces, metrics, logs |
| **New Relic APM** | ✅ Adopt | Application performance monitoring |
| **New Relic One** | ✅ Adopt | Observability platform |
| **Deployment Notifications** | ✅ Adopt | Deploy event tracking |
| **AWS CloudWatch** | ✅ Adopt | AWS-native monitoring and alerting |
| **Bugsnag** | ✅ Adopt | Error tracking and monitoring |
| DataDog | ❌ Hold | Do not adopt for new projects |
| Wormly | 🚫 Remove | Decommission |
| Sentry | 🚫 Remove | Decommission |
| Catchpoint | 🚫 Remove | Decommission |
| Tailscale | 🚫 Remove | Decommission |

## Structured Logging

- Use **structured JSON logging** with consistent format across all services
- Include: timestamps, severity levels, correlation IDs, service name
- Use appropriate log levels: ERROR, WARN, INFO, DEBUG
- **Never log**: passwords, tokens, PII, credit card numbers

### Correlation & Tracing

- Generate and propagate unique **request IDs** for distributed tracing
- Use OpenTelemetry SDK for automatic context propagation
- Implement **dynamic sampling** for logs and metrics to manage volume

## Instrumentation

- Instrument code with **metrics, logs, and traces** using OpenTelemetry
- Set up automated alerts for anomalies and outages
- Monitor key business metrics and SLAs
- Implement **health checks** for all services (`/health`, `/ready`)
- Use dashboards for real-time monitoring (New Relic, CloudWatch)

## Language-Specific Patterns

### TypeScript/Node.js
```typescript
// Use structured logger with correlation ID
logger.info('Order processed', {
  orderId,
  correlationId: getCorrelationId(),
  duration: processingTime
})
```

### Go
```go
// Use structured logging (slog or zerolog)
slog.Info("order processed",
    "orderId", orderId,
    "correlationId", correlationID,
    "duration", processingTime,
)
```

### C#/.NET
```csharp
// Use LoggerMessage source generation
[LoggerMessage(EventId = 1, Level = LogLevel.Information,
    Message = "Order {OrderId} processed in {Duration}ms")]
private partial void LogOrderProcessed(string orderId, long duration);
```
