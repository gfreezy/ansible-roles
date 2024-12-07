Traefik
=========

This role deploys Traefik as a reverse proxy with Docker.

Requirements
------------

- Docker installed on target host
- Ansible 2.1 or higher
- Cloudflare DNS API token if using DNS challenge for Let's Encrypt certificates

Role Variables
--------------

- gfreezy_traefik_conf_dir`: Directory for Traefik configuration files
- `gfreezy_traefik_services_conf_dir`: Directory for service-specific configurations
- `gfreezy_traefik_docker_traefik_conf`: Path to Traefik config directory inside container
- `gfreezy_traefik_network`: Docker gfreezy_traefik_network name for Traefik
- `gfreezy_traefik_cloudflare_dns_api_token`: Cloudflare API token for DNS challenge
- `gfreezy_traefik_custom_certs_dir`: Directory for custom certificates

Dependencies
------------

None

Example Playbook
----------------

    - hosts: servers
      roles:
         - role: gfreezy.traefik
           vars:
            gfreezy_traefik_conf_dir: /etc/traefik
             gfreezy_traefik_network: traefik
             gfreezy_traefik_cloudflare_dns_api_token: your-token-here

License
-------

GPL-2.0-or-later

Author Information
------------------

Created by gfreezy (https://github.com/gfreezy)
