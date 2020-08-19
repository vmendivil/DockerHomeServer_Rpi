* Run docker compose definition to create containers:
    docker-compose -f docker-compose-traefik2.yml up -d

* Get docker group id (PGID):
    grep docker /etc/group | cut -d ':' -f 3

* Stop all containers at once:
    docker kill $(docker ps -q)
