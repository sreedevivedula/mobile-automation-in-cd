FROM thyrlian/android-sdk-vnc:latest

MAINTAINER sreedevi@thoughtworks.com

ENV DEBIAN_FRONTEND noninteractive

# Install appium
RUN apt-get update && \
    apt-get install -y curl && \
    curl -sL https://deb.nodesource.com/setup_12.x | bash - && \
    apt-get install -y nodejs && \
    npm install -g appium --unsafe-perm=true --allow-root

# Install Android SDK Components and Emulator System Image
ARG ANDROID_API_VERSION=android-28
ARG BUILD_TOOLS_VERSION=28.0.1
ARG EMULATOR_IMAGE_PATH="system-images;${ANDROID_API_VERSION};google_apis;x86"
ARG VIRTUAL_DEVICE_ID=19
RUN sdkmanager "platform-tools" "platforms;${ANDROID_API_VERSION}" "emulator" "build-tools;${BUILD_TOOLS_VERSION}" && \
    yes | sdkmanager "${EMULATOR_IMAGE_PATH}" && \
    echo "no" | avdmanager create avd -n test-avd -k ${EMULATOR_IMAGE_PATH} -d ${VIRTUAL_DEVICE_ID} && \
    echo "hw.ramSize=1024" >> /root/.android/avd/test-avd.avd/config.ini

ADD supervisord.conf /etc/supervisor/conf.d/
CMD ["/usr/bin/supervisord"]
