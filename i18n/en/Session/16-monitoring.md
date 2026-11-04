# 16. Monitoring

[한국어](../../../Session/16-monitoring.md) | **English** | [日本語](../../ja/Session/16-monitoring.md) | [简体中文](../../zh-CN/Session/16-monitoring.md) | [Español](../../es/Session/16-monitoring.md) | [Français](../../fr/Session/16-monitoring.md)

### Node Exporter - [ FW, DNS, WEB1, WEB2, DB, MGMT ]

```bash
apt install -y prometheus-node-exporter
systemctl enable --now prometheus-node-exporter
```

### Blackbox Exporter - [ MGMT ]

```bash
apt install -y prometheus-blackbox-exporter
vim /etc/prometheus/blackbox.yml
```

```
# =====================================================================
#  /etc/prometheus/blackbox.yml  (MGMT)
#  http_2xx    : success if the URL returns a 2xx response (HTTPS certificate is checked too)
#  tcp_connect : success if the port accepts a connection
# =====================================================================
modules:
  http_2xx:
    prober: http
    timeout: 5s
    http:
      preferred_ip_protocol: ip4
  tcp_connect:
    prober: tcp
    timeout: 5s
    tcp:
      preferred_ip_protocol: ip4
```

```bash
systemctl restart prometheus-blackbox-exporter
```

### Prometheus - [ MGMT ]

```bash
apt install -y prometheus
vim /etc/prometheus/prometheus.yml
```

```
# =====================================================================
#  /etc/prometheus/prometheus.yml  (MGMT)
#  node      : CPU, memory, disk of the 6 servers (node-exporter 9100)
#  shop/live : whether the shop and JJong LIVE open at the customer address (blackbox 9115)
#  mariadb   : whether DB 3306 responds (blackbox 9115)
# =====================================================================
global:
  scrape_interval: 15s
  evaluation_interval: 15s

rule_files:
  - /etc/prometheus/rules-jjong.yml

scrape_configs:
  - job_name: prometheus
    static_configs:
      - targets: ['localhost:9090']

  - job_name: node
    static_configs:
      - targets: ['10.10.30.254:9100']
        labels: { host: FW }
      - targets: ['10.10.10.10:9100']
        labels: { host: DNS }
      - targets: ['10.10.10.21:9100']
        labels: { host: WEB1 }
      - targets: ['10.10.10.22:9100']
        labels: { host: WEB2 }
      - targets: ['10.10.20.10:9100']
        labels: { host: DB }
      - targets: ['localhost:9100']
        labels: { host: MGMT }

  - job_name: shop
    metrics_path: /probe
    params: { module: [http_2xx] }
    static_configs:
      - targets: ['https://www.jjongmarket.kr/']
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: 127.0.0.1:9115

  - job_name: live
    metrics_path: /probe
    params: { module: [http_2xx] }
    static_configs:
      - targets: ['https://www.jjongmarket.kr/hls/stream.m3u8']
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: 127.0.0.1:9115

  - job_name: mariadb
    metrics_path: /probe
    params: { module: [tcp_connect] }
    static_configs:
      - targets: ['10.10.20.10:3306']
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: 127.0.0.1:9115
```

```bash
vim /etc/prometheus/rules-jjong.yml
```

```
# =====================================================================
#  /etc/prometheus/rules-jjong.yml  (MGMT) - alert rules
#  check: http://10.10.30.10:9090/alerts
# =====================================================================
groups:
  - name: jjong
    rules:
      - alert: HostDown
        expr: up{job="node"} == 0
        for: 1m
        labels: { severity: critical }
        annotations: { summary: "{{ $labels.host }} is down" }

      - alert: ShopDown
        expr: probe_success{job="shop"} == 0
        for: 30s
        labels: { severity: critical }
        annotations: { summary: "Shop (www.jjongmarket.kr) is not responding" }

      - alert: LiveDown
        expr: probe_success{job="live"} == 0
        for: 30s
        labels: { severity: warning }
        annotations: { summary: "JJONG LIVE stream stopped" }

      - alert: MariaDBDown
        expr: probe_success{job="mariadb"} == 0
        for: 30s
        labels: { severity: critical }
        annotations: { summary: "MariaDB 10.10.20.10:3306 is not responding" }

      - alert: HighCPU
        expr: 100 - avg by (host) (rate(node_cpu_seconds_total{mode="idle"}[2m])) * 100 > 80
        for: 2m
        labels: { severity: warning }
        annotations: { summary: "{{ $labels.host }} CPU usage is high" }

      - alert: DiskAlmostFull
        expr: (1 - node_filesystem_avail_bytes{mountpoint="/"} / node_filesystem_size_bytes{mountpoint="/"}) * 100 > 85
        for: 1m
        labels: { severity: warning }
        annotations: { summary: "{{ $labels.host }} disk usage over 85%" }
```

```bash
promtool check config /etc/prometheus/prometheus.yml
systemctl restart prometheus
```

### Grafana - [ MGMT ]

Grafana is not in the Debian repository, so download `grafana-enterprise_8.3.4_amd64.deb` in advance. Attach a NAT adapter to MGMT and send it from the host PC.

```powershell
scp .\grafana-enterprise_8.3.4_amd64.deb root@<MGMT_NAT_IP>:/root/
scp .\configs\monitoring\grafana\jjong-infra.json root@<MGMT_NAT_IP>:/root/
```

```bash
apt install -y /root/grafana-enterprise_8.3.4_amd64.deb
vim /etc/grafana/provisioning/datasources/prometheus.yaml
```

```
# /etc/grafana/provisioning/datasources/prometheus.yaml  (MGMT)
# Registers Prometheus as a data source automatically when Grafana starts
apiVersion: 1
datasources:
  - name: Prometheus
    uid: prometheus
    type: prometheus
    access: proxy
    url: http://localhost:9090
    isDefault: true
    editable: false
```

```bash
vim /etc/grafana/provisioning/dashboards/jjong.yaml
```

```
# /etc/grafana/provisioning/dashboards/jjong.yaml  (MGMT)
# Grafana automatically loads the JSON dashboards in this folder at startup.
apiVersion: 1
providers:
  - name: jjong
    folder: JJONG
    type: file
    disableDeletion: false
    options:
      path: /var/lib/grafana/dashboards
```

```bash
mkdir -p /var/lib/grafana/dashboards
cp /root/jjong-infra.json /var/lib/grafana/dashboards/
chown -R grafana:grafana /var/lib/grafana/dashboards
systemctl daemon-reload
systemctl enable --now grafana-server
```

CLIENT Firefox → `http://10.10.30.10:3000` → Dashboards → JJONG → **쫑마켓 인프라 현황** (JJongMarket Infrastructure Status)

> The dashboard JSON has Korean titles and breaks if pasted into the console. Always transfer it as a file with scp.
