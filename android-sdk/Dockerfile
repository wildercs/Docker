# basado en https://registry.hub.docker.com/u/samtstern/android-sdk/dockerfile/ with openjdk-8
FROM java:8

MAINTAINER Wilder Campos <wcamposs@everis.com>

ENV DEBIAN_FRONTEND noninteractive

ENV ANDROID_SDK_VERSION r24.4.1
ENV ANDROID_BUILD_TOOLS_VERSION 23.0.2

# Instalar dependencias
RUN dpkg --add-architecture i386 && \
    apt-get update && \
    apt-get install -yq libstdc++6:i386 zlib1g:i386 libncurses5:i386 ant maven --no-install-recommends && \
    apt-get clean
ENV GRADLE_URL http://services.gradle.org/distributions/gradle-2.2.1-all.zip
RUN curl -L ${GRADLE_URL} -o /tmp/gradle-2.2.1-all.zip && unzip /tmp/gradle-2.2.1-all.zip -d /opt/ && rm /tmp/gradle-2.2.1-all.zip
ENV GRADLE_HOME /opt/gradle-2.2.1

# Descargar y descomprimir SDK
ENV ANDROID_SDK_FILENAME android-sdk_${ANDROID_SDK_VERSION}-linux.tgz
ENV ANDROID_SDK_URL http://dl.google.com/android/${ANDROID_SDK_FILENAME}
ENV ANDROID_API_LEVELS android-23,android-24
RUN curl -L "${ANDROID_SDK_URL}" | tar --no-same-owner -xz -C /opt/
ENV ANDROID_HOME /opt/android-sdk-linux
ENV ANDROID_SDK /opt/android-sdk-linux
ENV PATH ${ANDROID_HOME}/tools:$ANDROID_HOME/platform-tools:$PATH
#VOLUME /opt/

# Instalar componentes Android SDK
RUN echo y | android update sdk --no-ui --all --filter "platform-tools,${ANDROID_API_LEVELS},build-tools-${ANDROID_BUILD_TOOLS_VERSION}"
RUN echo y | android update sdk --no-ui --all --filter "extra-android-m2repository,extra-android-support,extra-google-google_play_services,extra-google-m2repository"

# Soporte  Gradle
ENV TERM dumb
ENV JAVA_OPTS -Xms256m -Xmx512m

#docker run -t -v $(pwd)/app:/project/app your-android-project-build:latest ./gradlew clean assembleRelease
#/usr/bin/mvn