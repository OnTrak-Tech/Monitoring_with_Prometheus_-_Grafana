## Setup Instructions

This project can be implemented on any Linux-based Server or local machine. However, this instruction assumes that the server is an AWS EC2 instance running on ubuntu 24.04

### Prerequisite

i) AWS EC2 instance running on ubuntu with git and docker installed.
ii) Basic knowledge of docker, linux cli, prometheus, and grafana

### Sever Setup (Containerized Prometheus, Grafana, and Node Exporter)

- ssh into your EC2 instance
- run ```git clone https://github.com/jsabrokwah/grafana-prometheus.git```
- run ```sudo docker compose up -d```
- Verify that your containers are running by ```sudo docker compose ps``` You will see a results similar to this:

```bash
NAME            IMAGE                        COMMAND                  SERVICE         CREATED       STATUS       PORTS
grafana         grafana/grafana-oss:latest   "/run.sh"                grafana         2 hours ago   Up 2 hours   0.0.0.0:3000->3000/tcp, [::]:3000->3000/tcp
node-exporter   prom/node-exporter:latest    "/bin/node_exporter …"   node-exporter   2 hours ago   Up 2 hours   0.0.0.0:9100->9100/tcp, [::]:9100->9100/tcp
prometheus      prom/prometheus:latest       "/bin/prometheus --c…"   prometheus      2 hours ago   Up 2 hours   0.0.0.0:9090->9090/tcp, [::]:9090->9090/tcp
```

- Open the Prometheus Client in your browser ```http://your-instance-ip:9090/targets``` It should look similar to this image ![Prometheus Target Page](./Screenshots/Prometheus%20Target%20Page.png)

- Open the Grafana Dashboard in your browser ```http://your-instance-ip:3000``` It should look similar to this image ![Grafana Dashboard](./Screenshots/Grafana%20Login%20Page.png)

  - Enter ```admin``` as both the username and password.
  - Update your password when prompted.

### Building the Dashboards

[Follow this blog](https://betterstack.com/community/guides/monitoring/visualize-prometheus-metrics-grafana/#configuring-the-prometheus-data-source) to for a step-by-step guide on building the dashboard.

