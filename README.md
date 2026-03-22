# gratheon/grafana
A Grafana image with a few plugins pre-installed

## Usage in local development
Make sure to run `../postgres` first.
Then spin up containers:
```bash
just start
```

`grafana` database is created by `postgres/init-databases.sql` on first initialization.

## Bundled Prometheus + Loki
`docker-compose.dev.yml` includes:
- `prometheus` for metrics
- `loki` for logs
- `promtail` to ship Docker container logs into Loki

- Prometheus UI: `http://localhost:9090`
- Loki API: `http://localhost:3100`
- Grafana UI: `http://localhost:9000`
- Provisioned datasources: `Prometheus` (default), `Loki`

### Current scrape targets
- `prometheus:9090`
- `user-cycle:4000/metrics`

### Notes
- Prometheus is pull-based. Services expose `/metrics`, and Prometheus scrapes on an interval (currently `15s`).
- Loki stores logs and Promtail labels them with Docker metadata (including Compose `service`).
- Service dashboards include a `Log Lines / sec by Service` panel using Loki to break down logs by service label.
- This setup expects Docker container log files to be available at `/var/lib/docker/containers` for Promtail.
