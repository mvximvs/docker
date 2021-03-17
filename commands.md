docker container run --publish 80:80 nginx

-detach: tells docker to run in the bg

docker  container ls (ps the old way)
docker  container stop <idnumber>
docker  container ls -a (list all)
docker  container run --publish 80:80 --detach --name webhost nginx (detach can be shortened with -d)
docker  container logs webhost (name of the container)
docker  container logs top webhost (the process running inside the container)
docker  container --help
docker  container rm ids ids ids (remove an array of not running containers)
docker  container rm -f ids (force deletion)
docker  container stop id

ps aux (show all the processes running on the server)

docker run

## **what happends when running containers?**
- he will look for that image in the image cache if not found it will look on the docker hub
- if no version is specified he will choose the latest
- he will start a new container based on that image... a new layer on top of that image
- get a vitual ip on the private network inside docker engine
- open port 80 on host and forwards to port 80 in container
- localport:dockerimageport
- you can specify versions of your image


**assignments**
- docs.docker.com and --help command
- run a nginx, a mysql, and a httpd (apache) server
- run all of them --detach or -d name them with --name
- nginx should listen on 80:80 httpd on 8080:80, mysql 3306:3306
- when running mysql use the --env option or -e to pass in MYSQL_RANDOM_ROOT_PASSWORD=YES
- use docker container logs on mysql to find the random password it created on startup
- clean it all up with docker container stop and docker container rm (both can accept multiple names or ids)
- use docker container ls to ensure everything is correct before and after cleanup

**result**
`docker container run --publish 80:80 -d --name webserver `
`docker container run --publish 3306:3306 -d --name mysqldb -e MYSQL_RANDOM_ROOT_PASSWORD=yes mysql`
`docker container run --publish 8080:80 -d --name apacheserver httpd`

## CLI process monitoring
running docker commands to see what's going on inside a container
- top (lists de processes running inside that container)
- inspect ( metadata about the container startup config, volumes, networking)
- stats (monitoring of resources)

## Getting inside a container with bash command`
getting into the container
- docker container run -it (start a new container interactively) (it: -i: interactive keep session open; -t:tty terminal)
- docker container exec -it (run additional command in existing container)
- different linux distros in containers
**example**
  - docker container run -it --name proxy nginx bash
  - start existing container with docker container -ai container (ai: attach interactive)
  - docker container exec -it mysql bash (shell inside an existing running container)
  - to monitor processes inside a container you can use ps aux (apt-get update && apt-get install -y procps) 
  - docker pull alpine
  - error: 
    OCI runtime exec failed: exec failed: container_linux.go:370: starting container process caused: exec: "bash": executable file not found in $PATH: unknown
    docker container exec -it container sh ( or /bin/sh or  /bin/bash)

## Docker networks => private and public comms in containers
  - Concepts
    - docker container port containername (ports open in a container)
    - each container connected to a private virtual network "bridge"
    - each virtual network routes through NAT firewall on host IP
    - all containers on a virtual network cant talk to each other without -p
    - you can create custom networks to isolate containers
    - "Batteries includes, but removable"
    - make new virtual networks
    - attach container to more then one virtual network
    - skip virtual networks and use host IP (--net=host)
    - use different Docker network drivers to gain new abilities
- commands
    - docker container run -p 80:80 --name webhost -d nginx
    - docker container port webhost
    - docker container inspect --format '{{ .NetworkSettings.IPAddress }}' containerName (get the ip address)
    
## CLI Management of of virtual networks
- Concepts
    - Create your apps so front/back sit on the same docker network
    - Their inter-communication never leaves host
    - all externally exposed ports closed by default
    - you must manually expose via -p which is better default security!
- Commands
    - docker network ls
    - docker  network inspect
    - docker network create --driver
    - docker network connect 
    - docker network disconnect

## Docker Networks DNS and how containers find each other
- Concepts
    - Understand how dns is the key to easy inter-container comms
    - see how it works by default with custom networks
    - learn how to use --link to enable DNS on default bridge network
- Commands
    - docker container exec -it cont1 ping cont2 (ping container to find if they see each other)
    
## images
docker history imageName
docker image inspect imageName (gives you back the metadata)

## image tags and how to update them to docker hub
docker image tag --help
docker image push myrepo/image
docker login

## Building images... The Docker file
- Concepts
    - a Dockerfile (-f)
    - **FROM**: all images must have a from
    - **ENV**: for setting environment variables
    - **RUN**: optional commands to run at shell inside container at build time
      (when you change commands with && it stands that all the concatenated set of commands
      will live on the same layer, saves time and space) random note: never log to a file
    - **EXPOSE**: expose these ports on the docker virtual networks
      (you still need to use -p or -P to open/forward these ports on host)
    - **CMD**: required param, the final param that you run everytime you launch a container
    - **COPY**: 
## Running docker builds
- docker image build -t customNginx . (build the dockerfile in this directory)
- each line has a hash... it means that if the hash hasn't  changed docker will not rebuild this line

## Extending official images
- docker container run -p 80:80 --rm nginx (delete when exit)
- docker image build -t nginx-with-html





























        


