# Immich

## Pre-requisite

1. VM or LXC installed
2. tailscale install and authenticated
3. docker is installed

## Steps

1. create directory `mkdir -p /opt/immich-app && cd /opt/immich-app`
2. clone immich docker-compose file `wget -O docker-compose.yml https://github.com/immich-app/immich/releases/latest/download/docker-compose.yml`
3. clone immich env file `wget -O .env https://github.com/immich-app/immich/releases/latest/download/example.env`
4. update `.env` file
```
UPLOAD_LOCATION=/opt/immich-app/library
DB_DATA_LOCATION=/opt/immich-app/postgres
TZ=Asia/Bangkok
DB_PASSWORD=<Random password>
```
5. `docker compose up -d` and `tailscale serve --bg 2283`

## Useful

- to migrate from Google photos `takeout.google.com` with `immich-go`

## Reference

[Guide](https://techfuelhq.com/tutorials/immich-google-photos-migration-2026)
