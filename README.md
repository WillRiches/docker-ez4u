# ez4u Docker configuration

Based on [https://github.com/maxpou/docker-symfony](maxpou/docker-symfony)

## Installation

1. Create a `.env` from the `.env.dist` file. Adapt it according to your Symfony application

    ```bash
    cp .env.dist .env
    ```


2. Build/run containers with (with and without detached mode)

    ```bash
    $ docker-compose build
    $ docker-compose up -d
    ```

3. Prepare Symfony app
    1. Update app/config/parameters.yml

        ```yml
        # path/to/your/symfony-project/app/config/parameters.yml
        parameters:
            database_host: db
        ```

    2. Composer install & create database

        ```bash
        $ docker-compose exec php bash
        $ composer install
        $ sf3 doctrine:database:create
        $ sf3 doctrine:schema:update --force
        ```

## Usage

Just run `docker-compose up -d`, then:

* Symfony app: visit [localhost:80](http://localhost:80)  
* Kibana: [localhost:81](http://localhost:81)
* PhpMyAdmin: [localhost:8080](http://localhost:8080)
* Log files: logs/nginx and logs/symfony

## Useful commands

```bash
# bash commands
$ docker-compose exec php bash

# Composer (e.g. composer update)
$ docker-compose exec php composer update

# SF commands (Tip: there is an alias inside php container)
$ docker-compose exec php sf3 cache:clear
# Same command by using alias
$ docker-compose exec php bash

# Retrieve an IP Address (here for the nginx container)
$ docker inspect --format '{{ .NetworkSettings.Networks.dockersymfony_default.IPAddress }}' $(docker ps -f name=nginx -q)
$ docker inspect $(docker ps -f name=nginx -q) | grep IPAddress

# MySQL commands
$ docker-compose exec db mysql -uroot -p"root"

# Check CPU consumption
$ docker stats $(docker inspect -f "{{ .Name }}" $(docker ps -q))

# Delete all containers
$ docker rm $(docker ps -aq)

# Delete all images
$ docker rmi $(docker images -q)
```

## FAQ
* How to config Xdebug?
Xdebug is configured out of the box!
Just config your IDE to connect port  `9001` and id key `PHPSTORM`
