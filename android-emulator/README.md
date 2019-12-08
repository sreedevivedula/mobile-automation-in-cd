## To run the container
```
    export SELENIUM_GRID_HOST=<Selenium Grid Host>
    export SELENIUM_GRID_PORT=<Selenium Grid Port>
    export APPIUM_NODE_HOST=<VM OR HOST IP>
    export APPIUM_NODE_PORT=<PORT>
    docker run --privileged -d -p $APPIUM_NODE_PORT:$APPIUM_NODE_PORT -p 5901:5901 -p 2222:22 \
        -e SELENIUM_GRID_HOST=$SELENIUM_GRID_HOST -e SELENIUM_GRID_PORT=$SELENIUM_GRID_PORT \
        -e APPIUM_NODE_HOST=$APPIUM_NODE_HOST
        --name android-emulator suryasreevedula/android-emulator:latest

```