### KREIRAMO LINUX KORISNIKA

`sudo useradd --no-create-home clientID`

gdje je *ID* jedinstveni id tenanta iz baze podataka

### UID i GID za kreiranog korisnika

`id -u client1`

`id -g client1`

### LARAVEL APLIKACIJA jednog TENANTA - docker container
Pokrece se u docker container-u.
Docker container pokrecemo i zaustavljamo iz *manager* aplikacije.
Docker container se kreira i pokrece iz foldera **clientID** pozivom komande `docker-compose up`:
    - `docker-compose.yml` je file za kreiranje docker image-a, kopiramo ga iz ./container direktorija i isti je za svakog tenanta
    - `.env` file mapiramo na `.env` file unutar docker container-a (laravel aplikacija) jednog tenanta
    - `.env` file sadrzi i dodatne promjenjive koje se mapiraju na env varijable u `docker-compose.yml`
    - *manager* aplikacij kreira novog klijenta, tj folder clientID, gdje je ID id tenanta iz baze podataka


### MYSQL BAZA

Pokrece se kao servis na posebnom serveru.
Root pristup ima samo admin.
Za svakog korisnika kreira se novi db korisnik i baza.
Kako pravim korisnika i kako da mu dodjelim prava na bazi?

### REDIS BAZA

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
