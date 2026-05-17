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
      pratikshinde55/ps-devops:ps-observe-grafana-v1
