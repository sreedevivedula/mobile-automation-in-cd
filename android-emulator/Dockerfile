# ====================================================================== #
# Android SDK Docker Image
# ====================================================================== #

# Base image
# ---------------------------------------------------------------------- #
FROM ubuntu:18.04

# Author
# ---------------------------------------------------------------------- #
LABEL maintainer "sreedevi@thoughtworks.com"

# support multiarch: i386 architecture
# install Java
# install essential tools
# install Qt
RUN dpkg --add-architecture i386 && \
    apt-get update -y && \
    apt-get install -y --no-install-recommends libncurses5:i386 libc6:i386 libstdc++6:i386 lib32gcc1 lib32ncurses5 lib32z1 zlib1g:i386 && \
    apt-get install -y --no-install-recommends openjdk-8-jdk && \
    apt-get install -y --no-install-recommends git wget unzip && \
    apt-get install -y --no-install-recommends qt5-default

# download and install Android SDK
# https://developer.android.com/studio/#downloads
ARG ANDROID_SDK_VERSION=4333796
ENV ANDROID_HOME /opt/android-sdk
RUN mkdir -p ${ANDROID_HOME} && cd ${ANDROID_HOME} && \
    wget -q https://dl.google.com/android/repository/sdk-tools-linux-${ANDROID_SDK_VERSION}.zip && \
    unzip *tools*linux*.zip && \
    rm *tools*linux*.zip

# set the environment variables
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64
ENV PATH ${PATH}:${ANDROID_HOME}/emulator:${ANDROID_HOME}/tools:${ANDROID_HOME}/platform-tools:${ANDROID_HOME}/tools/bin
ENV _JAVA_OPTIONS -XX:+UnlockExperimentalVMOptions -XX:+UseCGroupMemoryLimitForHeap
# WORKAROUND: for issue https://issuetracker.google.com/issues/37137213
ENV LD_LIBRARY_PATH ${ANDROID_HOME}/emulator/lib64:${ANDROID_HOME}/emulator/lib64/qt/lib

# accept the license agreements of the SDK components
ADD license_accepter.sh /opt/
RUN chmod +x /opt/license_accepter.sh && /opt/license_accepter.sh $ANDROID_HOME

# install and configure VNC server
ENV USER root
ENV DISPLAY :1
EXPOSE 5901
ADD vncpass.sh /tmp/
ADD watchdog.sh /usr/local/bin/
ADD supervisord_vncserver.conf /etc/supervisor/conf.d/
RUN apt-get update -y && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends xfce4 xfce4-goodies xfonts-base dbus-x11 tightvncserver expect && \
    apt-get install -y --no-install-recommends  supervisor && \
    mkdir -p /var/log/supervisord && \
    chmod +x /tmp/vncpass.sh; sync && \
    /tmp/vncpass.sh && \
    rm /tmp/vncpass.sh && \
    apt-get remove -y expect && apt-get autoremove -y

# Install appium
RUN apt-get update && \
    apt-get install -y curl && \
    curl -sL https://deb.nodesource.com/setup_12.x | bash - && \
    apt-get install -y nodejs && \
    npm install -g appium --unsafe-perm=true --allow-root

# Add Appium Node Configs
ADD appium-node-configs /appium-node-configs

# Install Android SDK Components and Emulator System Image
ARG ANDROID_API_VERSION=android-28
ARG BUILD_TOOLS_VERSION=28.0.1
ARG EMULATOR_IMAGE_PATH="system-images;${ANDROID_API_VERSION};google_apis;x86"
ARG VIRTUAL_DEVICE_ID=19
RUN sdkmanager "platform-tools" "platforms;${ANDROID_API_VERSION}" "emulator" "build-tools;${BUILD_TOOLS_VERSION}" && \
    yes | sdkmanager "${EMULATOR_IMAGE_PATH}" && \
    echo "no" | avdmanager create avd -n test-avd -k ${EMULATOR_IMAGE_PATH} -d ${VIRTUAL_DEVICE_ID} && \
    echo "hw.ramSize=1024" >> /root/.android/avd/test-avd.avd/config.ini

# setup adb server
EXPOSE 5037

ADD supervisord.conf /etc/supervisor/conf.d/
CMD ["/usr/bin/supervisord"]