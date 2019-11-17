FROM thyrlian/android-sdk-vnc:latest

MAINTAINER sreedevi@thoughtworks.com

ENV DEBIAN_FRONTEND noninteractive

# Install appium
RUN apt-get update && \
    apt-get install -y curl && \
    curl -sL https://deb.nodesource.com/setup_12.x | bash - && \
    apt-get install -y nodejs && \
    npm install appium

# Install Android Emulator
ARG ANDROID_API_VERSION=android-28
ARG BUILD_TOOLS_VERSION=28.0.3
ARG EMULATOR_IMAGE_PATH="system-images;${ANDROID_API_VERSION};google_apis;x86"

RUN sdkmanager "platform-tools" "platforms;${ANDROID_API_VERSION}" "emulator" "build-tools;${BUILD_TOOLS_VERSION}" && \
    yes | sdkmanager "${EMULATOR_IMAGE_PATH}" && \
    echo "no" | avdmanager create avd -n test-avd -k ${EMULATOR_IMAGE_PATH} -d 19 && \
    echo "hw.ramSize=1024" >> /root/.android/avd/test-avd.avd/config.ini

ADD supervisord.conf /etc/supervisor/conf.d/
CMD ["/usr/bin/supervisord"]