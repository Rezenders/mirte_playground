# Use Mirte with Docker

## Download docker image
You can download the docker image directly from github

```Bash
docker pull ghcr.io/kas-lab/mirte_playground:main
```

## Build docker image

If you prefer, you can build the image yourself instead.

Build:
```Bash
mkdir -p ~/mirte_ws/src
cd ~/mirte_ws/src
git clone https://github.com/kas-lab/mirte_playground.git
docker build -t mirte .
```

## Run docker image

Allow container to use the host machine display
```Bash
xhost +
```

```{note}
If you built the image yourself, replace `ghcr.io/kas-lab/mirte_playground:main` with `mirte`
```

Run container without GPU support:
```Bash
docker run -it --rm --name mirte -e DISPLAY=$DISPLAY -e QT_X11_NO_MITSHM=1 -v /tmp/.X11-unix:/tmp/.X11-unix -v /etc/localtime:/etc/localtime:ro --network host ghcr.io/kas-lab/mirte_playground:main
```

Run container with nvidia support (for this to work, first you need to install [nvidia-docker](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)):
```Bash
docker run -it --rm --gpus all --runtime=nvidia --name mirte -e DISPLAY=$DISPLAY -e QT_X11_NO_MITSHM=1 -e NVIDIA_VISIBLE_DEVICES=all -e NVIDIA_DRIVER_CAPABILITIES=all -v /dev/dri:/dev/dri -v /tmp/.X11-unix:/tmp/.X11-unix -v /etc/localtime:/etc/localtime:ro --network host ghcr.io/kas-lab/mirte_playground:main
```

After you started a container in one terminal, you can access the same container in other terminals with the command below. This approach is recommended over starting new containers.
```Bash
docker exec -it mirte bash
```

### Developing with docker

To make it easier to develop using the provided docker images, you can mount folders from the local machine inside the container, and simply code from the local machine. To do that, simply add an argument of the form `-v $HOME/mirte_ws/src/mirte_playground:/home/ubuntu-user/mirte_ws/src/mirte_playground` to the docker run command. This will mount the `mirte_playground` package from the local machine into the container, you can mount as many folders as you want.

```{note}
Don't forget to first download the packages to the local machine before trying to mount them.
```

Run container without GPU support:
```Bash
docker run -it --rm --name mirte -e DISPLAY=$DISPLAY -e QT_X11_NO_MITSHM=1 -v /tmp/.X11-unix:/tmp/.X11-unix -v /etc/localtime:/etc/localtime:ro -v $HOME/mirte_ws/src/mirte_playground:/home/ubuntu-user/mirte_ws/src/mirte_playground --network host ghcr.io/kas-lab/mirte_playground:main
```

Run container with nvidia support (for this to work, first you need to install [nvidia-docker](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)):
```Bash
docker run -it --rm --gpus all --runtime=nvidia --name mirte -e DISPLAY=$DISPLAY -e QT_X11_NO_MITSHM=1 -e NVIDIA_VISIBLE_DEVICES=all -e NVIDIA_DRIVER_CAPABILITIES=all -v /dev/dri:/dev/dri -v /tmp/.X11-unix:/tmp/.X11-unix -v /etc/localtime:/etc/localtime:ro -v $HOME/mirte_ws/src/mirte_playground:/home/ubuntu-user/mirte_ws/src/mirte_playground --network host ghcr.io/kas-lab/mirte_playground:main
```