# gratheon/grafana
A Grafana image with a few plugins pre-installed

## Usage in local development
Make sure to run `../postgres` first.
Then spin up containers:
```bash
just start
```

`grafana` database is created by `postgres/init-databases.sql` on first initialization.

## Bundled Prometheus
`docker-compose.dev.yml` now includes a `prometheus` service and Grafana datasource provisioning.

- Prometheus UI: `http://localhost:9090`
- Grafana UI: `http://localhost:9000`
- Provisioned datasource: `Prometheus` (default)

### Current scrape targets
- `prometheus:9090`
- `user-cycle:4000/metrics`

### Notes
- Prometheus is pull-based. Services expose `/metrics`, and Prometheus scrapes on an interval (currently `15s`).
- This is the standard setup for in-house infra metrics and works well for your single-node deployment.
