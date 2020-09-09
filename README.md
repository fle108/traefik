# Traefik desktop setup

## __Simple setup for localhost traefik use__

> designed to run on localhost (docker desktop) but with tls activated using self signed certs in order to build dev environnement near to prod's one.

---

### docker-compose

> contains:

`traefik:latest`

### files

> contains:

```sh
./config
  ⎥–– certs/docker-cert.pem
  ⎥        /docker-key.pem
  ⎥
  ⎥–– data/traefik.toml
  ⎥       /traefik.config.toml
```

---

### labels to use with next containers

> replace values `app_name`, `container_port`, `network_name`

```dockerfile
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.<app_name>.entrypoints=http"
      - "traefik.http.routers.<app_name>.rule=Host(`<app_name>.localhost`)"
      - "traefik.http.middlewares.<app_name>-https-redirect.redirectscheme.scheme=https"
      - "traefik.http.routers.<app_name>.middlewares=<app_name>-https-redirect"
      - "traefik.http.routers.<app_name>-secure.entrypoints=https"
      - "traefik.http.routers.<app_name>-secure.rule=Host(`<app_name>.localhost`)"
      - "traefik.http.routers.<app_name>-secure.tls=true"
      - "traefik.http.routers.<app_name>-secure.service=<app_name>"
      - "traefik.http.services.<app_name>.loadbalancer.server.port=<container_port>"
      - "traefik.docker.network=<network_name>"
```

---

### use

run with `docker-compose up -d`
