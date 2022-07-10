FROM debian:stable-slim

RUN apt-get update -y && apt-get upgrade -y

RUN apt-get install -y \ 
    gcc \
    cmake \
    build-essential \
    file \
    apt-utils \ 
    libxml2 \
    libxslt-dev \
    curl \
    libvorbis-dev \
    libtheora-dev \
    libcurl4-openssl-dev \ 
    libssl-dev \
    libpthread-stubs0-dev \
    pkg-config \
    && rm -rf /var/lib/apt/lists/* 


RUN curl https://ftp.osuosl.org/pub/xiph/releases/icecast/icecast-2.4.4.tar.gz -o icecast-2.4.4.tar.gz

RUN tar -xvf icecast-2.4.4.tar.gz && mv icecast-2.4.4 icecast
RUN rm icecast-2.4.4.tar.gz

WORKDIR /icecast

RUN ./configure --with-curl --with-openssl
RUN make
RUN make install

RUN mkdir -p /data/icecast/log
RUN mkdir -p /data/icecast/web
RUN mkdir -p /data/icecast/admin

RUN touch /data/icecast/log/error.log
RUN touch /data/icecast/log/access.log
RUN touch /etc/mime.types

COPY icecast_configs/icecast.xml /data/icecast/icecast.xml

RUN useradd -ms /bin/bash icecast_user
RUN chown icecast_user src/icecast

RUN chown -R icecast_user /data/icecast/log /data/icecast/web /data/icecast/admin
RUN chown icecast_user /data/icecast/log/access.log
RUN chown icecast_user /data/icecast/log/error.log

RUN cp -r /icecast/web/* /data/icecast/web
RUN cp -r /icecast/admin/* /data/icecast/admin

USER icecast_user

EXPOSE 8001

ENTRYPOINT ["src/icecast", "-c", "/data/icecast/icecast.xml"]