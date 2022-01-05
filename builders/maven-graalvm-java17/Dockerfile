FROM ubuntu:impish

# Defining default Java and Maven version
ARG JAVA_VERSION="21.2.0.r16-grl"
ARG MAVEN_VERSION="3.8.4"

# Installing basic packages
RUN apt-get update &&  apt-get install -y unzip curl zip dirmngr apt-transport-https lsb-release ca-certificates

RUN curl -sL https://deb.nodesource.com/setup_14.x | bash -
RUN apt-get install -y nodejs
RUN npm install --global yarn

# Downloading SDKMAN!
RUN curl -s "https://get.sdkman.io" | bash

## Installing Java and Maven, removing some unnecessary SDKMAN files
RUN bash -c "source $HOME/.sdkman/bin/sdkman-init.sh && \
    yes | sdk install java $JAVA_VERSION && \
    yes | sdk install maven $MAVEN_VERSION && \
    rm -rf $HOME/.sdkman/archives/* && \
    rm -rf $HOME/.sdkman/tmp/*"

RUN bash -c "source $HOME/.sdkman/bin/sdkman-init.sh && \
    gu install native-image"

ENV PATH="/root/.sdkman/candidates/java/current/bin:/root/.sdkman/candidates/maven/current/bin:${PATH}"

