# Prometheus & Grafana Monitoring Stack Documentation

## Overview

This project implements a comprehensive monitoring solution using Prometheus for metrics collection and Grafana for visualization. The stack includes Node Exporter for system metrics collection and is orchestrated using Docker Compose.

## Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Node Exporter │    │   Prometheus    │    │    Grafana      │
│   (Port: 9100)  │───▶│   (Port: 9090)  │───▶│   (Port: 3000)  │
│                 │    │                 │    │                 │
│ System Metrics  │    │ Metrics Storage │    │  Visualization  │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

## Components

### 1. Prometheus
- **Image**: `prom/prometheus:latest`
- **Port**: 9090
- **Purpose**: Time-series database for metrics collection and storage
- **Configuration**: `/etc/prometheus/prometheus.yml`
- **Data Persistence**: `prometheus_data` volume

### 2. Node Exporter
- **Image**: `prom/node-exporter:latest`
- **Port**: 9100
- **Purpose**: Collects hardware and OS metrics from the host system
- **Host Mounts**: `/proc`, `/sys`, `/` (read-only)

### 3. Grafana
- **Image**: `grafana/grafana-oss:latest`
- **Port**: 3000
- **Purpose**: Metrics visualization and alerting
- **Data Persistence**: `grafana_data` volume
- **Email Notifications**: Configured with SMTP

## Configuration Files

### Docker Compose (`Configurations/compose.yaml`)

The main orchestration file defining all services, networks, and volumes.

**Key Features:**
- Custom network: `prometheus-grafana`
- Persistent volumes for data retention
- Service dependencies
- Environment variables for Grafana SMTP

### Prometheus Configuration (`Configurations/prometheus.yml`)

```yaml
global:
  scrape_interval: 10s

scrape_configs:
  - job_name: node-exporter
    static_configs:
      - targets:
          - 'node-exporter:9100'
```

**Configuration Details:**
- **Scrape Interval**: 10 seconds (high frequency for detailed monitoring)
- **Target**: Node Exporter service on port 9100
- **Service Discovery**: Static configuration using Docker service names

## Network Architecture

### Custom Network: `prometheus-grafana`
- **Driver**: Bridge
- **Purpose**: Isolates monitoring stack from other containers
- **Benefits**: 
  - Service discovery by name
  - Network segmentation
  - Controlled communication

### Port Mappings
- **Prometheus**: `9090:9090` - Web UI and API access
- **Node Exporter**: `9100:9100` - Metrics endpoint
- **Grafana**: `3000:3000` - Dashboard interface

## Data Persistence

### Volumes
1. **prometheus_data**: Stores time-series metrics data
2. **grafana_data**: Stores dashboards, users, and configuration

### Host Mounts (Node Exporter)
- `/proc:/host/proc:ro` - Process information
- `/sys:/host/sys:ro` - System information
- `/:/rootfs:ro` - Root filesystem access

## Security Considerations

### Current Setup
- All services expose ports to host
- Read-only mounts for Node Exporter
- Custom network isolation

### Production Recommendations
1. **Reverse Proxy**: Use nginx/traefik for SSL termination
2. **Authentication**: Enable Grafana authentication
3. **Firewall**: Restrict port access
4. **Secrets Management**: Use Docker secrets for credentials
5. **Network Policies**: Implement additional network restrictions

## Email Notifications

Grafana is configured with SMTP for email alerts:

```yaml
environment:
  - GF_SMTP_ENABLED=true
  - GF_SMTP_HOST=smtp.gmail.com:587
  - GF_SMTP_USER=your_email@gmail.com
  - GF_SMTP_PASSWORD=your_app_password_or_email_password
  - GF_SMTP_FROM_ADDRESS=your_email@gmail.com
  - GF_SMTP_FROM_NAME=Grafana Alerts
  - GF_SMTP_SKIP_VERIFY=true
```

## Usage Instructions

### Starting the Stack
```bash
cd Configurations
docker-compose up -d
```

### Accessing Services
- **Prometheus**: http://localhost:9090
- **Grafana**: http://localhost:3000
- **Node Exporter**: http://localhost:9100

### Default Credentials
- **Grafana**: admin/admin (change on first login)

### Stopping the Stack
```bash
docker-compose down
```

### Viewing Logs
```bash
docker-compose logs -f [service_name]
```

## Monitoring Capabilities

### Available Metrics
- **System**: CPU, Memory, Disk, Network
- **Process**: Running processes and resource usage
- **Hardware**: Temperature, power consumption
- **Network**: Interface statistics, connections

### Dashboard Features
- Real-time system monitoring
- Historical data analysis
- Custom alerting rules
- Email/Slack notifications
- Multi-panel visualizations

## Troubleshooting

### Common Issues

1. **Port Conflicts**
   - Check if ports 3000, 9090, 9100 are available
   - Modify port mappings if needed

2. **Permission Issues**
   - Ensure Docker has access to host directories
   - Check volume mount permissions

3. **Network Connectivity**
   - Verify services can communicate within Docker network
   - Check firewall settings

4. **Data Persistence**
   - Ensure volumes are properly mounted
   - Check disk space availability

### Verification Commands
```bash
# Check service status
docker-compose ps

# View service logs
docker-compose logs prometheus
docker-compose logs grafana
docker-compose logs node-exporter

# Test connectivity
curl http://localhost:9090/api/v1/targets
curl http://localhost:9100/metrics
```

## Maintenance

### Backup Strategy
1. **Prometheus Data**: Backup `prometheus_data` volume
2. **Grafana Configuration**: Export dashboards and data sources
3. **Configuration Files**: Version control all YAML files

### Updates
```bash
# Pull latest images
docker-compose pull

# Restart with new images
docker-compose up -d
```

### Monitoring Health
- Check Prometheus targets status
- Verify Grafana data source connectivity
- Monitor container resource usage
- Review alert notifications

## Screenshots Reference

The `Screenshots/` directory contains visual documentation:
- Complete Dashboard.png
- Disk Usage Visualization.png
- Grafana Email Notification.png
- Grafana Homepage.png
- Grafana Slack Notification.png
- Imported Dashboard.png
- Memory Usage Dashboard.png
- Prometheus Homepage.png
- Prometheus&Grafana&Node_Running.png

## Best Practices

1. **Resource Limits**: Set memory and CPU limits for containers
2. **Health Checks**: Implement container health checks
3. **Logging**: Configure proper log rotation
4. **Monitoring**: Monitor the monitoring stack itself
5. **Documentation**: Keep configuration changes documented
6. **Testing**: Test backup and recovery procedures

## Future Enhancements

- Add more exporters (MySQL, Redis, etc.)
- Implement service discovery
- Add SSL/TLS encryption
- Integrate with external authentication
- Implement high availability setup
- Add custom metrics collection