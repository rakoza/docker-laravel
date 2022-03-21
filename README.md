### LINUX KORISNIK
sudo useradd --no-create-home client2

### LARAVEL APLIKACIJA
Za svaku instancu pravim posebni docker container.

### BAZA PODATAKA

Pokrece se kao servis na posebnom serveru
Root pristup ima samo admin
Za svakog korisnika kreira se novi db korisnik i baza.
Kako pravim korisnika i kako da mu dodjelim prava na bazi?

### REDIS

Pokrece se kao servis na posebnom server

### NGINX PROXY

Koristit cu [nginx-proxy](https://github.com/nginx-proxy/nginx-proxy) docker image.

`nginx-proxy` sets up a container running nginx and docker-gen. `docker-gen` generates reverse proxy configs for nginx and reloads nginx when containers are started and stopped.


1. `docker pull nginxproxy/nginx-proxy:alpine`
2. `docker network create nginx-proxy-net`
3. dodam docker-compose.yml za pokretanje proxija:
```
version: '3'

services:
  nginx-proxy:
    image: nginxproxy/nginx-proxy:alpine
    ports:
      - "789:80"
    volumes:
      - /var/run/docker.sock:/tmp/docker.sock:ro


networks:
  default:
    external:
      name: nginx-proxy-net

```
4. klijentu dodam sljedece:
 - u .env file `APP_DOMAIN=rakoza.local`
 - u docker-compose.yml
```
services:
    app:
        environment:
            VIRTUAL_HOST: ${APP_DOMAIN}
```
i
 ```
 networks:
    default:
        external:
            name: nginx-proxy-net
```
