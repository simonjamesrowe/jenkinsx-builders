FROM gcr.io/jenkinsxio/builder-base:0.0.86

CMD ["gradle"]

ENV ANDROID_VERSION 4333796
ENV ANDROID_HOME /opt/android-sdk-linux
ENV PATH ${PATH}:${ANDROID_HOME}/tools:${ANDROID_HOME}/tools/bin:${ANDROID_HOME}/platform-tools

ARG GRAAL_VERSION=21.0.0

ENV GRAAL_CE_URL=https://github.com/graalvm/graalvm-ce-builds/releases/download/vm-${GRAAL_VERSION}/graalvm-ce-java11-linux-amd64-${GRAAL_VERSION}.tar.gz
ARG INSTALL_PKGS="gzip"

ENV GRAALVM_HOME /opt/graalvm
ENV JAVA_HOME /opt/graalvm

RUN yum install -y ${INSTALL_PKGS} && \
     ### Installation of GraalVM
	 mkdir -p ${GRAALVM_HOME} && \
	 cd ${GRAALVM_HOME} && \
	 curl -fsSL ${GRAAL_CE_URL} | tar -xzC ${GRAALVM_HOME} --strip-components=1  && \
     ### Cleanup
     yum clean all && \
     rm -f /tmp/graalvm-ce-amd64.tar.gz && \
     rm -rf /var/cache/yum
     ###

ENV PATH $GRAALVM_HOME/bin:$PATH
RUN gu install native-image

ENV GRADLE_HOME /opt/gradle
ENV GRADLE_VERSION 6.5.1

RUN set -o errexit -o nounset \
    && echo "Set URLS" \
    && GRADLE_URL="https://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip" \
    && SHA256_URL="${GRADLE_URL}.sha256" \
    \
    && echo "Determine sha256sum" \
    && GRADLE_DOWNLOAD_SHA256=$(curl -sLf "${SHA256_URL}") \
    \
	&& echo "Downloading Gradle" \
	&& wget -q -O gradle.zip "${GRADLE_URL}" \
	\
	&& echo "Checking download hash" \
	&& echo "${GRADLE_DOWNLOAD_SHA256} *gradle.zip" | sha256sum -c - \
	\
	&& echo "Installing Gradle" \
	&& unzip -q gradle.zip \
	&& rm gradle.zip \
	&& mkdir -p /opt \
	&& mv "gradle-${GRADLE_VERSION}" "${GRADLE_HOME}/" \
	&& ln -s "${GRADLE_HOME}/bin/gradle" /usr/bin/gradle

RUN gradle --version

# jx
ENV JX_VERSION 2.1.155
RUN curl -f -L https://github.com/jenkins-x/jx/releases/download/v${JX_VERSION}/jx-linux-amd64.tar.gz | tar xzv && \
  mv jx /usr/bin/
