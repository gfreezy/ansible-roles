Service Role
=========

This role deploys a service using blue/green deployment strategy with Docker containers.

Requirements
------------

- Docker must be installed on the target host
- Traefik must be configured as the reverse proxy

Role Variables
--------------

Required variables:

- `gfreezy_service_name`: Name of the service to deploy
- `gfreezy_service_image`: Docker gfreezy_service_image to use for the service
- `gfreezy_service_port`: Port the service listens on (default: 80)
- `gfreezy_service_hostname`: gfreezy_service_hostname for the service
- `gfreezy_traefik_conf_dir`: Directory for Traefik configuration files
- `gfreezy_traefik_services_conf_dir`: Directory for service-specific configurations
- `gfreezy_traefik_docker_traefik_conf`: Path to Traefik config directory inside container
- `gfreezy_traefik_network`: Docker gfreezy_traefik_network name for Traefik
- `gfreezy_traefik_cloudflare_dns_api_token`: Cloudflare API token for DNS challenge
- `gfreezy_traefik_custom_certs_dir`: Directory for custom certificates

Optional variables:

- `gfreezy_service_docker_volumes`: Dictionary of volume mappings (default: {})
- `gfreezy_service_docker_env`: Dictionary of environment variables (default: {})

Dependencies
------------

No dependencies on other roles.

Example Playbook
----------------

    - hosts: servers
      roles:
         - role: gfreezy.service
           gfreezy_service_name: myapp
           gfreezy_service_image: nginx:latest
           gfreezy_service_port: 8080
           gfreezy_service_hostname: myapp.example.com
           gfreezy_traefik_conf_dir: /etc/traefik
           gfreezy_traefik_network: traefik
           gfreezy_traefik_cloudflare_dns_api_token: your-token-here

License
-------

GPL-2.0-or-later

Author Information
------------------

Created by gfreezy (AllSunday)
