## Rebuild single container (and not its dependencies)

```
$ docker-compose up -d --no-deps --build <service_name>
```

## Remove everything

```
$ docker system prune -a --volumes
```

## Remove more specific stuff

<https://linuxize.com/post/how-to-remove-docker-images-containers-volumes-and-networks/>

## YQ and Docker-compose

TODO: explain

```
$ yq m docker-compose.yml docker-compose.local.yml | yq d - 'services.frontend' | docker-compose -f - up -
```
