# Traefik Jaeger sample integration

Small docker-compose project for Træfik + Jaeger integration.

## How to start?

- Ensure you have Docker CE and Docker Compose installed.

- Start by editing your `/etc/hosts` file so that the folloiwng domains points to the IP
of your Docker Engine (usually `127.0.0.1`):

```text
127.0.0.1       whoami.docker.localhost jaeger.docker.localhost hotrod.docker.localhost
```

- Start the application stack with the following command:

```shell
docker-compose up -d
```

- In your favorite web browser open <http://hotrod.docker.localhost> and <http://jaeger.docker.localhost>

Thanks to [Jaeger hotrod example](https://github.com/jaegertracing/jaeger/tree/master/examples/hotrod)
