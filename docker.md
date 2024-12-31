

# Networking
## [Networking using the host network](https://docs.docker.com/engine/network/tutorials/host/)
This series of tutorials deals with networking standalone containers which bind directly to the Docker host's network, with no network isolation.

**Procedure**
1. Create and start the container as a detached process. The `--rm` option means to remove the container once it exits/stops. The `-d` flag means to start the container detached (in the background).
```
$ docker run --rm -d --network host --name my_nginx nginx
```

# Reference
[dockerdocs manuals](https://docs.docker.com/manuals/)
