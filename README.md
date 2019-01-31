# Docker for Building a Local Development Environment

This project provides a basic Docker setup, for building a local development environment for linux, nginx ,mysql, PHP.
Nginx  can setting multi vhost application development.

## Why?

Why?
Good question.
But easy to answer.
The intent is to provide a starting point for developers to get up and running quickly, using Docker, to build a local development environment.


## Installation

To get up and running, after cloning the repository:

1. Install Docker. If you’re running a Linux distribution, use its package manager. If you’re using either macOS or Windows, download the respective Docker package installers: [Docker for Mac](https://docs.docker.com/docker-for-mac/) or [Docker for Windows](https://docs.docker.com/docker-for-windows/).

2. git clone https://github.com/kcloze/docker-for-local-development.git, you can clone it in your any path.

3. In `docker-compose.yml` change the default php volumes  setting, `/PATH/TO/YOUR/DOCUMENT/ROOT`, to suit your web root path . 

4. More than likely, there will be a `/PATH/TO/YOUR/DOCUMENT/ROOT/site1.com/` directory in your source. So change  `docker/nginx/site1.conf` setting to be `root /var/www/html/site1.com;`.

5. Setting  `/etc/hosts`, add `127.0.0.1 www-local.site1.com`.

5. Make sure `/PATH/TO/YOUR/DOCUMENT/ROOT` has index.html and index.php file.

6. Build the configuration by running: `docker-compose -f docker-compose-php72.yml up -d`.

7. Open browser urls: `localost:8090` and `www-local.site1.com:8090`.

8. Check log file by runnig `docker-compose logs -f`.


### Check That Everything Is Working

To check that the build is working, run `docker-compose ps`.
This should give you output similar to the below.

```console
               Name                             Command             State               Ports
---------------------------------------------------------------------------------------------------------
dockerforlocaldevelopment_mysql_1     docker-entrypoint.sh mysqld   Up      3306/tcp
dockerforlocaldevelopment_nginx_1     nginx -g daemon off;          Up      443/tcp, 0.0.0.0:8090->80/tcp
dockerforlocaldevelopment_php_1       php-fpm                       Up      9000/tcp
```

If you’d like to see more detailed information, check the log file by running `docker-compose logs`.
To check that the files are inside the container, run the following command, substituting `dockerforlocaldevelopment` to match your directory name:

```console
docker exec -it dockerforlocaldevelopment_nginx_1 ls -lahrt /var/www/html
```

## Contributing

See the [CONTRIBUTING](CONTRIBUTING.md) file.

## License

This project is licensed under the [MIT License](/LICENSE).
