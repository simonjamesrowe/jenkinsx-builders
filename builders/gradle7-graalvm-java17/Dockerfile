FROM ubuntu:impish

# Defining default Java and Maven version
ARG JAVA_VERSION="21.3.0.r17-grl"
ARG GRADLE_VERSION="7.3.3"

# Installing basic packages
RUN apt-get update &&  apt-get install -y unzip curl zip

# Downloading SDKMAN!
RUN curl -s "https://get.sdkman.io" | bash

## Installing Java and Maven, removing some unnecessary SDKMAN files
RUN bash -c "source $HOME/.sdkman/bin/sdkman-init.sh && \
    yes | sdk install java $JAVA_VERSION && \
    yes | sdk install gradle $GRADLE_VERSION && \
    rm -rf $HOME/.sdkman/archives/* && \
    rm -rf $HOME/.sdkman/tmp/*"

RUN bash -c "source $HOME/.sdkman/bin/sdkman-init.sh && \
    gu install native-image"