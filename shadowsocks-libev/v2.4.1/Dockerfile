FROM ubuntu:trusty

MAINTAINER Sah Lee <contact@leesah.name>

ENV DEPENDENCIES git-core build-essential autoconf libtool libssl-dev
ENV BASEDIR /tmp/shadowsocks-libev
ENV PORT 8338
ENV VERSION v2.4.1

# Set up building environment
RUN apt-get update \
 && apt-get install -y $DEPENDENCIES

# Get the latest code, build and install
RUN git clone https://github.com/shadowsocks/shadowsocks-libev.git $BASEDIR
WORKDIR $BASEDIR
RUN git checkout $VERSION \
 && ./configure \
 && make \
 && make install

# Tear down building environment and delete git repository
WORKDIR /
RUN rm -rf $BASEDIR/shadowsocks-libev\
 && apt-get --purge autoremove -y $DEPENDENCIES

# Port in the json config file won't take affect. Instead we'll use 8388.
EXPOSE $PORT

# Override the host and port in the config file.
ADD entrypoint /
ENTRYPOINT ["/entrypoint"]
CMD ["-h"]
