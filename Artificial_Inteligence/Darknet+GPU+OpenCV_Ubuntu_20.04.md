Steps for this guide were taken from: https://medium.com/@abhichand26/how-to-setup-cuda-in-ubuntu-for-darknet-yolo-fdd25cd70aba

This tutorial is tested on multiple 20.04.1 PCs with GTX 1080ti & GTX 1050ti.

## Content:
- Ubuntu Setup
- CUDA Download and Setup
- Install cuDNN
- Install Darknet
- Add GPU Support Darknet
- Add OpenCV support to Darknet.

## 1. Ubuntu Setup
First we need to install some packages in our Ubuntu system. Run the following commands.

``` bash
sudo apt-get update
sudo apt-get upgrade -y
sudo apt-get install -y libopencv-dev
sudo apt-get install -y build-essential cmake unzip pkg-config
sudo apt-get install -y libxmu-dev libxi-dev libglu1-mesa libglu1-mesa-dev
sudo apt-get install -y libjpeg-dev libpng-dev libtiff-dev
sudo apt-get install -y libavcodec-dev libavformat-dev libswscale-dev
sudo apt-get install -y libv4l-dev libxvidcore-dev libx264-dev
sudo apt-get install -y libgtk-3-dev libopenblas-dev
sudo apt-get install -y libatlas-base-dev liblapack-dev gfortran
sudo apt-get install -y libhdf5-serial-dev graphviz
sudo apt-get install -y python3-dev python3-tk python-imaging-tk
sudo apt-get install -y linux-image-generic linux-image-extra-virtual
sudo apt-get install -y linux-source linux-headers-generic
```

You can also copy paste the above commands into a sh file like installLibs.sh Then just execute that file using
`sudo sh installLibs.sh`


## 2. CUDA Download and Setup
First make sure you disable the nouveau driver if you have it installed.
To check if it is loaded run this command.

`lsmod | grep nouveau`

If there is no result, then you are good to go.
If you get a result, then you need to disable nouveau.

### Disable nouveau
Create a file at /etc/modprobe.d/blacklist-nouveau.conf using the nano command like this:
nano /etc/modprobe.d/blacklist-nouveau.conf

### Add the following contents in the file:
blacklist nouveau options nouveau modeset=0
Save and exit the file by typing
crtl + o
[enter]
crtl + x


