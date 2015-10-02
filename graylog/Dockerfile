# Dockerfile for Graylog
# File adapted from 
# <https://github.com/Graylog2/graylog2-images/blob/master/docker/Dockerfile>

FROM phusion/baseimage:0.9.16
MAINTAINER MOE-ESTL
ENV REFRESHED_AT 2015-09-29

ENV DEBIAN_FRONTEND noninteractive

ENV JAVA_HOME /opt/graylog/embedded/jre

RUN apt-get update && \
    apt-get install -y curl ntp ntpdate tzdata && \
    curl -O -L https://packages.graylog2.org/releases/graylog2-omnibus/ubuntu/graylog_latest.deb && \
    dpkg -i graylog_latest.deb && \
    rm graylog_latest.deb && \
    sed -i "0,/^\s*$/s//\/opt\/graylog\/embedded\/share\/docker\/run_graylogctl\n/" /etc/rc.local && \
    sed -i "0,/^\s*$/s//tail\ \-F\ \/var\/log\/graylog\/server\/current\ \&\n/" /etc/rc.local && \
    apt-get clean && \
    rm -rf /tmp/* /var/tmp/*

VOLUME /var/opt/graylog/data
VOLUME /var/log/graylog
VOLUME /opt/graylog/plugin
VOLUME /opt/graylog/conf/nginx/ca

# Message forwarding for Logstash
EXPOSE 12201/UDP

# Web Interface
EXPOSE 9000 443

CMD ["/opt/graylog/embedded/share/docker/my_init"]