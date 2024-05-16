# Monitor-Stack with Prometheus and Grafana running using Docker Compose

This Docker Compose configuration sets up a monitoring stack using Prometheus for metrics collection and Grafana for visualization. It also includes exporters for collecting metrics from hosts and Pi-hole.

## Services

### Prometheus

Prometheus is a monitoring and alerting toolkit designed for reliability and scalability. It collects metrics from configured targets and stores them for querying and visualization.

- **Image**: `docker.io/prom/prometheus:latest`
- **Configuration**: Mounts a custom Prometheus configuration file (`prometheus.yaml`) located in the `./config` directory.
- **Exposed Port**: `9090`
- **Volume**: Uses an external volume (`prometheus_data`) for persistent storage.

### Grafana

Grafana is a multi-platform open source analytics and interactive visualization web application. It provides features to create, explore, and share dashboards.

- **Image**: `docker.io/grafana/grafana-oss:10.4.2`
- **Exposed Port**: `3000`
- **Volume**: Uses an external volume (`grafana_data`) to persist Grafana data.

### Node Exporter

The Prometheus Node Exporter collects system metrics from hosts.

- **Image**: `quay.io/prometheus/node-exporter:v1.8.0`
- **Network Mode**: Uses host network for better access to host metrics.
- **Volume**: Mounts the root filesystem (`/`) of the host to collect metrics.

### Pi-hole Exporter

Collects metrics from a Pi-hole instance for monitoring its status.

- **Image**: `docker.io/ekofr/pihole-exporter:latest`
- **Environment Variables**:
    - `PIHOLE_HOSTNAME`: IP address or hostname of the Pi-hole instance.
    - `PIHOLE_PASSWORD`: Password to access Pi-hole (change this for security).
    - `PORT`: Exporter port for exposing metrics (default is `9617`).

## Setup Instructions

**1. Clone the Repository**:

`git clone https://github.com/True-Mousse/Monitor-Stack.git`

**2. Create Docker Network (`lab-net`)**:

`docker network create lab-net`

**3. Create Docker Volumes**:

`docker volume create prometheus_data grafana_data`

**4. Start the Services**:

`docker-compose up -d`

**5. Access Prometheus**: Open Prometheus in your browser at http://localhost:9090.

**6. Access Grafana**: Open Grafana in your browser at http://localhost:3000.

- Default credentials: `admin` / `admin` (change this for security).

**7. Configure Dashboards**:

- Add Prometheus as a data source in Grafana.
- Import pre-built dashboards or create custom dashboards to visualize metrics.

## Important Notes

- **Network**: This setup assumes usage of an external Docker network named `lab-net`. Ensure this network is correctly configured or adjust as needed.
- **Volumes**: External volumes (`prometheus_data` and `grafana_data`) are used for persistent data storage. Make sure these volumes exist or create them using Docker volume commands.
