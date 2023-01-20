# docker-cloudflare
- traefik
- nginx-proxy-manager
- socket-proxy
- whoami
- portainer
- authelia
- crowdsec
- crowdsec-dashboard

before
```bash
docker network create -d bridge socket_proxy --subnet 172.16.91.0/24
docker network create -d bridge t2_proxy --subnet 172.16.90.0/24
```