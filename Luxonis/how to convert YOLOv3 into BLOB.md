






## 1. Convert YOLO model to Tensorflow frozen model

### Make a directory called "model" and put your model .cfg and .weights files in there. Rename them to "model" 

```bash
mkdir model
cd model
cp your file ./
cd ..
````

### Cloning repo that helps with the conversion
``` bash
git clone https://github.com/mystic123/tensorflow-yolo-v3.git
cd tensorflow-yolo-v3/
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
pip install tensorflow=1.11.0
pip install pillow
```



### converting the model

Check the size of your pictures by going to the .cfg file. Look for the width or height and change the size argument below accordingly.

```bash
python3 convert_weights_pb.py --class_names ../model/model.names --data_format NHWC --weights_file ../model/model.weights --size 608
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

### Convert Frozen TF model

--input model is the Frozen TF model
--transformations_cofig use the yolo_v3.json configuration file with custom operations located in the <OPENVINO_INSTALL_DIR>/deployment_tools/model_optimizer/extensions/front/tf repository.

classes, coords, num, and masks are attributes that you should copy from the configuration file that was used for model training

```bash
cp 
python3 mo_tf.py --input_model ~/tensorflow-yolo-v3/frozen_darknet_yolov3_model.pb --data_type FP16 --transformations_config ~/model/yolo_v3.json --batch 1 --outputexport MYRIAD_COMPILE=$(find /opt/intel/ -iname myriad_compile)_dir ~/model/




### convert IR into blob

There are seeral ways you can convert to a blob readable by the device. We will explore 2 local options as there i a limitation of 100MB upload for the cloud option

#### compile tool in Open Vino

```bash
cd /opt/intel/openvino_2021/deployment_tools/tools/compile_tool
./compile_tool -m /home/freeman/mvi_dino/model/frozen_darknet_yolov3_model.xml  -o /home/freeman/mvi_dino/model/model.blob -d MYRIAD -VPU_NUMBER_OF_SHAVES 5 -VPU_NUMBER_OF_CMX_SLICES 5
Inference Engine: 
	IE version ......... 2021.4.0
	Build ........... 2021.4.0-3839-cd81789d294-releases/2021/4

Network inputs:
    inputs : FP16 / NCHW
Network outputs:
    detector/yolo-v3/Conv_14/BiasAdd/YoloRegion : FP16 / NCHW
    detector/yolo-v3/Conv_22/BiasAdd/YoloRegion : FP16 / NCHW
    detector/yolo-v3/Conv_6/BiasAdd/YoloRegion : FP16 / NCHW
[Warning][VPU][Config] Deprecated option was used : VPU_MYRIAD_PLATFORM
Done. LoadNetwork time elapsed: 27079 ms
```

```bash
export MYRIAD_COMPILE=$(find /opt/intel/ -iname myriad_compile)
$MYRIAD_COMPILE -m ~/model/frozen_darknet_yolov3_model.xml -ip U8 -VPU_NUMBER_OF_SHAVES 5 -VPU_NUMBER_OF_CMX_SLICES 5 -o ~/model/
```

