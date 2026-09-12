# Vaultwarden

## Pre-requisite

1. VM or LXC installed
2. tailscale install and authenticated
3. docker is installed

## Steps

1. create vaultwarden directory `mkdir -p /opt/vaultwarden && cd /opt/vaultwarden`
2. create `docker-compose.yml` to start `vaultwarden` and `caddy << a reverse proxy for vault warden`

```
services:
    container_name: vaultwarden
    restart: unless-stopped
    environment:
      SIGNUPS_ALLOWED: true ## On 1st installation, enable this and signup your account then disable it to enhance security
      WEBSOCKET_ENABLED: true
      ADMIN_TOKEN: "${ADMIN_TOKEN}" ## argon2 hash token created by useful command below
    volumes:
      - vw-data:/data

  caddy:
    image: caddy:2
    container_name: caddy
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - caddy_data:/data
      - caddy_config:/config
    depends_on:
      - vaultwarden

volumes:
  vw-data:
  caddy_data:
  caddy_config:
```

3. export argon2 password hash into `/opt/vaultwarden/.env` file `ADMIN_TOKEN=<token>`
4. create `Caddyfile` to reverse the proxy back to tailscale DNS

```
<tailscale DNS name e.g. vaultwarden.mackarel-pike.ts.net> {
    reverse_proxy vaultwarden:80
    tls internal
}
```

5. Start the service using `docker compose up -d`
6. Create vaultwarden account
7. stop docker using `docker compose down` and update environment variable to `SIGNUPS_ALLOWED: false`
8. restart the docker `docker compose up -d`


## Useful command

```bash
    docker run --rm -it vaultwarden/server /vaultwarden hash ## Generating argon2 hashed password

    docker compose up -f <custom filename apart from docker-compose.yml> -d     ## -d is shorten from detached
    docker compose down -v                                                      ## stop current docker instance (-v to delete the container)
    docker compose ls                                                           ## list all composed files
    docker compose ps                                                           ## list all containers
    docker rm <container>
```

# Reference

- [reference of some random guy](https://www.jackywarner.com/blog/cyber/part2/)
