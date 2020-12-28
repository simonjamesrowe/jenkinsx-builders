FROM docker:dind

RUN apt

ENV DOCKER_VERSION 17.12.0
ENV EXPOSECONTROLLER_VERSION 2.3.34
ENV GCLOUD_VERSION 270.0.0
ENV HELM_VERSION 2.17.0
ENV HELM3_VERSION 3.0.3
ENV HELMFILE_VERSION 0.98.2
ENV JX_RELEASE_VERSION 1.0.17
ENV SKAFFOLD_VERSION 1.0.1
ENV CONTAINER_STRUCTURE_TEST_VERSION 1.8.0
ENV REFLEX_VERSION 0.3.1
ENV GORELEASER_VERSION 0.110.0
ENV VELERO_VERSION 1.2.0
ENV EKSCTL_VERSION 0.11.1
ENV KUBECTL_VERSION 1.16.0
###
WORKDIR /home/jenkins

ENV PATH=$PATH:/usr/bin/google-cloud-sdk/bin

# Docker
RUN curl -f https://download.docker.com/linux/static/stable/x86_64/docker-$DOCKER_VERSION-ce.tgz | tar xvz && \
  mv docker/docker /usr/bin/ && \
  rm -rf docker && \
# helm
  curl -f https://storage.googleapis.com/kubernetes-helm/helm-v${HELM_VERSION}-linux-amd64.tar.gz  | tar xzv && \
  mv linux-amd64/helm /usr/bin/ && \
  mv linux-amd64/tiller /usr/bin/ && \
  rm -rf linux-amd64 && \
# helm3
  curl -L https://get.helm.sh/helm-v${HELM3_VERSION}-linux-amd64.tar.gz | tar xzv && \
  mv linux-amd64/helm /usr/bin/helm3 && \
  rm -rf linux-amd64 && \
  mkdir /usr/bin/helm3-bin && \
  ln -s /usr/bin/helm3 /usr/bin/helm3-bin/helm && \
# helmfile
  curl -LO https://github.com/roboll/helmfile/releases/download/v${HELMFILE_VERSION}/helmfile_linux_amd64 && \
  mv helmfile_linux_amd64 /usr/bin/helmfile && \
  chmod +x /usr/bin/helmfile && \
# gcloud
  curl -f -L https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-${GCLOUD_VERSION}-linux-x86_64.tar.gz | tar xzv && \
  mv google-cloud-sdk /usr/bin/ && \
# install the docker credential plugin
  gcloud components install docker-credential-gcr && \
# install alpha gcloud components
  gcloud components install alpha -q && \
# jx-release-version
  curl -f -o ./jx-release-version -L https://github.com/jenkins-x/jx-release-version/releases/download/v${JX_RELEASE_VERSION}/jx-release-version-linux && \
  mv jx-release-version /usr/bin/ && \
  chmod +x /usr/bin/jx-release-version && \
# exposecontroller
  curl -f -L https://github.com/fabric8io/exposecontroller/releases/download/v$EXPOSECONTROLLER_VERSION/exposecontroller-linux-amd64 > exposecontroller && \
  chmod +x exposecontroller && \
  mv exposecontroller /usr/bin/ && \
# skaffold
  curl -f -Lo skaffold https://storage.googleapis.com/skaffold/releases/v${SKAFFOLD_VERSION}/skaffold-linux-amd64 && \
  chmod +x skaffold && \
  mv skaffold /usr/bin && \
# container structure test
  curl -LO https://storage.googleapis.com/container-structure-test/v${CONTAINER_STRUCTURE_TEST_VERSION}/container-structure-test-linux-amd64 && \
  chmod +x container-structure-test-linux-amd64 && \
  mv container-structure-test-linux-amd64 /usr/local/bin/container-structure-test && \
# kubectl
  curl -f -LO https://storage.googleapis.com/kubernetes-release/release/v${KUBECTL_VERSION}/bin/linux/amd64/kubectl && \
  chmod +x kubectl && \
  mv kubectl /usr/bin/ && \
# aws ecr docker credential helper.
  mkdir ecr && \
  curl -f -L -o ./ecr/docker-credential-ecr-login https://amazon-ecr-credential-helper-releases.s3.us-east-2.amazonaws.com/0.4.0/linux-amd64/docker-credential-ecr-login && \
  chmod +x ecr/docker-credential-ecr-login && \
  mv ecr/docker-credential-ecr-login /usr/bin/ && \
  rm -rf ecr && \
# ACR docker credential helper
#??https://github.com/Azure/acr-docker-credential-helper
  mkdir acr && \
  curl -f -L https://aadacr.blob.core.windows.net/acr-docker-credential-helper/docker-credential-acr-linux-amd64.tar.gz | tar -xzv -C ./acr/ && \
  mv acr/docker-credential-acr-linux /usr/bin/ && \
  rm -rf acr && \
# reflex
  curl -f -L https://github.com/ccojocar/reflex/releases/download/v${REFLEX_VERSION}/reflex_${REFLEX_VERSION}_linux_amd64.tar.gz | tar xzv && \
  mv reflex /usr/bin/ && \
# goreleaser
  mkdir goreleaser && \
  curl -Lf https://github.com/goreleaser/goreleaser/releases/download/v${GORELEASER_VERSION}/goreleaser_Linux_x86_64.tar.gz | tar -xzv -C ./goreleaser/ && \
  mv goreleaser/goreleaser /usr/bin/ && \
  rm -rf goreleaser && \
# velero
  curl -f -L https://github.com/heptio/velero/releases/download/v${VELERO_VERSION}/velero-v${VELERO_VERSION}-linux-amd64.tar.gz | tar xzv && \
  mv velero-v${VELERO_VERSION}-linux-amd64/velero /usr/bin/ && \
  rm -rf velero-v${VELERO_VERSION}-linux-amd64 && \
# eksctl
  curl --silent --location "https://github.com/weaveworks/eksctl/releases/download/${EKSCTL_VERSION}/eksctl_Linux_amd64.tar.gz" | tar xz -C /tmp && \
  mv /tmp/eksctl /usr/local/bin

ENV PATH /home/jenkins/.jx/bin:${PATH}:/opt/google/chrome

CMD ["helm","version"]
###


CMD ["gradle"]

ENV ANDROID_VERSION 4333796
ENV ANDROID_HOME /opt/android-sdk-linux
ENV PATH ${PATH}:${ANDROID_HOME}/tools:${ANDROID_HOME}/tools/bin:${ANDROID_HOME}/platform-tools

RUN yum install -y java-11-openjdk-devel

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
ENV JX_VERSION 2.1.153
RUN curl -f -L https://github.com/jenkins-x/jx/releases/download/v${JX_VERSION}/jx-linux-amd64.tar.gz | tar xzv && \
  mv jx /usr/bin/
