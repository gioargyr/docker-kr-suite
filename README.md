# docker-kr-suite
This is a docker holding all National and Kapodistrian UoA's KRR&A team's tools.

You need to install docker engine for following any of the instructions below.

# Build docker-image from source (Dockerfile)
* Clone https://github.com/gioargyr/docker-kr-suite
* Go to /<file_path_to>/kr-suite and build (terminal of your machine): [sudo] docker build -t <docker-image-name> .

# OR
alternatively (suggested):
* follow the instructions below and use as docker <docker-image-name> the "gioargyr/kr-suite:1.0.0" (already built image).

# Create docker-container from docker-image:
* A docker-image named <docker-image-name> is available either locally (previously built), or in DockerHub.
* Use the docker-image (terminal of your machine): $ [sudo] docker run --name <docker-container-name> -p 9999:8080 -v /<some_dir>:/inout <docker-image-name>
* Now the docker-container <docker-container-name> should be running.

# Using the docker-container:
* Use (the command line of) the container (terminal of your machine): $ [sudo] docker exec -it <docker-container-name> /bin/bash
* Starting tomcat (terminal of container): $ rocket.sh

Now all the WEB Services tools (Sextant, Strabon, OnTop Spatial) are available in your machine's port 9999.

* Going through the logs (terminal of your machine): $ [sudo] docker logs <docker-container-name>
* You can make local files available in container's dir "/inout" by copying them in you machine's dir: /<some_dir>

# General docker tips:
* Listing all docker containers (terminal of your machine): [sudo] docker ps -a 
* Listing docker images (terminal of your machine): [sudo] docker images
* See more in https://www.docker.com/
