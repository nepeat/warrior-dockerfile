## A Dockerfile for the [ArchiveTeam Warrior](https://www.archiveteam.org/index.php?title=ArchiveTeam_Warrior)

<img alt="Warrior logo" src="https://www.archiveteam.org/images/f/f3/Archive_team.png" height="100px"><img alt="Docker logo" src="https://upload.wikimedia.org/wikipedia/commons/7/79/Docker_%28container_engine%29_logo.png" height="100px">

Build, run, grab the container IP and access the web interface on port 8001.

Available as a Trusted Build on the index as [`archiveteam/warrior-dockerfile`](https://index.docker.io/u/archiveteam/warrior-dockerfile/) so you can just

```
docker pull archiveteam/warrior-dockerfile
# run without -d to follow the warrior install process
# you will need to detach or stop-and-start the container.
# use -p to bind port 8001 on the docker container 
# (default ip 172.17.0.x) to port 8001 on localhost.
docker run [-d] [-p 8001:8001] archiveteam/warrior-dockerfile
```

If you prefer to just run the process in the background, and automatically start it again after machine reboot, use this instead:

``` shell-interaction
docker run --detach \
  --publish 8001:8001 \
  --restart always \
  archiveteam/warrior-dockerfile
```

To access the web interface get the container IP from `docker inspect` and point your browser to `http://IP:8001`. If you are running this container on a headless machine, be sure to bind the docker container's port to a port on that machine (e.g. `-p 8001:8001`) so that you can access the web interface on your LAN.

You can stop and resume the Warrior with `docker stop` and `docker start`


Raspberry Pi:
You can build the container with the following command:
``` shell-interaction
docker build --rm -t warrior-arm32v5:latest -f Dockerfile.raspberry .
```

The image needs a place to store the downloaded data as well as its
configuration.  Say you have a location suitable at /var/local/warrior
use the command below, otherwise update the data and config.json paths.

First, create an empty config.json if it doesn't exist.  Otherwise when you
mount the path with docker it will create it as a directory.
``` shell-interaction
touch /var/local/warrior/config.json
```

Now start the container.
``` shell-interaction
docker run \
	--volume /var/local/warrior/data:/data/data \
	--volume /var/local/warrior/config.json:/home/warrior/projects/config.json \
	--publish 8001:8001 \
	--restart always \
	warrior-arm32v5:latest
```
