Very basic lab on Prometheus + Grafana

# Setup

```
$ docker compose up
```

# Endpoints

- Prometheus:
    - Dashboard:
        http://localhost:9090
    - Node exporter (agent):
        http://localhost:9100/metrics

- Grafana:
    http://localhost:3000


# Grafana

## Reset admin password

```
$ docker exec grafana grafana cli admin reset-admin-password admin
```

Now credentials are admin/admin

## Add a dashboard

- Home > Connections > Data sources > Add data source
    - Prometheus
    - Prometheus server URL: http://prometheus:9090
    - HTTP method: GET


### Display downstream bandwidth

- Home > Dashboards > New > Add visualization
    - Metric: node_network_receive_bytes_total
    - Run queries
        all network interfaces are shown
    - Label filters:
        Select label: device
        Select value: eth0
    - Run queries
        this metric is a "counter", a "rate" may be more useful
    - Operations
        rate

### Add upstream

- Add query
- Metric: node_network_transmit_bytes_total
- Operation: rate
- Apply/Save


### Import a prebuild dashboard

- Visit https://grafana.com/grafana/dashboards/1860-node-exporter-full/
- Click: Copy ID to clipboard
- In Grafana
    - Home > Dashboards > New > Import
    - Paste ID
    - Click: Load
    - Select a Prometheus data source: prometheus


# Prometheus

## Add a panel

- Find "node_network_receive_bytes_total"
- Click "Graph"

with rate: `irate(node_network_receive_bytes_total{device="eth0"}[5m])`
