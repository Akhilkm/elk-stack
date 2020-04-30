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
ENV KIBANA_VERSION 7.6.2
ENV PATH /opt/kibana/bin:$PATH
ENV KIBANA_USER "kibana"
ENV KIBANA_GROUP "kibana"
ENV KIBANA_HOME "/opt/kibana"

## Install dependant libraries
RUN apt-get update && apt-get install curl build-essential gcc make -y \
    && apt-get install software-properties-common -y

## Install Kibana 
RUN cd /opt \
    && curl -O https://artifacts.elastic.co/downloads/kibana/kibana-${KIBANA_VERSION}-linux-x86_64.tar.gz \
    && curl https://artifacts.elastic.co/downloads/kibana/kibana-${KIBANA_VERSION}-linux-x86_64.tar.gz.sha512 | shasum -a 512 -c -  \
    && tar -xzf kibana-${KIBANA_VERSION}-linux-x86_64.tar.gz && mv kibana-${KIBANA_VERSION}-linux-x86_64 kibana-${KIBANA_VERSION} \
    && rm -rf kibana-${KIBANA_VERSION}*.tar.gz* \
    && ln -s kibana-${KIBANA_VERSION} /opt/kibana

## User creation
RUN useradd -d $KIBANA_HOME -M -s /bin/false -U $KIBANA_USER \
    && chown -R ${KIBANA_USER}:${KIBANA_GROUP} /opt/kibana*

## Change User
USER ${KIBANA_USER}

## Copy configuration
COPY config $KIBANA_HOME/config
COPY run.sh ${KIBANA_HOME}/

## Default working directory
WORKDIR $KIBANA_HOME

## Start command
CMD [ "bash", "-c", "${KIBANA_HOME}/run.sh" ]