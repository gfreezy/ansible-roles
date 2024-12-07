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

- `conf_dir`: Directory for Traefik configuration files
- `services_conf_dir`: Directory for service-specific configurations
- `docker_traefik_conf`: Path to Traefik config directory inside container
- `network`: Docker network name for Traefik
- `cloudflare_dns_api_token`: Cloudflare API token for DNS challenge
- `custom_certs_dir`: Directory for custom certificates

Dependencies
------------

None

Example Playbook
----------------

    - hosts: servers
      roles:
         - role: gfreezy.traefik
           vars:
             conf_dir: /etc/traefik
             network: traefik
             cloudflare_dns_api_token: your-token-here

License
-------

GPL-2.0-or-later

Author Information
------------------

Created by gfreezy (https://github.com/gfreezy)
