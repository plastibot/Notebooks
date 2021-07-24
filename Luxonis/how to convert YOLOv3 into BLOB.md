## 1. Convert YOLO model to Tensorflow frozen model

### Cloning repo that helps with the conversion
``` bash
git clone https://github.com/mystic123/tensorflow-yolo-v3.git
cd tensorflow-yolo-v3/
git checkout ed60b90
```

### Installing dependencies

The program above has the following dependencies:

Python 3.6 or lower
Tensorflow 1.11.0

### Checking your version of python

Enter the following to check what version of python you have

```bash
python3 -V
```

If your system has something higher than python 3.6.9 you will need to install a previous version of python. Otherwise you can skip this step.

The easiest way to have more than one version of python installed is using pyenv. Follow this link for instructions to install pyenv and setup a different version.


### Create virtual environment

We are going to create a virtual environment so that we can have tensor flow 1.11.0 along side other versions. enter the following commands

```bash
python3 -m venv venv
source ./venv/bin/activate
pip install --upgrade pip
pip install tensorflow=1.11.0
```

### converting the model

```bash
python3 convert_weights_pb.py --class_names /model/model.names --data_format NHWC --weights_file /model/model.weights
```

You shoud now have a Tensorflow frozen model on directory ~/tensorflow-yolo-v3/



## 2. Convert frozen TF model to openvino 20.01 IRv10Convert frozen TF model to openvino 20.01 IRv10

### install openVino

```bash
wget "https://registrationcenter-download.intel.com/akdlm/irc_nas/17662/l_openvino_toolkit_p_2021.3.394.tgz"
```

### Extract & install openvino

``` bash
tar xf l_openvino_toolkit_p_2021.3.394.tgz
cd l_openvino_toolkit_p_2021.3.394
./install.sh
```

### Install openvino dependencies

```bash
cd /opt/intel/openvino_2021/install_dependencies
sudo -E ./install_openvino_dependencies.sh
```

### Set the Environment Variables

```bash
source /opt/intel/openvino_2021/bin/setupvars.sh
```

The OpenVINO environment variables are removed when you close the shell. As an option, you can permanently set the environment variables as follows:

Open the .bashrc file in <user_directory>

```bash
nano ~/.bashrc
```

Add this line to the end of the file:

```bash
source /opt/intel/openvino_2021/bin/setupvars.sh
```

Save and close the file by pressing Ctrl+X then Y and enter 

