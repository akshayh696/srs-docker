ARG SRS_VERSION=3.0.42

FROM debian:jessie AS build
#https://github.com/akshayh696/srs-stream/archive/1.1.tar.gz
ARG SRS_VERSION
ENV SRS_VERSION=${SRS_VERSION}
ENV SRS_COMMIT=1.6
ENV SRS_CONFIGURE_ARGS=

RUN set -ex; \
    apt-get update; \
    apt-get install -y --no-install-recommends wget ca-certificates sudo python net-tools libfile-spec-native-perl; \
    cd /tmp; \
    #wget https://github.com/ossrs/srs/archive/${SRS_COMMIT}.tar.gz; \
    wget https://github.com/akshayh696/srs-stream/archive/${SRS_COMMIT}.tar.gz; \
    tar zxf ${SRS_COMMIT}.tar.gz; \
    cd srs-stream-${SRS_COMMIT}/trunk; \
#   cd srs-${SRS_COMMIT}/trunk; \
    ./configure --prefix=/srs ${SRS_CONFIGURE_ARGS}; \
    make; \
    make install; \
    rm -rf /tmp/*; \
    rm -rf /var/lib/apt/lists/*;
COPY ./srs.conf /srs/conf/docker.conf
COPY ./srs-cust.conf /srs/conf/srs-cust.conf


FROM debian:jessie-slim AS dist
ARG SRS_VERSION
ENV SRS_VERSION=${SRS_VERSION}
EXPOSE 1935 1985 8080
COPY --from=build /srs /srs
WORKDIR /srs
CMD ["./objs/srs", "-c", "./conf/docker.conf"]
