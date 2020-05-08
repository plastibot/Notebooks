
## Install ONNX from source on a Mac

### 1. install dependencies

ONNX requires cmake, autoconf, automake. Use Homebrew to install it.

````bash
$brew install cmake
$brew install autoconf
$brew install automake
````

ONNX requires protobuf-compiler libprotoc-dev. Go to the following link and download version 3.11.4.
You will have to scroll down a little bit to find 3.11.4

````
https://github.com/protocolbuffers/protobuf/releases
````

unzip the file and enter the folder 

````bash
$cd protobuf-3.11.4
$./autogen.sh && ./configure && make
$make check
$sudo make install
````

Check installation was succesfull. The below comands should report /usr/local/bin/protoc and libprotoc 3.11.4

````bash
$which protoc
$protoc --version
````

ONNX requires Python 3.6.X
If you don't have that version of Python installed on your mac, install 
pyenv so that you can have more than one version in your system. Instructions on
how to install pyenv are here:

https://realpython.com/intro-to-pyenv/

Once you have installed pyenv, Let's create version 3.6.9

````bash
$pyenv global 3.6.9
````

Let's then create the virtual environment

````bash
$mkdir onnx
$cd onnx
$pyenv virtualenv 3.6.9 onnx
$pyenv local onnx
$python -V
````

From now on, eveytime you enter the onnx folder it will automatically activate the virtual environment
onnx with python 3.6.9.

### 2. install ONNX from source

```bash
$git clone https://github.com/onnx/onnx.git
$cd onnx
$git submodule update --init --recursive
$pip install -e .
````

Verification and Test

````bash 
$cd ..
$python -c "import onnx"
$pip install pytest nbval
$cd onnx/
$pytest
````

### 3. Install Tensorflow

use the stable 2.x release

````bash
$pip install -U tensorflow
$pip install -U tensorflow-addons
````

Verification and Test - will return version 2.2 for the version and 3 for thelast command (ignore the system warnings).

````python
$ python
'Python 3.6.9 (default, May  8 2020, 13:10:40) 
[GCC 4.2.1 Compatible Apple LLVM 11.0.0 (clang-1100.0.33.12)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import tensorflow as tf
>>> tf.__version__
'2.2.0'
>>> tf.add(1,2).numpy()
2020-05-08 15:16:13.849678: I tensorflow/core/platform/cpu_feature_guard.cc:143] Your CPU supports instructions that this TensorFlow binary was not compiled to use: AVX2 FMA
2020-05-08 15:16:13.868484: I tensorflow/compiler/xla/service/service.cc:168] XLA service 0x7f9c12ab3fa0 initialized for platform Host (this does not guarantee that XLA will be used). Devices:
2020-05-08 15:16:13.868509: I tensorflow/compiler/xla/service/service.cc:176]   StreamExecutor device (0): Host, Default Version
3
````


