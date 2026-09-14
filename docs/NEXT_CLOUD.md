# Next cloud

## Pre-requisite

1. VM or LXC installed
2. tailscale install and authenticated
3. docker is installed

## Steps

1. create directory `mkdir ~/nextcloud && cd ~/nextcloud`
2. create `.env` file and update
```
MYSQL_ROOT_PASSWORD=supersecretrootpassword
MYSQL_DATABASE=nextcloud
MYSQL_USER=nextclouduser
MYSQL_PASSWORD=supersecretuserpassword
```
3. create `docker-compose.yml` file
```
services:
  db:
    image: mariadb:10.6
    restart: always
    command: --transaction-isolation=READ-COMMITTED --binlog-format=ROW
    volumes:
      - db-val:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}
      - MYSQL_DATABASE=${MYSQL_DATABASE}
      - MYSQL_USER=${MYSQL_USER}
      - MYSQL_PASSWORD=${MYSQL_PASSWORD}

  nextcloud:
    image: nextcloud:apache
    restart: always
    ports:
      - 8080:80
    volumes:
      - nextcloud-val:/var/www/html
    environment:
      - MYSQL_HOST=db
      - MYSQL_DATABASE=${MYSQL_DATABASE}
      - MYSQL_USER=${MYSQL_USER}
      - MYSQL_PASSWORD=${MYSQL_PASSWORD}
    depends_on:
      - db

volumes:
  db-val:
  nextcloud-val:
```
5. `docker compose up -d` and `tailscale serve --bg 8080`
