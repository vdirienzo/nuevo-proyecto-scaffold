# Observability Stack Templates

> Templates for production-grade observability infrastructure
>
> **Project:** nuevo-proyecto-scaffold
> **Author:** Homero Thompson del Lago del Terror

---

## Overview

This directory contains templates for a complete observability stack including:

- **Metrics**: Prometheus, OpenTelemetry, Custom metrics
- **Logging**: Loki, Promtail, Pino (Node.js), Loguru (Python)
- **Tracing**: Jaeger, Tempo, OpenTelemetry
- **Dashboards**: Grafana with pre-built dashboards
- **Error Tracking**: Sentry (client, server, edge, Python)
- **SLO Monitoring**: Service Level Objectives and Error Budgets
- **Alerting**: Prometheus alerting rules

---

## Directory Structure

```
observability/
├── otel/                          # OpenTelemetry
│   ├── otel-collector.yaml.tmpl   # Collector config
│   ├── tracing.ts.tmpl             # Node.js instrumentation
│   ├── tracing.py.tmpl             # Python instrumentation
│   └── metrics.ts.tmpl             # Custom metrics
│
├── prometheus/                     # Prometheus
│   ├── prometheus.yml.tmpl         # Server config
│   └── alerts.yml.tmpl             # Alert rules
│
├── grafana/                        # Grafana
│   ├── datasources.yml.tmpl        # Data sources
│   └── dashboards/                 # Pre-built dashboards
│       ├── api.json.tmpl
│       ├── nodejs.json.tmpl
│       └── python.json.tmpl
│
├── logging/                        # Logging
│   ├── pino.config.ts.tmpl         # Node.js logger
│   ├── loguru.config.py.tmpl       # Python logger
│   ├── loki.yml.tmpl               # Loki config
│   └── promtail.yml.tmpl           # Log collector
│
├── jaeger/                         # Jaeger (tracing)
│   └── jaeger.yml.tmpl
│
├── tempo/                          # Tempo (tracing alternative)
│   └── tempo.yml.tmpl
│
├── sentry/                         # Error tracking
│   ├── sentry.client.ts.tmpl       # Browser
│   ├── sentry.server.ts.tmpl       # Node.js server
│   ├── sentry.edge.ts.tmpl         # Edge runtime
│   └── sentry.py.tmpl              # Python
│
├── slo/                            # SLO monitoring
│   ├── slo-definitions.yaml.tmpl
│   └── error-budget.yaml.tmpl
│
├── k8s/                            # Kubernetes
│   ├── servicemonitor.yaml.tmpl
│   └── podmonitor.yaml.tmpl
│
└── docker-compose.observability.yml.tmpl  # Full stack
```

---

## Quick Start

### 1. Using Docker Compose

```bash
# Copy and customize the template
cp observability/docker-compose.observability.yml.tmpl docker-compose.observability.yml

# Replace template variables
sed -i 's/{{PROJECT_NAME}}/my-project/g' docker-compose.observability.yml

# Start the observability stack
docker compose -f docker-compose.observability.yml up -d

# Access dashboards
# Grafana: http://localhost:3000 (admin/admin)
# Prometheus: http://localhost:9090
# Jaeger: http://localhost:16686
```

### 2. Using in Application

#### Node.js / TypeScript

```typescript
// Copy templates
cp observability/otel/tracing.ts.tmpl src/instrumentation.ts
cp observability/logging/pino.config.ts.tmpl src/logger.ts

// Install dependencies
npm install @opentelemetry/sdk-node pino pino-http

// Import at application entry
import './instrumentation';
import { logger } from './logger';
```

#### Python / FastAPI

```python
# Copy templates
cp observability/otel/tracing.py.tmpl src/tracing.py
cp observability/logging/loguru.config.py.tmpl src/logging_config.py

# Install dependencies
pip install opentelemetry-sdk opentelemetry-instrumentation-fastapi loguru

# Import in main.py
from .tracing import instrument_app
from .logging_config import logger

instrument_app(app)
```

---

## Template Variables

All templates use these variables:

| Variable | Description | Example |
|----------|-------------|---------|
| `{{PROJECT_NAME}}` | Project name | `my-awesome-app` |
| `{{AUTHOR}}` | Project author | `Your Name` |
| `{{ENVIRONMENT}}` | Deployment env | `production` |
| `{{CLUSTER_NAME}}` | K8s cluster | `prod-cluster` |
| `{{PROJECT_DOMAIN}}` | Domain | `example.com` |
| `{{NAMESPACE}}` | K8s namespace | `default` |
| `{{API_METRICS_PORT}}` | API metrics port | `8080` |
| `{{WEB_METRICS_PORT}}` | Web metrics port | `3000` |
| `{{DB_NAME}}` | Database name | `myapp` |
| `{{DB_USER}}` | Database user | `postgres` |
| `{{DB_PASSWORD}}` | Database password | `secret` |

---

## Components

### OpenTelemetry Collector

Central hub for telemetry data:
- Receives traces, metrics, logs
- Processes and exports to backends
- Protocol translation (OTLP, Jaeger, Zipkin)

**Endpoints:**
- OTLP gRPC: `4317`
- OTLP HTTP: `4318`
- Prometheus: `8889`

### Prometheus

Metrics storage and querying:
- Scrapes metrics from services
- Evaluates alert rules
- Powers Grafana dashboards

**Access:** http://localhost:9090

### Grafana

Unified observability platform:
- Pre-built dashboards (API, Node.js, Python)
- Connects to Prometheus, Loki, Jaeger
- Alert visualization

**Access:** http://localhost:3000

