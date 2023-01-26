>VERSION="20.04.5 LTS (Focal Fossa)"; Docker version 20.10.23, build 7155243; Thu Jan 26 16:50:54 2023

The latest versions of dockers were used on the publication mark. Now they may not be compatible! Be careful.
Docker USE only `HTTP`. To use `HTTPS`, configure nginx-proxy-manager
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
cd  secrets/
for i in *; do openssl rand -hex 16 > $i; done
```
## Change configs
```bash
cd appdata/authelia/
cp configuration.example.yml .configuration.yml 
```
and replase all `site.domain` in configuration.yml 

>Note: also change appdata/traefik/rules/middlewares.toml 
http://authelia:9091/api/verify?rd=https://auth.`site.domain`"

## Deploy
```bash
docker network create -d bridge socket_proxy --subnet 172.16.91.0/24
docker network create -d bridge t2_proxy --subnet 172.16.90.0/24
docker-compose up -d
docker-compose -p npm -f docker-compose-npm.yml up -d
docker-compose -p svc -f docker-compose-svc.yml up -d
docker-compose -p auth -f docker-compose-auth.yml up -d
docker-compose -p crds -f docker-compose-crwd.yml up -d
docker-compose -p mntr -f docker-compose-mntr.yml up -d
```