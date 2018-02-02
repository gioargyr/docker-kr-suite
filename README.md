# KR-SUITE
This KR-suite docker packs all the linked data tools developed by the [KRR&A team](http://kr.di.uoa.gr/) of the [National and Kapodistrian Univeristy of Athens](http://www.di.uoa.gr/). The KR-suite contains the following open source tools:
* __[Strabon](http://strabon.di.uoa.gr/).__ Strabon is a spatiotemporal RDF store. You can use it to store linked geospatial data that changes over time and pose queries using two popular extensions of SPARQL (GeoSPARQL and stSPARQL). Strabon has been shown experimentally to be the most efficient spatiotemporal RDF store available today.
* __[GeoTriples](http://geotriples.di.uoa.gr/).__ GeoTriples is a tool for transforming geospatial data from their original formats (e.g., shapefiles or spatially-enabled relational databases) into RDF.
* __[Sextant](http://sextant.di.uoa.gr/).__ Sextant is a web based and mobile ready platform for visualizing, exploring and interacting with linked geospatial data. The core feature of Sextant is the ability to create thematic maps by combining geospatial and temporal information that exists in a number of heterogeneous data sources.

You need to install docker engine for following any of the instructions below (https://www.docker.com/get-docker).

## Build docker-image from source (Dockerfile)
* Clone https://github.com/gioargyr/docker-kr-suite
* Go to /<file_path_to>/kr-suite and build (terminal of your machine): [sudo] docker build -t __docker-image-name__ .

## OR
alternatively (suggested):
* follow the instructions below and use as docker __docker-image-name__ the "gioargyr/kr-suite:1.0.0" (already built image).

## Create docker-container from docker-image:
* A docker-image named __docker-image-name__ is available either locally (previously built), or in DockerHub.
* Use the docker-image (terminal of your machine): $ [sudo] docker run --name __docker-container-name__ -p 9999:8080 -v /<some_dir>:/inout __docker-image-name__
* Now the docker-container __docker-container-name__  should be running.

## Using the docker-container:
* Use (the command line of) the container (terminal of your machine): $ [sudo] docker exec -it __docker-container-name__  /bin/bash
* Starting tomcat (terminal of container): $ rocket.sh

**Attention**  When the container is up and running, it provides only a postgres(+postgis) DBMS. In order to make Strabon and Sextant available,
you should start container's tomcat as described in the previous instruction.

Now all the WEB Services tools (Sextant, Strabon, OnTop Spatial) are available in your machine's port 9999.

* Going through the logs (terminal of your machine): $ [sudo] docker logs <docker-container-name>
* You can make local files available in container's dir "/inout" by copying them in you machine's dir: /<some_dir>

## General docker tips:
* Listing all docker containers (terminal of your machine): [sudo] docker ps -a 
* Listing docker images (terminal of your machine): [sudo] docker images

For more detailed information on docker, visit the [official Docker Documentation](https://docs.docker.com/).

## Sextant Configuration
Sextant is one of the tools that come with the KR-suite. To change the default configurations you can alter the server-configuration.properties file that is located under /Sextant/WEB-INF/classes/server/
There you can change the default SPARQL endpoint that is used to store and load maps, set PROXY server configuration and use your Bing Maps key to get access to the Bing Maps base tiles. For more detailed information you can view the embedded Manual Pages by pressing the "?" button in the top-right corner of the interface.

## GeoTriples Usage
* Go to the /bin directory of GeoTriples: cd <path_to_geotriples-1.1.6-bin>/bin
* Run geotriples-cmd in order to see all available options and examples: ./geotriples-cmd