### Loki + Promtail

Log aggregation:
- Loki: Log storage (like Prometheus for logs)
- Promtail: Log collector (tails logs, ships to Loki)

**Features:**
- Label-based indexing
- LogQL query language
- Integration with Grafana

### Jaeger / Tempo

Distributed tracing:
- **Jaeger**: Feature-rich, battle-tested
- **Tempo**: Lightweight, native Grafana integration

**Choose based on:**
- Jaeger: More features, mature ecosystem
- Tempo: Simpler, better Grafana integration

### Sentry

Error tracking and performance monitoring:
- Client (browser), Server (Node.js), Edge, Python
- Session replay
- Performance profiling
- Release tracking

**Required:** Set `SENTRY_DSN` environment variable

---

## SLO Monitoring

### Defined SLOs

1. **API Availability**: 99.9% uptime (30-day window)
2. **API Latency**: 95% requests < 500ms
3. **Error Rate**: < 1% of requests fail
4. **DB Performance**: 95% queries < 100ms
5. **Job Success**: 99.5% background jobs succeed
6. **Cache Hit Rate**: > 90%

### Error Budget Alerts

- **Fast Burn**: 5% budget in 1 hour → Critical
- **Slow Burn**: 5% budget in 6 hours → Warning
- **Exhausted**: Budget depleted → Critical

---

## Grafana Dashboards

### API Dashboard
- Request rate by endpoint
- Error rate by status code
- Latency percentiles (P50, P95, P99)
- Active requests
- Slowest endpoints

### Node.js Dashboard
- Event loop lag
- Memory usage (heap, external)
- Garbage collection
- Active handles/requests
- CPU usage

### Python Dashboard
- Request rate by endpoint
- Response time P95
- Memory (RSS, Virtual)
- GC collections
- Database connections
- Exception rate

---

## Kubernetes Integration

### ServiceMonitor

Monitors Kubernetes services exposing `/metrics`:

```yaml
kubectl apply -f k8s/servicemonitor.yaml
```

### PodMonitor

Monitors individual pods:

```yaml
kubectl apply -f k8s/podmonitor.yaml
```

**Requirements:**
- Prometheus Operator installed
- Pods labeled correctly

---

## Environment Variables

### Required

```bash
# Sentry
SENTRY_DSN=https://xxx@sentry.io/xxx
NEXT_PUBLIC_SENTRY_DSN=https://xxx@sentry.io/xxx

# OpenTelemetry
OTEL_EXPORTER_OTLP_ENDPOINT=http://otel-collector:4318

# Application
ENVIRONMENT=production
APP_VERSION=1.0.0
```

### Optional

```bash
# Grafana
GRAFANA_USER=admin
GRAFANA_PASSWORD=secure-password

# Prometheus
PROMETHEUS_RETENTION=30d

# Log Level
LOG_LEVEL=info
OTEL_LOG_LEVEL=info

# Sentry Sampling
SENTRY_TRACES_SAMPLE_RATE=0.1
SENTRY_PROFILES_SAMPLE_RATE=0.1
```

---

## Best Practices

### 1. Sampling

- **Development**: 100% sampling
- **Production**: 10% traces, 1% profiles
- **High traffic**: Adaptive sampling

### 2. Cardinality

- Limit label values
- Avoid user IDs in metrics
- Use aggregation

### 3. Retention

- Metrics: 30 days (Prometheus)
- Logs: 30 days (Loki)
- Traces: 7 days (Jaeger/Tempo)

### 4. Alerting

- Alert on symptoms, not causes
- Use error budgets
- Reduce noise

### 5. Security

- Never log secrets
- Redact sensitive fields
- Use HTTPS in production

---

## Troubleshooting

### Metrics Not Showing

```bash
# Check Prometheus targets
curl http://localhost:9090/api/v1/targets

# Check if app exposes metrics
curl http://localhost:8080/metrics

# Verify network connectivity
docker compose logs prometheus
```

### Traces Not Appearing

```bash
# Check OTLP endpoint
curl http://localhost:4318/v1/traces

# Verify instrumentation
# Node.js: Check console for "OpenTelemetry initialized"
# Python: Check logs for trace IDs
```

### Logs Not in Loki

```bash
# Check Promtail
docker compose logs promtail

# Verify Loki
curl http://localhost:3100/ready

# Check labels
curl -G -s "http://localhost:3100/loki/api/v1/labels"
```

---

## Production Considerations

### Scalability

- Use remote storage (Thanos, Cortex)
- Distributed tracing backend
- Log aggregation at scale

### High Availability

- Run multiple Prometheus instances
- Load balance OpenTelemetry Collectors
- Replicated storage

### Cost Optimization

- Adjust retention periods
- Use sampling strategically
- Archive cold data

---

## Resources

### Documentation

- [OpenTelemetry](https://opentelemetry.io/docs/)
- [Prometheus](https://prometheus.io/docs/)
- [Grafana](https://grafana.com/docs/)
- [Loki](https://grafana.com/docs/loki/latest/)
- [Jaeger](https://www.jaegertracing.io/docs/)
- [Sentry](https://docs.sentry.io/)

### Tools

- [Grafana Dashboards](https://grafana.com/grafana/dashboards/)
- [Prometheus Exporters](https://prometheus.io/docs/instrumenting/exporters/)
- [OpenTelemetry Registry](https://opentelemetry.io/registry/)

---

## Support

For issues or questions:
1. Check the troubleshooting section
2. Review component documentation
3. Open an issue in the project repository

---

**Last Updated:** 2026-01-19
**Version:** 1.0
