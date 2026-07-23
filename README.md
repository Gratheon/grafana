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

## Production deprecation observation

Grafana and the related Influx route return HTTP `410 Gone` during the
30-day deprecation observation period started at `2026-07-23 20:05:49 UTC`.
Do not delete the deployment or data before `2026-08-22 20:05:49 UTC` and a
separate approval.

Install `config/nginx.conf` as the active Nginx include and copy
`config/nginx-legacy-deprecation.logrotate` to
`/etc/logrotate.d/nginx-legacy-deprecation`. Run `nginx -t` before reloading
Nginx. The shared Let's Encrypt certificate uses the Nginx authenticator, so
`return 410` must remain inside `location /` to allow temporary HTTP-01
challenge locations.
