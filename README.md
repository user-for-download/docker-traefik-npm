>20.04.5 LTS (Focal Fossa); Docker version 20.10.23, build 7155243; Thu Jan 26 16:50:54 2023; default user ubuntu (1000,1000)

The latest versions of dockers were used on the publication mark. Now they may not be compatible! Be careful.
Traefik USE only `HTTP` (port 82). To use `HTTPS` (port 443), configure nginx-proxy-manager
## Features
- traefik
- nginx-proxy-manager
- socket-proxy
- whoami
- grafana
- portainer
- alertmanager
- authelia
- crowdsec
- crowdsec-dashboard

![traefik](https://i.ibb.co/86ygk01/1.jpg)

## Create new secrets
```bash
cd docker-traefik-npm/
cd secrets/ && for i in *; do openssl rand -hex 16 > $i; done
cd ..
```
## Change config .env
```bash
cp .env.example .env
```
> change `SITE.DOMAIN `, `USER` and `PUID, PGID`

## Deploy first
```bash
docker network create -d bridge socket_proxy --subnet 172.16.91.0/24
docker network create -d bridge t2_proxy --subnet 172.16.90.0/24
```
> NOTE: if you run npm and traefik on the same host, then name port 80 at traefik!!!
```yaml
     ports:
       - target: 82
         published: 82
         protocol: tcp
         mode: host
```
> NOTE: otherwise you’ll have a conflict

```bash
docker-compose up -d
docker-compose -p npm -f docker-compose-npm.yml up -d
```
visit [http://ip_address:81](https://nginxproxymanager.com/) User guide - [nginx-proxy-manager](https://nginxproxymanager.com/)
> Email:    admin@example.com
> Password: changeme

## Deploy services
```bash
docker-compose -p svc -f docker-compose-svc.yml up -d
docker-compose -p crds -f docker-compose-crwd.yml up -d
docker-compose -p mntr -f docker-compose-mntr.yml up -d
```

## If you need authorization, configure 
authella works only with HTTPS! Сreate a proxy in nginx-proxy-manager and get a certificate through Lets. 
```bash
cd appdata/authelia/
cp configuration.example.yml .configuration.yml 
```
and replase all `site.domain` in configuration.yml 

> Note: also change appdata/traefik/rules/middlewares.toml 
> address:  https://auth. `site.domain`

## Deploy
```bash
docker-compose -p auth -f docker-compose-auth.yml up -d
```
and uncomment 
> #traefik.http.routers.SERVICES.middlewares: middlewares-authelia@file in docker-compose-*.yml files.