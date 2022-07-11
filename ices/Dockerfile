FROM debian:stable-slim

RUN apt-get update -y && apt-get upgrade -y

RUN apt-get install -y \
    ices2 

RUN mkdir -p /data/ices/logs
RUN touch /data/ices/logs/ices.log

ADD ices_configs /data/ices/ices_configs

ADD music /data/ices/music

RUN useradd -ms /bin/bash ices_user
RUN chown -R ices_user /data/ices/logs /data/ices/ices_configs /data/ices/music
USER ices_user

ENTRYPOINT ["ices2", "/data/ices/ices_configs/ices.xml"]