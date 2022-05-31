# Koraci za dodavanje novog klijenta

- kreiram linux korisnika koji ce se zvati po id-u klijenta:
`sudo useradd --no-create-home client1`

- uzmem UID i GID za kreiranog korisnika i smjestim u bazu, potrebni su mi za .env file klijenta:
`id -u client1`
`id -g client1`

- kreiramo folder client1 kao
- u njega kopiramo

- za .env file klijenta su mi jos potrebni:
TZ - timezone
APP_SRC - izabr iz liste foldera, iz ./apps
APP_DOMAIN - to je web domen klijenta, mapira se u VIRTUAL_HOST env varijablu u docker-compose.yml, a onda se koristi u nginx-proxy

sve ostale env varijable bi takodje trebalo biti konfigurabilne, tj treba ih smjestiti u bazu podataka uz klijenta

- takodje moramo kreirati i bazu podataka sa posebnim korisnikom za prisutup bazi, trebamo zapamtiti user i pass za pristup bazi


# Laravel aplikacija jednog tenanta
- pokretat ce se u docker container-u
- docker container pokrecemo i zaustavljamo iz nc aplikacije
- docker container se kreira iz foldera clientID:
    - docker-compose.yml file za kreiranje docker image-a kopiramo iz ./container direktorija i isti je za svakog tenanta
    - .env file mapiramo na .env file unutar docker container-a (laravel aplikacija) jednog tenanta
    - .env file sadrzi i dodatne promjenjive koje se mapiraju na env varijable u docker-compose.yml
    - nc kreira novog klijenta, tj folder clientID, gdje je ID id tenanta iz baze podataka

# Laravel aplikacija - konfiguracija docker container-a
- treba imati Dockerfile koji konfigurise docker container
- treba imati ./docker folder sa fajlovima za apache, php i supervisord konfiguraciju kao i za inicijalizaciju kontejnera:
    - start-container
    - supervisord.conf
    - php.ini
    - vhost.conf


