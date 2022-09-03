## Testing Certbot with `--dry-run`

First, make sure the Nginx webserver is running, and that any SSL parts of the Nginx config is commented out. In other words, only use a server context on port 80 in Nginx. Then enter command:

```
docker compose run --rm  certbot certonly --webroot --webroot-path /var/www/certbot/ --dry-run -d <server_name>
```

## Install Docker on server

From a repo:
https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-22-04

As a quick script from get.docker.com:
https://github.com/docker/docker-install

## Sources

- [HTTPS using Nginx and Let's encrypt in Docker](https://mindsers.blog/post/https-using-nginx-certbot-docker/)
- [Nginx and Let’s Encrypt with Docker in Less Than 5 Minutes](https://pentacent.medium.com/nginx-and-lets-encrypt-with-docker-in-less-than-5-minutes-b4b8a60d3a71)
- [Get Certbot Running with Docker](https://eff-certbot.readthedocs.io/en/stable/install.html#running-with-docker)
