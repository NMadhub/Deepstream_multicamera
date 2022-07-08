# Deepstream 5.1 YOLOv4 App

This Deepstream application showcases Multi-camera Object Detection and Live Tracking using YOLOv4 model running at High FPS throughput!

[![FPS](resources/fps.gif)]

## Index

1. [Deepstream Setup](#Deepstream-Setup)
    1. [Pull Deepstream Container](#Install-Deepstream)
2. [Running the Application](#Running-the-Application)
    1. [Clone the repository](#Cloning-the-repository)
    2. [Download the weights file](#download-the-weights-file)
    3. [Build the application](#build-the-application)
    4. [Run with different input sources](#Run-with-different-input-sources)
3. [Citations](#citations)

## Deepstream Setup

This post assumes you have a fully functional Jetson device. If not, you can refer the documentation [here](https://docs.nvidia.com/jetson/jetpack/install-jetpack/index.html).

### 1. Pull Deepstream Container

Enter the command:

```sh
docker pull nvcr.io/nvidia/deepstream:5.1-21.02-devel

docker run --name=ds-522 --gpus all -it --net=host --privileged -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=$DISPLAY -v <local_dir>:/workspace/user -w /opt/nvidia/deepstream/deepstream-5.1 nvcr.io/nvidia/deepstream:5.1-21.02-devel

```

For more information, go to the get started page of Deepstream [here](https://docs.nvidia.com/metropolis/deepstream/dev-guide/index.html).

## Running the Application

### 1. Clone the repository

This is a straightforward step, however, if you are new to git, I recommend glancing threw the steps.

First, install git

```sh
sudo apt install git
```

Next, clone the repository

```sh
# Using HTTPS
https://github.com/aj-ames/YOLOv4-Deepstream.git
# Using SSH
git@github.com:aj-ames/YOLOv4-Deepstream.git
```

### 2. Download the weights file

Download the weights file from [google-drive](https://drive.google.com/file/d/1nZds8loc4XdG4KQGdgoU-xyOgwJqv9m-/view?usp=sharing) and place it in `models/YOLOv4` directory.

Please ensure you have your CUDA libraries and paths are proper before proceeding further. If not please do as follows:
Go to ```vi ~/.bashrc```. Then ```source ~/.bashrc```
Add the following lines:
```sh
# CUDA
export CUDA=11.1
export PATH=/usr/local/cuda-$CUDA/bin${PATH:+:${PATH}}
export CUDA_PATH=/usr/local/cuda-$CUDA
export CUDA_HOME=/usr/local/cuda-$CUDA
export LIBRARY_PATH=$CUDA_HOME/lib64:$LIBRARY_PATH
export LD_LIBRARY_PATH=/usr/local/cuda-$CUDA/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda/extras/CUPTI/lib64:$LD_LIBRARY_PATH
export NVCC=/usr/local/cuda-$CUDA/bin/nvcc
export CFLAGS="-I$CUDA_HOME/include $CFLAGS"
```


### 3. Build the application

First, build the application by running the following command:

```sh
make clean && make -j$(nproc)
```

This will generate the binary called `ds-yolo`. This is a one-time step and you need to do this only when you make source-code changes.

For some common errors & fixes:

```sh
apt install libopencv-dev


apt-get install libboost-all-dev


apt install libgnutls28-dev
```

### 4. Run with different input sources

Next, create a file called `inputsources.txt` and paste the path of videos or rtsp url.

```sh
file:///home/nmadhab/Videos/sample_qHD.mp4
rtsp://admin:admin%40123@192.168.1.1:554/stream
```

Now, run the application by running the following command:

```sh
./ds-yolo
```

## Citations

* [AlexeyAB/darknet](https://github.com/AlexeyAB/darknet)
