# Apache-Guacamole-easy-docker-install
### Finally a easy way to install Apache guacamole using docker. <br />
Thanks to [Sonoran Tech](https://www.youtube.com/@SonoranTech-hf5hf) for the base of my docker compose and comand list.


## Install Docker
First wee need to install docker on the host system of your choosing. Please follow the guide from the offical side [here](https://docs.docker.com/engine/install/).<br />
Portainer is optional.

## Rady the Docker-Compose and pull every container
Download or copy the Guacamole docker-compose to the file directory of your choosing. then:

```
sudo docker compose up -d
```

afeer all is pulled and running use:

```
sudo docker ps
```
