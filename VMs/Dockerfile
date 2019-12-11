# This dockerfile builds a container that pulls down and runs the latest version of Benchmark
FROM openjdk:8-jdk
MAINTAINER "Dave Wichers dave.wichers@owasp.org"

RUN apt-get update && apt-get clean \
    && apt-get install -q -y \
    git maven \
    && apt-get autoremove -y \
    && apt-get clean

RUN mkdir /owasp \
    && git clone https://github.com/OWASP/benchmark /owasp/benchmark

RUN cd /owasp/benchmark \
    && mvn clean package cargo:install \
    && useradd -d /home/bench -m -s /bin/bash bench \
    && echo bench:bench | chpasswd \
    && chown -R bench /owasp/

WORKDIR /owasp/benchmark

USER bench

CMD ["mvn", "cargo:run", "-Pdeploy", "-Drunenv=remote"]
