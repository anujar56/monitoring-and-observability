1. Create a service

```
cat /etc/systemd/system/custom.sh <<EOF
[Unit]
Description=Custom Bash Exporter

[Service]
ExecStart=/usr/local/bin/custom.sh
Restart=always

[Install]
WantedBy=multi-user.target
EOF
```

```
systemctl daemon-reload
systemctl enable custom
systemctl start custom
```

2. Edit the prometheus.yml file to scrape at port `9100`

```
global:
  scrape_interval: 10s

scrape_configs:
  - job_name: 'prometheus_metrics'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:9090']
  - job_name: 'custom_exporter_metrics'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:9100']

```
3. Restart prometheus

```
systemctl restart prometheus.service
```

---

## Notes
- `nc` commands act a simple http server which expose the metrics to '9100'
- `nc` doesn't check the path hence when prometheus ask for `/metrics` it gives irrelevant of the requested path.
- Script first collects the metrics and waits for connection at `9100` (listening `-l`)
- If you're scraping from Prometheus or using curl, they expect a valid HTTP response.

```
  HTTP/1.1 200 OK
  Content-Type: text/plain
  cpu_load 0.34
```
