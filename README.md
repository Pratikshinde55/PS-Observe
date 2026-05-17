# PS-Observe
Production-grade monitoring and observability stack using Prometheus, Grafana, cAdvisor, and Node Exporter with container-level metrics visualization.



# Custom Branding Enterprise Grafana Dockerfile:
Overview

This Dockerfile creates a fully customized and production-ready Grafana image with:
- Custom PS-Observe branding
- Custom SVG logo replacement
- Enterprise Grafana setup
- Non-root container execution
- Optimized multi-stage build
- Production-grade container structure


For Dockerfile refer abhove **"Dockerfile-Custom_Grafana"** In this file use change need to change .svg logo file that you want and HTML Title:

For **Enterprise Grade** with Access HTTPS with domain then need pass env during run time: 

GF_SERVER_SERVE_FROM_SUB_PATH=true & GF_SERVER_ROOT_URL=%(protocol)s://%(domain)s/grafana/    Those are only needed when Grafana runs behind:
 - Nginx reverse proxy
 - ALB path routing
 - subpath like /grafana

Command:

      docker run -d \
      --name ps-observe-grafana \
      -p 30001:3000 \
      -e GF_SERVER_ROOT_URL=%(protocol)s://%(domain)s/grafana/ \
      -e GF_SERVER_SERVE_FROM_SUB_PATH=true \
      -v grafana-data:/var/lib/grafana \
      pratikshinde55/ps-devops:ps-observe-grafana-v1

Normal without domain:

   docker run -d --name ps-observe-grafana -p 3000:3000 -v grafana-data:/var/lib/grafana pratikshinde55/ps-devops:ps-observe-grafana-v1 --restart unless-stopped


Grafana Login Page:
<img width="1916" height="856" alt="image" src="https://github.com/user-attachments/assets/61b3709e-94a5-4c42-ac7e-3d943c5bf58e" />

Grafana Home Page:
<img width="1912" height="863" alt="image" src="https://github.com/user-attachments/assets/495bc28d-c212-400b-878f-1996dbb53433" />


# Custom secure Prometheus:
Custom production-style Prometheus image built for Docker-based monitoring environments.

This image is designed for:

 - Infrastructure monitoring
 - Container monitoring
 - Enterprise-style observability setup
 - Docker monitoring stack
 - Persistent TSDB metrics storage

Dockerfile has been attched in this project named as: Dockerfile-custom_prometheus

Prometheus Directory Structure:

/etc/prometheus

Used for:
- Prometheus binary
- Prometheus configuration
- monitoring runtime files

--

TSDB storage path:  /prometheus

Used for:
- metrics storage
- WAL
- TSDB chunks
- retention data

This follows official Prometheus container conventions.

Prometheus Configuration File: /etc/prometheus/prometheus.yml

This file contains:
- scrape jobs
- monitoring targets
- labels
- scrape intervals


Healthcheck:  

     HEALTHCHECK --interval=30s --timeout=10s --start-period=30s --retries=5 \
     CMD curl -f http://localhost:9090/-/healthy || exit 1


Docker healthcheck validates:
- Prometheus API health
- container readiness
- monitoring availability

Container becomes:
- healthy
- unhealthy

based on Prometheus runtime state.


TSDB Retention

Prometheus retention is configured dynamically during runtime.

Example: --storage.tsdb.retention.time=30d

This means:
- retain metrics for 30 days
- automatically remove older metrics


Persistent Storage:

Docker volume used: -v prometheus-data:/prometheus

This ensures:
- metrics survive container restart
- TSDB persists after container recreation
- long-term monitoring retention works correctly


Prometheus Runtime Command: 

     docker run -d \
     --name ps-observe-prometheus \
     --restart unless-stopped \
     -p 9090:9090 \
     -v prometheus-data:/prometheus \
     pratikshinde55/ps-devops:ps-observe-prometheus-v1 \
     --storage.tsdb.path=/prometheus \
     --storage.tsdb.retention.time=30d
