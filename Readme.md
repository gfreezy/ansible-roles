# Ansible Roles for Service Deployment with Traefik

This repository contains a Ansible role for deploying services with Traefik as a reverse proxy:

- `gfreezy.service`: Deploys services using blue/green deployment strategy

## Requirements

- Ansible 2.1 or higher
- Docker installed on target hosts
- Cloudflare DNS API token (for Let's Encrypt DNS challenge)

## Role: gfreezy.service

This role handles service deployment using a blue/green deployment strategy with Traefik integration. It provides:

- Zero-downtime deployments using blue/green pattern
- Automatic service registration with Traefik
- Health check monitoring during deployment
- Automatic cleanup of old service instances
- Integration with Docker containers

### Variables

| Variable | Default Value | Description |
|----------|--------------|-------------|
| `gfreezy_traefik_conf_dir` | `/opt/services/traefik` | Base configuration directory for Traefik |
| `gfreezy_traefik_services_conf_dir` | - | Directory for service-specific Traefik configurations |
| `gfreezy_traefik_docker_traefik_conf` | `/var/conf/traefik` | Directory for Docker-specific Traefik configurations |
| `gfreezy_traefik_network` | `traefik` | Name of the Docker network Traefik will use |
| `gfreezy_traefik_cloudflare_dns_api_token` | - | Cloudflare DNS API token for Let's Encrypt DNS challenge |
| `gfreezy_traefik_custom_certs_dir` | `{{gfreezy_traefik_conf_dir}}/certs` | Directory for custom SSL certificates |
| `gfreezy_traefik_custom_certs` | `[]` | List of custom certificates (example: `- certFile: /path/to/cert.crt keyFile: /path/to/key.key`) |
| `gfreezy_service_name` | - | Name of the service to deploy |
| `gfreezy_service_port` | `80` | Port the service listens on |
| `gfreezy_service_image` | - | Docker image for the service |
| `gfreezy_service_hostname` | - | Hostname for the service. If not set, traefik router will not be created. But you can still use `{gfreezy_service_name}-service` as a service name in the traefik configuration to write your own router. |
| `gfreezy_service_docker_volumes` | `{}` | Docker volumes to mount |
| `gfreezy_service_docker_env` | `{}` | Environment variables for the container |
| `gfreezy_service_use_custom_cert` | `false` | Whether to use custom certificates |
| `gfreezy_service_redirect_hostnames` | `[]` | List of hostnames to redirect to the service |
| `gfreezy_service_extra_hostnames` | `[]` | List of extra hostnames to add to the service |
| `gfreezy_service_redirect_to_https` | `false` | Whether to redirect HTTP to HTTPS |

## Example Usage

Here's a complete example showing how to use both roles together to deploy a service with Traefik:

```yaml
---
- name: "Deploy {{ gfreezy_service_name }} service"
  hosts: all
  become: yes # Use sudo
  user: ubuntu # Use ubuntu user
  vars:
    gfreezy_service_name: rizz
    gfreezy_service_image: "host.to.rizz.image.com/rizz"
    gfreezy_service_hostname: api.gfreezy_service_hostname.com
    gfreezy_traefik_conf_dir: /opt/service/traefik
    gfreezy_traefik_network: traefik
    gfreezy_traefik_services_conf_dir: "{{ gfreezy_traefik_conf_dir }}/services_conf"
    gfreezy_traefik_custom_certs_dir: "{{ gfreezy_traefik_conf_dir }}/certs"
    gfreezy_traefik_custom_certs:
      - certFile: /path/to/cert.crt
        keyFile: /path/to/key.key
    gfreezy_service_use_custom_cert: true
    gfreezy_service_redirect_hostnames:
      - www.gfreezy_service_hostname.com
    gfreezy_service_extra_hostnames:
      - api2.gfreezy_service_hostname.com

  tasks:
    - include_role:
        name: gfreezy.service
      vars:
        gfreezy_service_port: 8080
        gfreezy_service_docker_volumes:
          - "{{rizz_dir}}:/App/db"

    - name: Prune everything
      community.docker.docker_prune:
        images: true

```
