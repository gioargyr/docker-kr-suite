############################
# Dockerfile for RK-suite  #
# RK-team                  #
# Madgik                   #
# DI @ UoA                 #
#                          #
# java7                    #
# maven 3.0.5              #
# git 1.9.1                #
# tomcat 8.0.23            #
# postgres 9.4             #
# postgis 2.3              #
#                          #
# Sextant                  #
# Strabon                  #
# Ontop-spatial            #
# GeoTriples               #
# madIS                    #
############################

FROM ubuntu:14.04
MAINTAINER Giorgos Argyriou <gioargyr@gmail.com>


ENV PORT 8080
ENV TOMCAT_MAJOR_VERSION 8
ENV TOMCAT_MINOR_VERSION 8.0.23


# INSTALL PREREQUISITIES
RUN apt-get update \
 && apt-get install -y \
    wget \
    default-jdk \
    python-apsw \
    curl \
    git \
    mercurial \
    maven \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*


# DOWNLOAD AND INSTALL postgres AND postgis
RUN touch /etc/apt/sources.list.d/pgdg.list \
 && echo "deb http://apt.postgresql.org/pub/repos/apt/ trusty-pgdg main" >> /etc/apt/sources.list.d/pgdg.list \
 && wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -

RUN apt-get update \
 && apt-get install -y \
    postgresql-9.4 \
    postgresql-server-dev-9.4 \
    postgresql-9.4-postgis-2.3 \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

ADD initdb.sh enable_remote.sh /usr/local/bin/

RUN chmod -v +x /usr/local/bin/initdb.sh
RUN chown -v postgres:postgres /usr/local/bin/initdb.sh
RUN chmod -v +x /usr/local/bin/enable_remote.sh

USER postgres

## start postgres, add postgis, create template for strabon and stop postgres
RUN initdb.sh

USER root


# INSTALL TOMCAT
RUN wget -q https://archive.apache.org/dist/tomcat/tomcat-${TOMCAT_MAJOR_VERSION}/v${TOMCAT_MINOR_VERSION}/bin/apache-tomcat-${TOMCAT_MINOR_VERSION}.tar.gz \
 && wget -qO- https://archive.apache.org/dist/tomcat/tomcat-${TOMCAT_MAJOR_VERSION}/v${TOMCAT_MINOR_VERSION}/bin/apache-tomcat-${TOMCAT_MINOR_VERSION}.tar.gz.md5 | md5sum -c - \
 && tar zxf apache-tomcat-*.tar.gz \
 && rm apache-tomcat-*.tar.gz \
 && mv apache-tomcat* tomcat


# DOWNLOAD geotriples
RUN wget http://geotriples.di.uoa.gr/downloads/geotriples-1.1.6-bin.zip \
 && unzip geotriples-1.1.6-bin.zip \
 && rm -rf /geotriples-1.1.6-bin.zip


# DOWNLOAD madIS
RUN git clone https://github.com/madgik/madis.git


# DOWNLOAD AND BUILD strabon
RUN hg clone http://hg.strabon.di.uoa.gr/Strabon/ \
 && cd Strabon \
 && mvn clean package

RUN cp /Strabon/endpoint/target/strabon-endpoint-3.3.2-SNAPSHOT.war /tomcat/webapps/Strabon.war \
 && rm -rf /Strabon


# DOWNLOAD AND BUILD ontop-spatial
RUN git clone https://github.com/ConstantB/Ontop-spatial-example.git \
 && cp /Ontop-spatial-example/ontop-spatial-precompiled/webapp/QuestSesame/openrdf-* /tomcat/webapps \
 && rm -rf /Ontop-spatial-example


# DOWNLOAD AND BUILD sextant
RUN hg clone http://hg.strabon.di.uoa.gr/Sextant-New -r OL3 \
 && cd Sextant-New/JerseyServer \
 && mvn clean package

RUN cp /Sextant-New/JerseyServer/target/*.war /tomcat/webapps/Sextant_v2.0.war \
 && rm -Rf /Sextant-New


# FINAL CONFIGURATIONS
RUN mkdir /inout

ADD rocket.sh /usr/local/bin/

RUN chmod -v +x /usr/local/bin/rocket.sh

EXPOSE $PORT

CMD enable_remote.sh && su - postgres -c "/usr/lib/postgresql/9.4/bin/postgres -D /var/lib/postgresql/9.4/main -c config_file=/etc/postgresql/9.4/main/postgresql.conf"
