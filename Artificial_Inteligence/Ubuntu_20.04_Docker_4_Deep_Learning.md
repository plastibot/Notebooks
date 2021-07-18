
Steps for this guide were taken from here: https://towardsdatascience.com/set-up-of-a-personal-gpu-server-for-machine-learning-with-ubuntu-20-04-100e787105ad

## 1. install Ubuntu 20.04

Download image and install following directions here

Ensure you select to install 3rd party drivers.

After installation is complete update and install necessary packages:

``` bash
sudo apt-get update &&
sudo apt-get -y upgrade &&
sudo apt-get -y install build-essential gcc g++ make binutils &&
sudo apt-get -y install software-properties-common git &&
sudo apt-get install build-essential cmake git pkg-config
```

## 2. Set remote access

install ssh-server

``` bash
sudo apt update &&
sudo apt install openssh-server
```

Verify the ssh-server was installe correctly

```bash
sudo systemctl status ssh
```

Open the firewall

```bash
sudo ufw allow ssh
```

Make note of your server IP address

```bash
ip add
```

Test your ssh connection bu running from another computer the following:

```bash
ssh user@<local-ip-address>
```

where user is your server ubuu user name and local-ip-addres sis the Ip address you found on previous step.

Next, set-up ssh keys for password-less login as described here: https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-2

## 3. Confirm your Nvidia driver is installed

``` bash
nvidia-smi
```


## 4. Install Docker Container

Steps for this section of the guide were taken from: https://www.tensorflow.org/install/docker

Docker uses containers to create virtual environments that isolate a TensorFlow installation from the rest of the system. TensorFlow programs are run within this virtual environment that can share resources with its host machine (access directories, use the GPU, connect to the Internet, etc.). The TensorFlow Docker images are tested for each release.

Docker is the easiest way to enable TensorFlow GPU support on Linux since only the NVIDIA® GPU driver is required on the host machine (the NVIDIA® CUDA® Toolkit does not need to be installed).

### a) Install Docker CE

Follow installation instructions from here: https://docs.docker.com/get-docker/

### b) Install Nvidia Docker Support for GPU support.

setup the ```stable``` repository and the GPG key:

```bash
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
   && curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add - \
   && curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
```

Install the nvidia-docker2 package (and dependencies) after updating the package listing:

```bash
sudo apt-get update
sudo apt-get install -y nvidia-docker2
```

Restart the Docker daemon to complete the installation after setting the default runtime:

```bash
sudo systemctl restart docker
```

Test that your installation was succesfull by running the following base CUDA container:

```bash
sudo docker run --rm --gpus all nvidia/cuda:11.0-base nvidia-smi
```

This should result in a console output shown below:

```bash
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 450.51.06    Driver Version: 450.51.06    CUDA Version: 11.0     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  Tesla T4            On   | 00000000:00:1E.0 Off |                    0 |
| N/A   34C    P8     9W /  70W |      0MiB / 15109MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|  No running processes found                                                 |
+-----------------------------------------------------------------------------+
```

### c) Access Docker without the need of sudo

``` bash
$ sudo usermod -a -G docker $USER
$ sudo reboot
```

### d) Download TensorFlow docker image

The official TensorFlow Docker images are located in the tensorflow/tensorflow Docker Hub repository. https://hub.docker.com/r/tensorflow/tensorflow/

Image releases are tagged using the following format:

| Tag	| Description |
| -----| ------- |
| latest |	The latest release of TensorFlow CPU binary image. Default. |
| nightly	| Nightly builds of the TensorFlow image. (Unstable.) |
| version	| Specify the version of the TensorFlow binary image, for example: 2.1.0 |
| devel	| Nightly builds of a TensorFlow master development environment. Includes TensorFlow source code. |
| custom-op	| Special experimental image for developing TF custom ops. More info here. |


Each base tag has variants that add or change functionality:

| Tag Variants | Description |
|----|------|
| tag-gpu |	The specified tag release with GPU support. (See below) |
| tag-jupyter |	The specified tag release with Jupyter (includes TensorFlow tutorial notebooks) |

You can use multiple variants at once. For example, the following downloads TensorFlow release images to your machine:

```bash
docker pull tensorflow/tensorflow                     # latest stable release
docker pull tensorflow/tensorflow:devel-gpu           # nightly dev release w/ GPU support
docker pull tensorflow/tensorflow:latest-gpu-jupyter  # latest release w/ GPU support and Jupyter
```

### e) Start TensorFlow Docker container

```bash
docker run --gpus all [-it] [--rm] [-p hostPort:containerPort] [-v hostSource:containerDestination] tensorflow/tensorflow[:tag] [command]
```

for example to start a Jupyter Notebook server with GPU support using TensorFlow 2.2.2 and be able to read notebooks on server on directory /home/freeman/DeepLizard

```bash
docker run --gpus all -it -p 8888:8888 -v /home/freeman/DeepLizard:/tf/ tensorflow/tensorflow:2.2.2-gpu-py3-jupyter
```

## 5. Update Dependencies for your environment

The code examples I plan on running on the container environment requires SciKit-Learn 0.22.2.post1. The docker image does not include scikit-learn. In order to install the dependency you need to acces your running container as follows. You can ind more information about how to work with containers here: https://stephen-odaibo.medium.com/docker-containers-python-virtual-environments-virtual-machines-d00aa9b8475 and https://analyticsindiamag.com/docker-solution-for-tensorflow-2-0-how-to-get-started/

### a) Find the name of your running container

```bash
$docker ps -a
CONTAINER ID   IMAGE                                         COMMAND                  CREATED             STATUS                  PORTS                    NAMES
e31432eed782   tensorflow/tensorflow:2.3.1-gpu-jupyter       "bash -c 'source /et…"   About an hour ago   Up About an hour        0.0.0.0:8888->8888/tcp   funny_robinson
```
### b) Enter the container using an interactive shell

```bash
$docker exec -it funny_robinson bash
```

### c) Use PIP to install dependencies

```bash
$pip install scikit-learn==0.22.2.post1
```

### d) Exit the container interactive shell

```bash
$exit
```

### d) Commit changes and save the new container instance

Use the following command to commit the changes made to the container and save it as a new image/version.

```bash
docker commit container_id new_name_for_image
```

for example

```bash
docker commit funny_robinson DeepLizard_Keras_v1.03
```

### e) Run your new container

```bash
docker run --gpus all -it -p 8888:8888 -v /home/freeman/DeepLizard:/tf/ deeplizard_keras_v103

