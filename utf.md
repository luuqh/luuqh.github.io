[© Hung Q. Luu](https://luuqh.github.io)


## Setup Ubuntu Linux for Deep Learning

While Apple Mac is a reasonable choice to play around with data analytics, most of deep learning tasks are executed on Linux-like systems. We will set up an Ubuntu system with hardware configuration of Intel Core i7, NVIDIA GTX 970, 16 GB RAM, 1 TB SSD and 3 TB HDD.  

### Install Ubuntu and basic packages

Ubuntu is a Linux distribution based on Debian and composed mostly of free and open-source software. The latest Long-Term Support (LTS) version Ubuntu 20.04LTS is its best, providing many advance features and having a nice Graphic User Interface (GUI). It is prebuilt with a large collection of driver, from wifi adapter to mainboard, so it is very convenient to use it even for daily tasks.

To install Ubuntu, you need a blank flash (USB) thumbdrive. To move on, paste the below link to get the CD/DVD (ISO) image of Ubuntu
```
https://releases.ubuntu.com/20.04/ubuntu-20.04.2-desktop-amd64.iso
```
I prefer to use the 20.04LTS instead of the latest version 20.10. It is because when I install 20.10, many Docker and NVIDIA features are missing or unsupported, requiring me to go backward to a version that is more stable and well supported.

In the past, installing a new Operating System (OS) require to have a CD/DVD. Nowsadays you can install OS using flash drive. Note that you still do need a laptop/desktop running Windows (that is able to run an Windows-based installation software namely Rufus). Now download the latest version of Rufus (3.13 as the time of writing this instruction) from the address
```
https://github.com/pbatard/rufus/releases/download/v3.13/rufus-3.13.exe
```
Select the downloaded ISO file from Rufus > *Select* button. Retain everything else unchanged. Click *Start* to start writing. Sometimes Rufus may ask you to download supporting tool during installation, then accept it. When it asks you to select the ISO or DD mode, select the DD. In my case, the ISO mode does not work for version 20.04 (but strangly the ISO mode works when I install with Ubuntu 20.10).

It is convenient to use Google Chrome accorss platform. So it is one of the basic packages you want to install at the first sign.

You can download the [Google Chrome for Linux from here](https://www.google.com/chrome/). Don't forget to get the "64-bit .deb (For Debian/Ubuntu] version". Otherwise, you can directly use WGET to grab it from Terminal command line:
```
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
```
Once having the version, install Chrome in the Terminal with DPKG
```
cd [path/to/download/folder]
sudo dpkg -i 'google-chrome-stable_current_amd64.deb' 
```
Some other useful packages that can be installed from Ubuntu Store, such as [VS Code](https://snapcraft.io/code), [VLC](apt://vlc), etc.

### Install Conda and Docker

Anaconda is a distribution of the Python and R programming languages for scientific computing, that aims to simplify package management and deployment. The instruction for installing Conda is based on [this post](https://linuxize.com/post/how-to-install-anaconda-on-ubuntu-20-04/). 

**Step 1: Install Anaconda**

Since Anaconda relies on QT package for its interface, preinstallation is required
```
sudo apt install libgl1-mesa-glx libegl1-mesa libxrandr2 libxrandr2 libxss1 libxcursor1 libxcomposite1 libasound2 libxi6 libxtst6
```
Check out the link to identify the latest version (starting with *Anaconda3-2020.*)
```
https://repo.anaconda.com/archive/
```
Get the installation Bash script for latest version
```
wget -P /tmp https://repo.anaconda.com/archive/Anaconda3-2020.11-Linux-x86_64.sh
```
and install it
```
bash /tmp/Anaconda3-2020.02-Linux-x86_64.sh
```
I prefer to put all programs in a specific folder, so I create a folder *Programs* inside my working folder. So that I can install *Anaconda3*, *Tensorflow* and others inside this *Program* for the ease of management.
```
mrdir /home/(yourusername)/Programs
```
Now run the script file
```
bash /tmp/Anaconda3-2020.11-Linux-x86_64.sh
```
I am not sure the script or the Ubuntu 20.04 that causes this trouble, but seem like the user has to read carefully press Enter when reading all agreement, otherwise you have to run the script again. This does not happen in Ubuntu 20.10 when we use Up/Down button to quickly scrolling through this lenghy statements. As I plan to install inside my *Programs* folder, I type the path when being asked with Anaconda as
```
mrdir /home/(yourusername)/Programs/Anaconda3
```
Accept when being asked to initialize Conda with PATH environment, and then type in the Terminal for the update 
```
source ~/.bashrc
```
Now create a new environment, for instance *tensorflow*
```
conda create --name tensorflow
```
Activate this environment everytime you want to use it for your development with
```
conda activate tensorflow
```
**Step 2: Docker**

The guide to install Docker is given in the [Docker homepage](https://docs.docker.com/engine/install/ubuntu/). It consists of 3 simple sub-steps. First, install supporting packages
```
sudo apt-get update

sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```
Then add Docker official GPG key and verify it with the fingerprint

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo apt-key fingerprint 0EBFCD88
```
Next, add the repository associated wity your Ubuntu version
```
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```
Now start installing the software
```
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

### Install Tensorflow, CUDA, cuDNN

After checking the hardware, I found that my NVIDIA GTX 970 card is supported by the CUDA 6.5 onward.
```
https://developer.nvidia.com/cuda-downloads-geforce-gtx9xx
https://developer.nvidia.com/cuda-toolkit-65
```

**Step 1: Preparation**

Thanks to the backward compatibility, we can install the latest CUDA (11.x), Tensorflow (2.x) and cuDNN (8.x) to use my Maxwell-architecture card. 


Install some supporting packages for Python environment
```
sudo apt install python3-venv python3-dev
```
In case you want to stick to Conda environment, then donot forget to activate it, e.g. *tensorflow*
```
conda activate tensorflow
```
Sometimes PIP package is missing, so you may need to install it first
```
sudo apt install python3-pip
```
We also need the test resource package for a smooth installation of Tensorflow later, so get it 
```
sudo apt install python3-testresources
```
We can add a searching tool as well which later used with *locate* command like *locate cuda | grep /cuda$*
```
sudo apt install mlocate
```

**Step 2: Install CUDA and cuDNN*

To install the correct version of CUDA and cuDNN, locate your operating systems and distro from [the link](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=2004&target_type=deblocal)
```
https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=2004&target_type=deblocal
```
You can see that the instruction for install it can be summarized into the following steps
```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-ubuntu2004.pin
sudo mv cuda-ubuntu2004.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/11.2.1/local_installers/cuda-repo-ubuntu2004-11-2-local_11.2.1-460.32.03-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu2004-11-2-local_11.2.1-460.32.03-1_amd64.deb
sudo apt-key add /var/cuda-repo-ubuntu2004-11-2-local/7fa2af80.pub
sudo apt-get update
sudo apt-get -y install cuda
```

![Nvidia CUDA](images/nvidia-cuda-install.png)


Please check out the formal documents [NVIDIA CUDA Installation Guide for Linux](https://docs.nvidia.com/pdf/CUDA_Installation_Guide_Linux.pdf) to find the guide for different operating systems.


**Step 3: Install Tensorflow**

Tensorflow for Linux should be installed manually using Command Line Interface (CLI). You can find the instruction in the official [Tensorflow website](https://www.tensorflow.org/install/pip#virtual-environment-install). 

For me, I install the latest version of Tensorflow (2.4.1 as of the time of this writing) as follows
```
pip3 install --upgrade tensorflow
```
Tensorflow 2.* comes with natural support for Tensorflow-GPU. So you don't need to install a separate GPU-based Tensorflow version for NVIDIA card.


**Step 4: Testing*

To test whether you have installed successfully CUDA and Tensorflow, try with the following commands
```
whereis cuda
nvidia-smi
locate cuda | grep /cuda$
nvcc --version
```
If you see somethings like below, meaning that it was successfully installed.

![CUDA Testing](images/linux-cuda-testing.png) 
 
A quick verification of Tensorflow installation is the command
```
python3 -c "import tensorflow as tf;print(tf.reduce_sum(tf.random.normal([1000, 1000])))"
```
A testing for Tensorflow GPU installation is the command inside python3
```
python3 -c "import tensorflow as tf; tf.test.is_built_with_cuda()"
```
It should return *True* in case of successful installation.

Another test with Python is
```
python3 -c "import tensorflow as tf; print(tf.__version__);"
```
You should see two lines, one is the CUDA library information and another is the version of Tensorflow
```
2021-02-15 16:35:13.419737: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcudart.so.11.0
2.4.1
```

### Install OpenCV and Darknet

OpenCV is a well-known library for computer vision orginally developed by Intel. We will build OpenCV in our Linux rig from scratch. Darknet is an open source neural network framework written in C/C++ and CUDA, which contains the state-of-the-art real-time object detection system, so-called YOLO. We will install both of them here.

**Step 1: Install OpenCV** 

Unlike the case of Apple Mac M1, we donot need to build the source to get OpenCV worked.

```
sudo apt update
sudo apt install libopencv-dev python3-opencv
```
A simple test that return *4.2.0* will let you know that you have been successful after the command
```
python3 -c "import cv2; print(cv2.__version__)"
```

**Step 2: Install Darknet** 

Install darknet is simple and straightforward by [Joseph Redmon](https://pjreddie.com/darknet/install/). First, I create a folder *darknet* put it in the folder
```
/home/luuquanghung/Programs/darknet
cd darknet
```
Sometimes *git* tool is not available in your system, so install it with
```
sudo apt install git
```

Then grab darknet from GitHub 
```
git clone https://github.com/pjreddie/darknet.git
cd darknet
```
Since we use GPU, now open the *Makefike* in this folder - using text editor or using the following command line
```
nano Makefile
```
Locate the first line of file and change the content *GPU=0* to *GPU=1*. By configurating this, we allow for Darknet to use GPU in deep learning tasks. Also we can take advantage of the CV library in Darknet by changing from *OPENCV=0* to *OPENCV=1*.

Now move on with the compilation in the Terminal window
```
make
```
After compilation, try this command
```
./darknet
```
you will be succesful if you see something like
```
usage: ./darknet <function>

```

You can take some photo from elsewhere, say *YOURIMAGE.png* (case sensitive), put it in the same folder of darknet compilation. Now try the GPU mode with option *-i 0* where *0* is the slot of your NVIDIA GPU. In case you have multiple GPU, you can list them with *nvidia-smi* and select the preferred one with *1* or *2* instead of the default (first) detected GPU.
```
./darknet -i 0 test YOURIMAGE.png cfg/alexnet.cfg alexnet.weights
```
If you want to run the non-GPU for a particular purpose, execute with *-nogpu* option
```
./darknet -nogpu test YOURIMAGE.png cfg/alexnet.cfg alexnet.weights
```

**Step 3: Config OpenCV with Darknet** 

An error might occurs when you compile DarkNet is
```
to the PKG_CONFIG_PATH environment variable
No package 'opencv' found
./src/image_opencv.cpp:5:10: fatal error: opencv2/opencv.hpp: No such file or directory
5 | #include "opencv2/opencv.hpp"
| ^~~~~~~~~~~~~~~~~~~~
compilation terminated.
make: *** [Makefile:86: obj/image_opencv.o] Error 1
```
You need to fix at two places. One is change from *opencv* to *opencv4* in *Makefile* file, i.e. from
```
ifeq ($(OPENCV), 1) 
COMMON+= -DOPENCV
CFLAGS+= -DOPENCV
LDFLAGS+= `pkg-config --libs opencv` -lstdc++
COMMON+= `pkg-config --cflags opencv` 
endif
```
to
```
ifeq ($(OPENCV), 1) 
COMMON+= -DOPENCV
CFLAGS+= -DOPENCV
LDFLAGS+= `pkg-config --libs opencv4` -lstdc++
COMMON+= `pkg-config --cflags opencv4` 
endif
```
Another place is to change the file *./src/image_opencv.cpp* as follows. First, add the new statements 
```
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
```
then comment out the code associated with *IplImage* onwards
```
/*
IplImage *image_to_ipl(image im)
{
    int x,y,c;
    IplImage *disp = cvCreateImage(cvSize(im.w,im.h), IPL_DEPTH_8U, im.c);
    int step = disp->widthStep;
    for(y = 0; y < im.h; ++y){
        for(x = 0; x < im.w; ++x){
            for(c= 0; c < im.c; ++c){
                float val = im.data[c*im.h*im.w + y*im.w + x];
                disp->imageData[y*step + x*im.c + c] = (unsigned char)(val*255);
            }
        }
    }
    return disp;
}

image ipl_to_image(IplImage* src)
{
    int h = src->height;
    int w = src->width;
    int c = src->nChannels;
    image im = make_image(w, h, c);
    unsigned char *data = (unsigned char *)src->imageData;
    int step = src->widthStep;
    int i, j, k;

    for(i = 0; i < h; ++i){
        for(k= 0; k < c; ++k){
            for(j = 0; j < w; ++j){
                im.data[k*w*h + i*w + j] = data[i*step + j*c + k]/255.;
            }
        }
    }
    return im;
}

Mat image_to_mat(image im)
{
    image copy = copy_image(im);
    constrain_image(copy);
    if(im.c == 3) rgbgr_image(copy);

    IplImage *ipl = image_to_ipl(copy);
    Mat m = cvarrToMat(ipl, true);
    cvReleaseImage(&ipl);
    free_image(copy);
    return m;
}

image mat_to_image(Mat m)
{
    IplImage ipl = m;
    image im = ipl_to_image(&ipl);
    rgbgr_image(im);
    return im;
}*/
```
Then remove all CV_ from opencv flags:
```
if(w) cap->set(CV_CAP_PROP_FRAME_WIDTH, w);
if(h) cap->set(CV_CAP_PROP_FRAME_HEIGHT, w);
if(fps) cap->set(CV_CAP_PROP_FPS, w);
```
to
```
if(w) cap->set(CAP_PROP_FRAME_WIDTH, w);
if(h) cap->set(CAP_PROP_FRAME_HEIGHT, w);
if(fps) cap->set(CAP_PROP_FPS, w);
```
Finally, the line in *Makefile* 
```
setWindowProperty(name, CV_WND_PROP_FULLSCREEN, CV_WINDOW_FULLSCREEN);
```
needs to be changed to
```
setWindowProperty(name, WND_PROP_FULLSCREEN, WINDOW_FULLSCREEN);
```
The above debugging is suggested by [*George-Ogden* and *y9luiz*](https://github.com/pjreddie/darknet/issues/1886), which works well for me.

After compiling darknet with GPU enabled and running
```
./darknet detect cfg/yolo.cfg yolo.weights data/dog.jpg
```
You may received the *Assertion 0* error
```
...
28 conv    128  1 x 1 / 1    76 x  76 x 256   ->    76 x  76 x 128  0.379 BFLOPs
29 CUDA Error: out of memory
darknet: ./src/cuda.c:36: check_error: Assertion `0' failed.
Aborted (core dumped)
```
The fix is to change batch size from 64 to 32 in *cfg/yolo.cfg* - thanks to the suggestion of [affian](https://github.com/pjreddie/darknet/issues/304) as follows
```
sudo nano cfg/yolo.cfg 
```
Change *batch=64* to *batch=32* and save (with *Clt+O* and *Enter* before exit with *Clt+X*). Now try again and this statement should work
```
./darknet detect cfg/yolo.cfg yolo.weights data/dog.jpg
