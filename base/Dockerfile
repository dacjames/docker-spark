FROM debian:jessie

RUN apt-get update -y && \
    apt-get dist-upgrade -y

RUN apt-get install -y \
        git python curl

COPY webupd8team-java.list /etc/apt/sources.list.d/webupd8team-java.list

RUN echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && \
    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys EEA14886 && \
    apt-get update -y && \
    apt-get install -y oracle-java8-installer && \
    rm -rf /var/cache/oracle-jdk8-installer

# See: http://spark.apache.org/docs/latest/building-spark.html
RUN mkdir /opt/spark && \
    mkdir -p /tmp/spark/recovery && \
    git clone git://github.com/apache/spark.git -b branch-1.4 /opt/spark

# Disables the zinc incremental comilation,
# since it freezes and hangs the build for unkown reasons
ENV ZINC_INSTALL_FLAG false
RUN cd /opt/spark && \
    ./build/mvn -Phadoop-2.6 -Dhadoop.version=2.6.0 -DskipTests clean package && \
    pgrep java | xargs kill
