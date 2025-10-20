# Chainlink Monitoring & Alerting with Prometheus and Grafana
This guide outlines how to monitor and alert on key health indicators of a Chainlink node using Prometheus and Grafana.

# What This Dashboard Tracks
The Grafana dashboard is designed to monitor the following critical metrics:
- ETH Balance: Alerts when the Chainlink node's Ether balance falls below a defined threshold.
- Errored Job Runs: Tracks failed job executions.
- Node UI Port Availability: Detects if the Chainlink node's web interface becomes unresponsive.
- Full Node RPC Health: Monitors HTTP and WebSocket endpoints for responsiveness.
- Docker Container Logs: Observes container output for anomalies.
- New Heads Rate: Alerts if new heads per minute drop below 1 for over 5 minutes — indicating the node isn't receiving updates from Ethereum.
- Head Tracker Queue: Should remain at 0; alerts if it averages above 1 for more than 5 minutes.
- Callback Execution Time: Should stay below 1 second (ideally 100–300ms). Higher averages may indicate RPC or database bottlenecks.
- Heads Dropped: Should be 0. Higher values suggest the node is falling behind in processing new heads.

# Prerequisites
You’ll need:
- Prometheus (default port: 9090)
- Grafana (default port: 3000)
- A running Chainlink node exposing metrics on port 6688

# Prometheus Configuration
Edit your prometheus.yml file (typically located at /etc/prometheus/prometheus.yml) to include your Chainlink node:
```
scrape_configs:
  - job_name: 'Chainlink_Node_1'
    scrape_interval: 5s
    scrape_timeout: 5s
    static_configs:
      - targets: ['localhost:6688']
    authorization:
      credentials: 'mysecuretokenforPrometheus'  # Must match the AuthToken in secrets.toml
```
Note: The AuthToken is required to access the Chainlink metrics endpoint and must match the value in your node’s secrets.toml.

# Grafana Setup
- Log in to Grafana (http://localhost:3000) with default credentials admin/admin
- Change your password
- Add a new data source:
- Type: Prometheus
- URL: http://localhost:9090
- Click Save & Test

# Security Tips
- Restrict access to Prometheus (9090) and Grafana (3000) using firewalls or by binding them to 127.0.0.1
- Do the same for your Chainlink node’s metrics port (6688)
- Consider using reverse proxies or VPNs for secure remote access




