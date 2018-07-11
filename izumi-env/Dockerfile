ARG V_UBUNTU=18.04
FROM ubuntu:${V_UBUNTU}

ARG V_SCALA=2.12.6
ARG V_TYPESCRIPT=2.9.2
ARG V_GOLANG=1.10

RUN mkdir /work
RUN mkdir /root
VOLUME /work
VOLUME /root
WORKDIR /work

RUN echo "APT::Acquire::Queue-Mode "access"; APT::Acquire::Retries 3; " > /etc/apt/apt.conf.d/99parallel && \
    export DEBIAN_FRONTEND=noninteractive && \
    apt-get update && apt-get install -y tzdata gnupg ca-certificates wget curl unzip && \
    ln -fs /usr/share/zoneinfo/Etc/UTC /etc/localtime && \
    dpkg-reconfigure --frontend noninteractive tzdata && \
    apt-get clean -y

RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF && \
    echo "deb https://download.mono-project.com/repo/ubuntu stable-bionic main" | tee /etc/apt/sources.list.d/mono-official-stable.list && \
    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 2EE0EA64E40A89B84B2DF73499E82A75642AC823 && \
    echo "deb https://dl.bintray.com/sbt/debian /" | tee /etc/apt/sources.list.d/sbt.list

RUN apt-get update && \
    apt-get install -y git openjdk-8-jdk sbt mono-devel nuget nunit-console npm golang-${V_GOLANG} && \
    apt-get clean -y

RUN wget -q www.scala-lang.org/files/archive/scala-${V_SCALA}.deb && dpkg -i scala-${V_SCALA}.deb && rm -rf *.deb && \
    npm i -g typescript@${V_TYPESCRIPT} && \
    wget -q -O /usr/local/bin/coursier https://git.io/vgvpD && chmod +x /usr/local/bin/coursier

#  /usr/lib/go-1.10/bin needs to be added into PATH manually: https://github.com/golang/go/wiki/Ubuntu
ENV PATH $PATH:/usr/lib/go-${V_GOLANG}/bin

RUN java -version && \
    scalac -version && \
    csc -version && \
    tsc -version && \
    go version && \
    (sbt -help || true)
