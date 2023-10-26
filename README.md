Ansible Role: Docker Compose Traefik
=========

Creates directories and files for a Traefik setup, focussed on Cloudflare DNS provider.

Requirements
------------

If you use DNS challenge for TLS certificates, you need a DNS provider that allows API access, is supported by Traefik and a token to provide to Traefik.

Role Variables
--------------

The traefik files will be copied to:

```yaml
traefik_basepath: /srv/docker/traefik
```

Set desired Traefik version:

```yaml
traefik_version: 2.10
```

Change the container name:

```yaml
traefik_container_name: traefik
```

Create and join networks; By default, this role will create the network `internet` and connect Traefik to it. 
Here it is possible to create one network per Docker container that shall be reverse proxied to by Traefik, and connect both Traefik and the container to it. 
This way, it is possible to prohibit Internet access for single containers.

```yaml
traefik_proxy_networks:
  - proxy_network_1
  - proxy_network_1
```

Enable Traefik Dashboard on port 8080:

```yaml
traefik_enable_api: no
traefik_enable_api_insecure: no
```

Change log level and format:

```yaml
traefik_log_enable: yes
traefik_log_level: ERROR      # [DEBUG, INFO, WARNING, ERROR, CRITICAL]
traefik_log_format: common    # [common, json, logfmt]
traefik_log_file_path: /var/log/traefik/traefik.log
```

# Enable accesslog and change format:
```yaml
traefik_access_log_enable: yes      # yes or no
traefik_access_log_format: common   # [common, json, logfmt]
traefik_access_log_file_path: /var/log/traefik/access.log
```

Adapt the TLS certificate resolver settings. If you use `dns_challenge`, you have to set `http_challenge` to `no`, and vice versa.

`dns_challenge_provider` - e.g. cloudflare.

```yaml
traefik_tls_certresolver:
  staging:
    acme_email: your_email@example.com
    http_challenge: no
    http_challenge_entry_point: web
    dns_challenge: yes
    dns_challenge_provider: your_resolver
    dns_challenge_use_custom_resolvers: no
    dns_challenge_resolvers:
      - "1.1.1.1:53"
      - "8.8.8.8:53"
  production:
    acme_email: your_email@example.com
    http_challenge: no
    http_challenge_entry_point: web
    dns_challenge: yes
    dns_challenge_provider: your_resolver
    dns_challenge_use_custom_resolvers: no
    dns_challenge_resolvers:
      - "1.1.1.1:53"
      - "8.8.8.8:53"
```

Whether Traefik should verify TLS certificates, turn this off when you intend to use self-signed certificates.

```yaml
traefik_disable_tls_cert_verification: true
```

Change settings for Traefik `docker` and `file` providers:

```yaml
traefik_providers:
  docker:
    expose_by_default: false
  file:
    directory: /etc/traefik
    watch: true
```

Insert your Cloudflare DNS token:

```yaml
traefik_cf_dns_api_token: your_secure_token
```

Dependencies
------------

None.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
