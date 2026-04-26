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

