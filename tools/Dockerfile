# Use the latest Ubuntu image as a parent
FROM ubuntu:focal

ENV DEBIAN_FRONTEND=noninteractive TZ=Australia/Brisbane

# Initial updates and install core utilities
# net-tools = ifconfig, ip
RUN apt-get update -qq -y && \
    apt-get upgrade -y && \
    apt-get install -y \
       wget \
       curl \
       apt-transport-https \
       lsb-release \ 
       ca-certificates \
       gnupg2 \
       software-properties-common \
       locales \
       cron    \
       net-tools \
       vim \
       docker \ 
       docker-compose
RUN dpkg-reconfigure locales

# Install Webmin
RUN wget -q http://www.webmin.com/jcameron-key.asc && \
    apt-key add jcameron-key.asc && \
    sh -c 'echo "deb http://download.webmin.com/download/repository sarge contrib" > /etc/apt/sources.list.d/webmin.list' && \
    apt-get update && \
    apt-get install -y webmin && \
    apt-get clean && \
    echo "$(cat /etc/webmin/webmin.acl) docker" > /etc/webmin/webmin.acl

EXPOSE 10000
ENV LC_ALL C.UTF-8

WORKDIR /home
RUN echo "#! /bin/bash" > entrypoint.sh && \
    echo "sed -i 's;ssl=1;ssl=0;' /etc/webmin/miniserv.conf && systemctl enable cron && service webmin start && tail -f /dev/null" >> entrypoint.sh && \
    chmod 755 entrypoint.sh

CMD /home/entrypoint.sh