# Android SDK Emulator Image

## Image Info

This image is based on https://github.com/thyrlian/AndroidSDK

The image installs 
* Android SDK Version 4333796
* Android packages "platform-tools", "emulator"
* Android platform api specified by build arg ANDROID_API_VERSION
* Android build-tools specified by build arg BUILD_TOOLS_VERSION
* Android emulator image path specified by build arg EMULATOR_IMAGE_PATH
* Appium server

The default build args are
* ANDROID_API_VERSION=android-28
* BUILD_TOOLS_VERSION=28.0.3
* EMULATOR_IMAGE_PATH="system-images;${ANDROID_API_VERSION};google_apis;x86"

## To run the container
```
docker pull suryasreevedula/android-sdk-emulator
sudo docker run --privileged -d -p 4723:4723 -p 5901:5901 -p 2222:22 suryasreevedula/android-sdk-emulator
```

The container starts
* emulator
* appium server at 4723
* VNC server at 5901
