# Vaultwarden

## Pre-requisite

1. VM or LXC installed
2. tailscale install and authenticated
3. docker is installed

## Steps

1. create directory `mkdir -p /opt/vaultwarden && cd /opt/vaultwarden`
2. create `docker-compose.yml` to start `vaultwarden` and `caddy << a reverse proxy for vault warden`

```
services:
    container_name: vaultwarden
    restart: unless-stopped
    ports:
      - "127.0.0.1:8000:80"
    environment:
      SIGNUPS_ALLOWED: true ## On 1st installation, enable this and signup your account then disable it to enhance security
      WEBSOCKET_ENABLED: true
      ADMIN_TOKEN: "${ADMIN_TOKEN}" ## argon2 hash token created by useful command below
    volumes:
      - vw-data:/data

volumes:
  vw-data:
```

3. export argon2 password hash into `/opt/vaultwarden/.env` file `ADMIN_TOKEN=<token>`
4. Start the service using `docker compose up -d`
5. run `tailscale serve --bg --https=443 http://127.0.0.1:8000` 
5. Create vaultwarden account
6. stop docker using `docker compose down` and update environment variable to `SIGNUPS_ALLOWED: false`
7. restart the docker `docker compose up -d`

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
