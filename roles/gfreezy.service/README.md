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
- `gfreezy_traefik_network`: Docker gfreezy_traefik_network to attach to (default: traefik)
- `gfreezy_traefik_services_conf_dir`: Directory for Traefik service configurations
- `gfreezy_service_hostname`: gfreezy_service_hostname for the service

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

License
-------

GPL-2.0-or-later

Author Information
------------------

Created by gfreezy (AllSunday)
