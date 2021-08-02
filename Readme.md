# Lotterycamp

This is a fun application where lottery is run every minute and user can participate for a small minimal fee. I created this application to explore new technologies just for fun.

**Technology Used**

- Laravel (Main API)
- React (Frontend/Backend)
- Coinbase (Crypto Transaction API)
- Redis (Queue Driver)
- Laravel Echo Server (Realtime Web Socket Server)
- Mysql (Main Database)

## Features

- User dashboard and wallet
- Full featured admin area
- Lottery runs every minute
- Cryptocurrency payment
- Bot Users

## Installation

```
# create projects directory and clone the repo inside it
# it is to use the default  network name included in docker-compose.yml file
# or you can update the network name.
mkdir projects && cd projects

# clone the project along with sub-modules:
git clone --recurse-submodules git@github.com:limvus/lotterycamp.git

# update project with remote repo in future
git pull --recurse-submodules

# copy docker-compose.yml outside of the repo
# again just so that the default network name will be projects_default
# which is what we have in the docker-compose.yml
# or you can update the network name.
cp docker-compose.yml ../

# run docker-compose
# the docker-compose will run nginx-proxy and lets-encrypt container
# the first one is to automatically support virtual host and second one is to support https
docker-compose up -d
docker-compose logs --follow # view logs

# add vhost in /etc/hosts
# nginx-proxy will use vhosts name in the file to route request to respective container
# also lets-encrypt container will generate ssl certificates automatically provided following
# environment variables are set in the serving nginx container
# VIRTUAL_HOST=lotterycamp.com
# LETSENCRYPT_HOST=lotterycamp.com
127.0.0.1 lotterycamp.com

# for local installation, you need mkcert to install self-signed certificates
# first install mkcert then run:
mkcert install # this will make local browsers trust our certificates
# then generate certificate
cd ~/
mkdir .certs && cd .certs
mkcert localhost
mkcert lotterycamp.local
mkcert api.lotterycamp.local
mkcert backend.lotterycamp.local
# rename certificates in the form hostname.crt and hostname.key
lotterycamp.crt lotterycamp.key
api.lotterycamp.crt api.lotterycamp.key
# for local, letsencrypt container won't be able to generate certificates so we need to update docker-compose.yml volumes
remove certs from volume
map local certs directory to container volumes
- /home/sudip/.certs:/etc/nginx/certs:ro
# restart docker
docker-compose down && docker-compose up -d

# this will setup vhost, https and reverse-proxy required for our app to run.
# Now its time to go to individual app and setup there

```

- Follow installation guide for each sub-modules
  - [lottery-api](https://github.com/limvus/lottery-api)
  - [lottery-backend](https://github.com/limvus/lottery-backend)
  - [lottery-frontend](https://github.com/limvus/lottery-frontend)

## Deployment

I used namecheap for domain hosting, cloudflare for caching and dns service, and digitalocean for site hosting.

```
# digitalocean
- spin up droplet with ubuntu 20.04LTS
- setup git, docker and docker-compose
- link ssh connection to github repo and pulled the repo
and followed the instruction to setup site
- copy the droplet public ip address

# cloudflare
- in DNS, add a name and cname for domain
A lotterycamp.com 159.89.48.212 auto proxied
A www 159.89.48.212 auto proxied
cname api lotterycamp.com auto proxied
cname api lotterycamp.com auto proxied
- in ssl/tls
use full(strict) encryption (had issue with other before)
- in caching
use standard caching level
set 1 month or more for browser cache TTL (helps in gtmetrix score)
don't forget to purge cache here when you make changes in you code and it doesn't reflect

# namecheap
- put cloudflare dns info in the custom dns field of the domain

# setting up services
- same as in local
```

## Contribution

If you want to contribute, just fork the repository and play around, create
issues and submit the pull request. Help is always welcomed.

## Security

If you discover any security related issues, please email 4slimbu@gmail.com
instead of using the issue tracker.

## License

The scripts and documentation in this project are released under the MIT License

## Author

Sudip Limbu
