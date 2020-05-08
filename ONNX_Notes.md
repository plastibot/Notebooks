

## Steps to install ONNX on MacOS

### First create a virtual environment

````bash
$conda create -n onnx python=3.6
$conda activate onnx
````

### Next, you will need an install of protobuf and numpy to build ONNX. One easy way to get these dependencies is via Anaconda:

````bash
$conda install -c conda-forge protobuf numpy
````

### You can then install ONNX from PyPi

````bash
pip install onnx
````

### Verify installation was successfull

````bash
$python -c "import onnx"
````
