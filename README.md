## Stack
Inspired on [Carlosas stack](https://github.com/carlosas/docker-for-symfony)

* [nginx](https://nginx.org/)
* [PHP-FPM](https://php-fpm.org/)
* [MySQL](https://www.mysql.com/)
* [Redis](https://redis.io/)
* [Elasticsearch](https://www.elastic.co/products/elasticsearch)
* [Logstash](https://www.elastic.co/products/logstash)
* [Kibana](https://www.elastic.co/products/kibana)
* [RabbitMQ](https://www.rabbitmq.com/)

## Previous requirements

This stack needs [docker](https://www.docker.com/) and [docker-compose](https://docs.docker.com/compose/) to be installed.

## Installation

1. Create a `.env` file from `.env.dist` and adapt it according to the needs of the application

    ```sh
    $ cp .env.dist .env && nano .env
    ```

2.  Due to an Elasticsearch 6 requirement, we may need to set a host's sysctl option and restart ([More info](https://github.com/spujadas/elk-docker/issues/92)):

    ```sh
    $ sudo sysctl -w vm.max_map_count=262144
    ```

3. Build and run the stack in detached mode (stop any system's ngixn/apache2 service first)

    ```sh
    $ docker-compose build
    $ docker-compose up -d
    ```

4. For enter on a bash of PHP container:

        ```sh
        $ docker-compose exec php bash
        ```
5. Get the bridge IP address

    ```sh
    $ docker network inspect bridge | grep Gateway | grep -o -E '[0-9\.]+'
    # OR an alternative command
    $ ifconfig docker0 | awk '/inet:/{ print substr($2,6); exit }'
    ```

6. Update your system's hosts file with the IP retrieved in **step 3**

## Usage

Once all the containers are up, our services are available at:

* Symfony app: `http://dddpatterns.local:80`
* Mysql server: `dddpatterns.local:3306`
* Redis: `dddpatterns.local:6379`
* Elasticsearch: `dddpatterns.local:9200`
* Kibana: `http://dddpatterns.local:5601`
* RabbitMQ: `http://dddpatterns.local:15672`
* Log files location: *logs/nginx* and *logs/symfony*

---

:tada: Now we can stop our stack with `docker-compose down` and start it again with `docker-compose up -d`
