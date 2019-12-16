Ceph Monitoring
===============

## 1. Enable Prometheus module
```bash
ceph mgr module enable prometheus
```

Verify:
```bash
$ ceph mgr services
$ curl -sSL http://ceph-host:9283/metrics
```

## 2. Install Prometheus `node-exporter`
```bash
apt install prometheus-node-exporter
```

## 3. Install & Config Prometheus server
### 3.1 Install Prometheus server
```bash
apt install prometheus
```

### 3.2 Prometheus scraping 
```yaml
# prometheus.yml
global:
  scrape_interval:     15s
  evaluation_interval: 15s

scrape_configs:
- job_name: 'node'
  file_sd_configs:
  - files:
    - node_targets.yml
- job_name: 'ceph'
  honor_labels: true
  file_sd_configs:
  - files:
    - ceph_targets.yml
```

```yaml
# ceph_targets.yml
- targets:
  - sds-1:9283
  - sds-2:9283
  - sds-3:9283
  labels: {}
```

```yaml
# node_targets.yml
- targets:
  - sds-1:9100
  - sds-2:9100
  - sds-3:9100
  labels: {}
```

Then, restart prometheus service:
```bash
systemctl restart prometheus
```

## 3. Install & Config Grafana
### 3.1 Install Grafana
```bash
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"

sudo apt-get update
sudo apt-get install grafana
```

Install Grafana plugins
```bash
sudo grafana-cli plugins install grafana-piechart-panel
sudo grafana-cli plugins install vonage-status-panel

sudo systemctl start grafana-server
```

Access to Grafana via `http://localhost:3000`, username/password: `admin/admin`

### 3.2 Add Prometheus DataSource
// TODO

### 3.3 Add Grafana Dashboards
Add Ceph dashboards to Grafana. [link](https://github.com/ceph/ceph/tree/master/monitoring/grafana)
