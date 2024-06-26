# Introduction to Nginx with HTTPS on Docker

This project aims to provide a simple example of how to set up an Nginx server with HTTPS encryption provided Let's Encrypt, using the official Nginx and certbot/certbot Docker images.

## Structure of the project
The project has two docker-compose files, with some slight differences:

`docker-compose.yml` is the file that may be used to run the certbot commands to acquire and set up the certificates.

`docker-compose.prod.yml` contains some additional `command` and `entrypoint` lines that serves to keep the system running and automatically check for new certificates and reload nginx, as explained [in this article](https://pentacent.medium.com/nginx-and-lets-encrypt-with-docker-in-less-than-5-minutes-b4b8a60d3a71).

## About the use of docker volumes

As explained in the documentation for Certbot under [Where are my certificates](https://eff-certbot.readthedocs.io/en/stable/using.html#where-certs), do note that `/etc/letsencrypt/archive` and `/etc/letsencrypt/keys` contain all keys and certificates, while `/etc/letsencrypt/live` symlinks to the latest versions. Hence, it is important to include the entire `/etc/letsencrypt/` directory for the relevant docker volume, not just the `/etc/letsencrypt/live` directory.

Without the entire `/etc/letsencrypt/` directory, the certificates and keys will not be found inside the docker containers, as the symlinks has no actual files to link to.

## Testing Certbot with `--dry-run`

First, make sure the Nginx webserver is running, and that any SSL parts of the Nginx config is commented out. In other words, only use a server context on port 80 in Nginx. Then enter command:

```
sudo docker compose run --rm  certbot certonly --webroot --webroot-path /var/www/certbot/ --dry-run -d <server_name>
```

## Aquiring certificates from Let's encrypt

To aquire the necessary certificates, run the same command as for testing certbot, but without the `--dry-run` flag.

```
sudo docker compose run --rm  certbot certonly --webroot --webroot-path /var/www/certbot/ -d <server_name>
```

## SSL server configuration

The SSL server configuration is informed by Mozilla's [SSL Configuration Generator](https://ssl-config.mozilla.org/) with the following settings applied:

- Server Software: nginx
- Mozilla Configuration: Modern
- Environment:
  - Server Version: 1.23.1
  - OpenSSL Version: 3.0.5
- Miscellaneous
  - HTTP Strict Transport Security enabled
  - OCSP Stapling disabled

With the configurations from 220904, the server received an "A" rating from [SSL Labs](https://www.ssllabs.com/ssltest).

## How to install Docker on a server

Install from the docker repo:
[https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-22-04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-22-04)

For a quick install, using a script from `get.docker.com`:
[https://github.com/docker/docker-install](https://github.com/docker/docker-install)

## Sources

- Main guide followed: [HTTPS using Nginx and Let's encrypt in Docker](https://mindsers.blog/post/https-using-nginx-certbot-docker/)
- Additional advice and scripts for automatic renewal of certificates: [Nginx and Let’s Encrypt with Docker in Less Than 5 Minutes](https://pentacent.medium.com/nginx-and-lets-encrypt-with-docker-in-less-than-5-minutes-b4b8a60d3a71)
- Official docs from Certbot: [Get Certbot Running with Docker](https://eff-certbot.readthedocs.io/en/stable/install.html#running-with-docker)
