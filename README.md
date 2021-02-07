# Pimcore project

## 1. Clone Repositories

Create project folder:  
`mkdir pimcore`

## 2. Configure Docker

`docker-compose -v` => docker-compose grater than 1.22

check docker version:  
`docker -v` => docker engine version grater than 17.12.0+

Run docker compose, this will take some time:  
this command should be run inside the `pimcore/` where the docker-compose file exists
`docker-compose up`

check that all docker containers are up and running :  
`docker ps` or `docker-compose ps`

here you should see running all the services that you find in docker-compose file.

## 3. Docker

`docker exec -it -u pimcore pimcore-php bash`

in this moment you should be connected to the container and be in `/var/www/pimcore` folder

here you should run the composer command:  
`composer install`

inspect network to get getway ip:
`docker network inspect pimcore-backend`

there you should be able to see something similar to this:

```
"IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "192.168.112.0/20",
                    "Gateway": "192.168.112.1"
                }
            ]
        },
```

don't forget to add to your local `/etc/hosts` the plugy.hastb on localhost
`192.168.112.1 pimcore.elem`

add nginx config the include fastcgi_params;
check procedures in the db:
set global log_bin_trust_function_creators=1;