### Regenerate the kernel initramfs:
``` bash
sudo update-initramfs -u
Install CUDA Toolkit
Once this is done, you can download the CUDA Toolkit runfile from here
Assuming I want to download & install CUDA Toolkit 11.0 Update 1.
I will run the 2 commands mentioned in CUDA Toolkit website.
wget https://developer.download.nvidia.com/compute/cuda/11.0.3/local_installers/cuda_11.0.3_450.51.06_linux.run
sudo sh cuda_11.0.3_450.51.06_linux.run
Follow the installer instructions and install the CUDA Toolkit. (You can skip the NVIDIA Driver if you have it installed already)
Add CUDA to Environment Path
The file is a shell script for Bash (or the Linux Shell) to run.
nano ~/.bashrc
At the bottom of the file, add the following lines (make sure you change the CUDA version according to your installation):
# NVIDIA CUDA Toolkit
export PATH=/usr/local/cuda-11.0/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-11.0/lib64
export CPATH=/usr/local/cuda-11.0/targets/x86_64-linux/include:$CPATH
Save and exit the ~/.bashrc file by typing
crtl + o
[enter]
crtl + x
Update the system path file
source ~/.bashrc
Check that CUDA is properly installed
nvcc -V

## 3 — Install cuDNN
This is the NVIDIA CUDA Deep Neural Network library. It is used for installing the gpu-accelerated libraries. You will need a developer NVIDIA account to download cuDNN.
Signup & download cuDNN: https://developer.nvidia.com/cudnn
After downloading the appropriate cuDNN for your CUDA Toolkit version, open the terminal in which the downloaded file is located and run the following commands.

``` bash
tar -zxf cudnn-11.0-linux-x64-v8.0.4.30.tgz
cd cuda
sudo cp -P lib64/* /usr/local/cuda/lib64/
sudo cp -P include/* /usr/local/cuda/include/
cd ~
```

## 3. Installing Darknet

Steps for this guide were taken from: https://pjreddie.com/darknet/install/

First clone the Darknet git repository here. This can be accomplished by:

``` bash
git clone https://github.com/pjreddie/darknet.git
cd darknet
make
```

If this works you should see a whole bunch of compiling information fly by:

``` bash
mkdir -p obj
gcc -I/usr/local/cuda/include/  -Wall -Wfatal-errors  -Ofast....
gcc -I/usr/local/cuda/include/  -Wall -Wfatal-errors  -Ofast....
gcc -I/usr/local/cuda/include/  -Wall -Wfatal-errors  -Ofast....
.....
gcc -I/usr/local/cuda/include/  -Wall -Wfatal-errors  -Ofast -lm....
```

If you have any errors, try to fix them? If everything seems to have compiled correctly, try running it!

``` bash
./darknet
```

You should get the output:

``` bash
usage: ./darknet <function>
````

## 4. Compiling With CUDA
Darknet on the CPU is fast but it's like 500 times faster on GPU! You'll have to have an Nvidia GPU and you'll have to install CUDA. I won't go into CUDA installation in detail because it is terrifying.



Once you have CUDA installed, change the first line of the Makefile in the base directory to read:
``` bash
GPU=1
```

  Proceed to make the file once again
  
  ``` bash
  cd ~/darknet
  make
  ```

### Note:
if it fails with an error saying "compute 30" is not supported. Remove that line from the make file. As of Cuda 11.0 "Compute 30" has been deprecated.

``` bash
cd ~/darknet
nano Makefile
```

Remove this part

```
-gencode arch=compute_30,code=sm_30 \
```

so that it looks like this:

```
ARCH= -gencode arch=compute_35,code=sm_35 \
      -gencode arch=compute_50,code=[sm_50,compute_50] \
      -gencode arch=compute_52,code=[sm_52,compute_52]
````


Now you can make the project and CUDA will be enabled. By default it will run the network on the 0th graphics card in your system (if you installed CUDA correctly you can list your graphics cards using nvidia-smi). If you want to change what card Darknet uses you can give it the optional command line flag -i <index>, like:

  ``` bash
./darknet -i 1 imagenet test cfg/alexnet.cfg alexnet.weights
```
  
  If you compiled using CUDA but want to do CPU computation for whatever reason you can use -nogpu to use the CPU instead:

``` bash
./darknet -nogpu imagenet test cfg/alexnet.cfg alexnet.weights
```
  
  Enjoy your new, super fast neural networks!

## 5. Compiling With OpenCV
By default, Darknet uses stb_image.h for image loading. If you want more support for weird formats (like CMYK jpegs, thanks Obama) you can use OpenCV instead! OpenCV also allows you to view images and detections without having to save them to disk.

First install OpenCV. If you do this from source it will be long and complex so we will install using the package manager. OpenCV is available for installation from the default Ubuntu 20.04 repositories. To install it run:

``` bash
sudo apt update
sudo apt install libopencv-dev python3-opencv
```  
  
The command above will install all packages necessary to run OpenCV.

Verify the installation by importing the cv2 module and printing the OpenCV version:

``` bash
python3 -c "import cv2; print(cv2.__version__)"
```
  
At the time of writing, the version in the repositories is 4.2:

``` bash
4.2.0
```  
  
Next, change the 2nd line of the Makefile to read:

``` bash
OPENCV=1
```
  
  Proceed to make the file once again
  
  ``` bash
  cd ~/darknet
  make
  ```
  
### Note:
If you get an error saying:  
  
``` bash  
  ./src/image_opencv.cpp:12:1: error: ‘IplImage’ does not name a type; did you mean ‘image’?
```

  Per this page, https://github.com/pjreddie/darknet/issues/1347  Some C APIs, which were deprecated for a long time, were removed from the latest OpenCV release. Darknet code using functions that work on IplImage fails to compile.

The solution is to remove references to the C API and use the C++ API (eg. cv::Mat instead of IplImage)
  
  
  So based on this page https://www.programmersought.com/article/24634963452/ you need to replace the text on image_opencv.cpp as follows: 
  
  Open the file
  
``` bash  
  cd darknet/src
  nano image_opencv.cpp
```  

  Replace the code shown with this code:
  
``` c++
#ifdef OPENCV

#include "stdio.h"
#include "stdlib.h"
#include "opencv2/opencv.hpp"
#include "image.h"

using namespace cv;

extern "C" {

Mat image_to_mat(image im)
{
    image copy = copy_image(im);
    constrain_image(copy);
    if(im.c == 3) rgbgr_image(copy);

    Mat m(cv::Size(im.w,im.h), CV_8UC(im.c));
    int x,y,c;

    int step = m.step;
    for(y = 0; y < im.h; ++y){
        for(x = 0; x < im.w; ++x){
            for(c= 0; c < im.c; ++c){
                float val = im.data[c*im.h*im.w + y*im.w + x];
                m.data[y*step + x*im.c + c] = (unsigned char)(val*255);
            }
        }
    }

    free_image(copy);
    return m;
}

image mat_to_image(Mat m)
{
    int h = m.rows;
    int w = m.cols;
    int c = m.channels();
    image im = make_image(w, h, c);
    unsigned char *data = (unsigned char *)m.data;
    int step = m.step;
    int i, j, k;

    for(i = 0; i < h; ++i){
        for(k= 0; k < c; ++k){
            for(j = 0; j < w; ++j){
                im.data[k*w*h + i*w + j] = data[i*step + j*c + k]/255.;
            }
        }
    }
    rgbgr_image(im);
    return im;
}

void *open_video_stream(const char *f, int c, int w, int h, int fps)
{
    VideoCapture *cap;
    if(f) cap = new VideoCapture(f);
    else cap = new VideoCapture(c);
    if(!cap->isOpened()) return 0;
    if(w) cap->set(CAP_PROP_FRAME_WIDTH, w);
    if(h) cap->set(CAP_PROP_FRAME_HEIGHT, w);
    if(fps) cap->set(CAP_PROP_FPS, w);
    return (void *) cap;
}

image get_image_from_stream(void *p)
{
    VideoCapture *cap = (VideoCapture *)p;
    Mat m;
    *cap >> m;
    if(m.empty()) return make_empty_image(0,0,0);
    return mat_to_image(m);
}

image load_image_cv(char *filename, int channels)
{
    int flag = -1;
    if (channels == 0) flag = -1;
    else if (channels == 1) flag = 0;
    else if (channels == 3) flag = 1;
    else {
        fprintf(stderr, "OpenCV can't force load with %d channels\n", channels);
    }
    Mat m;
    m = imread(filename, flag);
    if(!m.data){
        fprintf(stderr, "Cannot load image \"%s\"\n", filename);
        char buff[256];
        sprintf(buff, "echo %s >> bad.list", filename);
        system(buff);
        return make_image(10,10,3);
        //exit(0);
    }
    image im = mat_to_image(m);
    return im;
}

int show_image_cv(image im, const char* name, int ms)
{
    Mat m = image_to_mat(im);
    imshow(name, m);
    int c = waitKey(ms);
    if (c != -1) c = c%256;
    return c;
}

void make_window(char *name, int w, int h, int fullscreen)
{
    namedWindow(name, WINDOW_NORMAL); 
    if (fullscreen) {
        setWindowProperty(name, WND_PROP_FULLSCREEN, WINDOW_FULLSCREEN);
    } else {
        resizeWindow(name, w, h);
        if(strcmp(name, "Demo") == 0) moveWindow(name, 0, 0);
    }
}

}

#endif
```

  That's it!
  
  
  You're done! To try it out, first re-make the project. Then use the imtest routine to test image loading and displaying:

./darknet imtest data/eagle.jpg
If you get a bunch of windows with eagles in them you've succeeded! They may look like:

