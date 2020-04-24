## Choose ubuntu 18.04 LTS version as the base image

FROM ubuntu:18.04
MAINTAINER Akhil <akhilu011@gmail.com>

## Locale settings
RUN echo """ \
\nLANGUAGE=\"en_US.UTF-8\" \
\nLANG=\"en_US.UTF-8\" \
\nLC_ALL=\"en_US.UTF-8\" \
""" >> /etc/environment

## Set basic environment variables
ENV ES_VERSION 7.6.2
ENV PATH /opt/elasticsearch/bin:$PATH
ENV ES_USER "elasticsearch"
ENV ES_GROUP "elasticsearch"
ENV ES_HOME "/opt/elasticsearch"
ENV JAVA_HOME "/usr/lib/jvm/java-11-openjdk-amd64" 

## Install dependant libraries
RUN apt-get update && apt-get install wget build-essential gcc make -y \
    && apt-get install software-properties-common  -y

## Install openjdk-8-jdk 
RUN apt-get install openjdk-11-jre-headless -y

## Install elasticsearch 
RUN cd /opt \
    && wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-${ES_VERSION}-linux-x86_64.tar.gz \
    && wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-${ES_VERSION}-linux-x86_64.tar.gz.sha512 \
    && shasum -a 512 -c elasticsearch-${ES_VERSION}-linux-x86_64.tar.gz.sha512 \
    && tar -xzf elasticsearch-${ES_VERSION}-linux-x86_64.tar.gz \
    && rm -rf elasticsearch-${ES_VERSION}*.tar.gz* \
    && ln -s /opt/elasticsearch-${ES_VERSION} /opt/elasticsearch

## User creation
RUN useradd -d $ES_HOME -M -s /bin/false -U $ES_USER \
    && chown -R ${ES_USER}:${ES_GROUP} /opt/elasticsearch*

## Change User
USER elasticsearch

## Default working directory
WORKDIR /opt/elasticsearch

## Start command
CMD [ "./bin/elasticsearch" ]