# Grafana dashboards

Dashboards for monitoring this Backstage instance. Import them in Grafana
(Dashboards → New → Import) or provision them via a dashboard sidecar.

| File | Description |
|------|-------------|
| `backstage.json` | Backend OpenTelemetry metrics: catalog, scaffolder, scheduler, HTTP, MCP actions, Node runtime, pod resources |

## Requirements

The backend must run with OpenTelemetry metrics exported to Prometheus
(see [Setup OpenTelemetry](https://backstage.io/docs/tutorials/setup-opentelemetry)).
Queries match metric names with or without a unit suffix, so both the
in-process Prometheus exporter and an OTel Collector pipeline work.

For every panel to populate:

- **Histogram buckets** – the OTel default buckets are sized for milliseconds,
  but Backstage records durations in seconds. Add a View with second-scale
  boundaries (e.g. `[0.01, 0.05, 0.1, 0.25, 0.5, 1, 2.5, 5, 10, 30, 60, 120, 300, 600]`)
  for `*.duration`, `catalog.processing.queue.delay` and
  `catalog.stitching.queue.delay`, otherwise percentiles are meaningless.
- **HTTP panels** – set `OTEL_SEMCONV_STABILITY_OPT_IN=http` so
  `http.server.request.duration` with `http.route` is emitted.
- **Runtime panels** – enable `@opentelemetry/instrumentation-runtime-node`
  (included in `auto-instrumentations-node`).
- **Pod panels** – use cAdvisor metrics (`container_*`), which kube-prometheus
  scrapes by default.

Variables `namespace`, `job` and `pod` come from the scrape target labels,
so scrape the backend with a ServiceMonitor or PodMonitor.
