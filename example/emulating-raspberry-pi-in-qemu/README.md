# Emulating a Raspberry Pi 3b with QEMU

The Dockerfile in this directory contains all the steps to set up a virtual image 
of Raspberry Pi OS and start it via QEMU, all contained in the 
docker-image.

See this [Blog post](https://interrupt.memfault.com/blog/emulating-raspberry-pi-in-qemu) 
for details.

## Running the pre-built Docker image

```shell
docker run -it --rm -p 2222:2222 stawiski/qemu-raspberrypi-3b:2023-05-03-raspios-bullseye-arm64
```

This will boot the emulated Raspberry Pi device and after some time should 
show a login prompt.

You can also connect via ssh after booting is finished

```shell
ssh -p 2222 pi@localhost
```

## Rebuilding the Docker image locally

```shell
cd example/emulating-raspberry-pi-in-qemu/
docker build -t qemu-raspberrypi-3b:local .
```

You can then run the locally built image with

```shell
docker run -it --rm -p 2222:2222 --name qemu-raspberrypi-3b-run qemu-raspberrypi-3b:local
```

If you want to inspect the contents of the Docker container 
itself, you can run the following when the container is started

```shell
docker exec -it qemu-raspberrypi-3b-run /bin/bash
```

## Limitations

Only some of the hardware of the Raspberry Pi is emulated, 
e.g. things like WIFI chip, bluetooth, GPIO pins, ... might 
be missing, so some software may fail to run if it depends 
on those things.
