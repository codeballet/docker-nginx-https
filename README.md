## About the docker volumes

As explained in the documentation for Certbot under [Where are my certificates](https://eff-certbot.readthedocs.io/en/stable/using.html#where-certs), do note that `/etc/letsencrypt/archive` and `/etc/letsencrypt/keys` contain all keys and certificates, while `/etc/letsencrypt/live` symlinks to the latest versions. Hence, it is important to include the entire `/etc/letsencrypt/` directory for the relevant docker volume, not just the `/etc/letsencrypt/live` directory. Otherwise, the certificates and keys will not be found, as the symlinks has no actual files to link to.

## Testing Certbot with `--dry-run`

First, make sure the Nginx webserver is running, and that any SSL parts of the Nginx config is commented out. In other words, only use a server context on port 80 in Nginx. Then enter command:

```
docker compose run --rm  certbot certonly --webroot --webroot-path /var/www/certbot/ --dry-run -d <server_name>
```

## Aquiring certificates from Let's encrypt

To aquire the necessary certificates, run the same command as for testing certbot, but without the `--dry-run` flag.

```
docker compose run --rm  certbot certonly --webroot --webroot-path /var/www/certbot/ -d <server_name>
```

## How to install Docker on a server

Install from the docker repo:
[https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-22-04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-22-04)

For a quick install, using a script from `get.docker.com`:
[https://github.com/docker/docker-install](https://github.com/docker/docker-install)

## Sources

- [HTTPS using Nginx and Let's encrypt in Docker](https://mindsers.blog/post/https-using-nginx-certbot-docker/)
- [Nginx and Let’s Encrypt with Docker in Less Than 5 Minutes](https://pentacent.medium.com/nginx-and-lets-encrypt-with-docker-in-less-than-5-minutes-b4b8a60d3a71)
- [Get Certbot Running with Docker](https://eff-certbot.readthedocs.io/en/stable/install.html#running-with-docker)
