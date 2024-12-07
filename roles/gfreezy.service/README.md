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

- `service_name`: Name of the service to deploy
- `image`: Docker image to use for the service
- `service_port`: Port the service listens on (default: 80)
- `network`: Docker network to attach to (default: traefik)
- `traefik_services_conf_dir`: Directory for Traefik service configurations
- `hostname`: Hostname for the service

Optional variables:

- `docker_volumes`: Dictionary of volume mappings (default: {})
- `docker_env`: Dictionary of environment variables (default: {})

Dependencies
------------

No dependencies on other roles.

Example Playbook
----------------

    - hosts: servers
      roles:
         - role: gfreezy.service
           service_name: myapp
           image: nginx:latest
           service_port: 8080
           hostname: myapp.example.com

License
-------

GPL-2.0-or-later

Author Information
------------------

Created by gfreezy (AllSunday)
