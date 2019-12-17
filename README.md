Clone using `$git clone --recursive  (you need to setup ssh key at github)

## Prerequisites
* docker swarm with at least one master and one worker
* ssh access to the master

## Setup

1. Clone repository to the master machine
2. Enter all private data into the docker-compose files for traefik, postgis and frost
3. Start traefik with `docker stack deploy -c traefik/docker-compose.yml traefik`.
4. Build postgis docker image `docker build -t registry.teco.edu/smartaqnet/postgis:9.5-alpine postgis`.
5. Upload docker image to registry accessable by all nodes `docker push registry.teco.edu/smartaqnet/postgis:9.5-alpine postgis`.
6. Start postgis with `docker stack deploy -c postgis/docker-compose.yml postgis` and wait for the replica containers to build the inital db.
7. Import any existsing postgis backup.
8. Start frost with `docker stack deploy -c frost/docker-compose.yml frost`. 

## Full Cleanup loosing all data!!!!

For a full cleanup every stack and its corresponding volumes needs to be removed.
```bash
# run this on one master node only
docker stack rm frost
docker stack rm postgis
docker stack rm traefik
# run this on every docker swarm node
docker system prune -af
docker volume prune -f
```
