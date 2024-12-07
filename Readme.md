# Ansible Roles for Service Deployment with Traefik

This repository contains two Ansible roles for deploying services with Traefik as a reverse proxy:

- `gfreezy.traefik`: Deploys and configures Traefik as a reverse proxy
- `gfreezy.service`: Deploys services using blue/green deployment strategy

## Requirements

- Ansible 2.1 or higher
- Docker installed on target hosts
- Cloudflare DNS API token (for Let's Encrypt DNS challenge)

## Role: gfreezy.traefik

This role sets up Traefik as a reverse proxy with the following features:

- Automatic HTTPS with Let's Encrypt
- Cloudflare DNS challenge for certificate issuance
- Docker provider configuration
- Prometheus metrics endpoint
- Web dashboard (on port 8080)

### Variables

The following variables are available for the `gfreezy.traefik` role:

- `traefik_cloudflare_email`: Cloudflare account email (required)
- `traefik_cloudflare_api_token`: Cloudflare API token (required)
- `traefik_domain`: Domain name for Traefik dashboard (required)
- `traefik_network`: Docker network name for Traefik (default: traefik)
- `traefik_dashboard_port`: Port for Traefik dashboard (default: 8080)
- `traefik_metrics_port`: Port for Prometheus metrics (default: 8082)
- `traefik_services_conf_dir`: Directory for service configurations (default: /etc/traefik/services)
- `traefik_log_level`: Logging level (default: ERROR)
- `traefik_acme_email`: Email for Let's Encrypt notifications (required)

## Role: gfreezy.service

This role handles service deployment using a blue/green deployment strategy with Traefik integration. It provides:

- Zero-downtime deployments using blue/green pattern
- Automatic service registration with Traefik
- Health check monitoring during deployment
- Automatic cleanup of old service instances
- Integration with Docker containers

### Variables

- `service_name`: Name of the service to deploy (required)
- `service_image`: Docker image to deploy (required)
- `service_port`: Port the service listens on (required)
- `service_env`: Dictionary of environment variables for the service
- `service_labels`: Dictionary of Docker labels to apply
- `service_volumes`: List of volumes to mount
- `service_networks`: List of Docker networks to connect to

## Example Usage

Here's a complete example showing how to use both roles together to deploy a service with Traefik:

```yaml
---
- name: "Deploy {{ service_name }} service"
  hosts: all
  become: yes # Use sudo
  user: ubuntu # Use ubuntu user
  vars:
    service_name: rizz
    image: "162953534862.dkr.ecr.us-west-2.amazonaws.com/rizz"
    traefik_conf_dir: /opt/service/traefik
    docker_network_name: traefik
    traefik_services_conf_dir: "{{ traefik_conf_dir }}/services_conf"
    hostname: api.hostname.com

  tasks:
    - include_role:
        name: gfreezy.traefik
      vars:
        conf_dir: "{{ traefik_conf_dir }}"
        services_conf_dir: "{{ traefik_services_conf_dir }}"
        custom_certs_dir: "{{ traefik_conf_dir }}/certs"
        network: "{{ docker_network_name }}"

    - include_role:
        name: gfreezy.service
      vars:
        service_port: 8080
        network: "{{ docker_network_name }}"
        docker_volumes:
          - "{{rizz_dir}}:/App/db"

    - name: Prune everything
      community.docker.docker_prune:
        images: true

```

