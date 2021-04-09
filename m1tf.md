(https://luuqh.github.io)[© Hung Q. Luu]



## Setup Apple silicon Mac for Deep Learning

Apple silicon-based products released in the end of 2020 are the game-changer. Mac computers with Apple silicon can run much faster than the Intel-based competitors for the same configuration. More interestingly, you can use your Mac for data science and machine learning tasks. I have configured my Mac Mini M1 to run deep learning tasks as follows.



### Install Tensorflow on Mac M1

Apple did release a hardware-accelerated TensorFlow and TensorFlow Addons for macOS 11.0+, which supports Tensorflow r2.4rc0 and TensorFlow Addons 0.11.2. It requires Python 3.8 which can be installed via [XCode Command Line Tool](https://developer.apple.com/download/more/?=command%20line%20tools) (you need an Apple Developer account to install XCode). Please check out the nice instruction of [Fabrice Daniel](https://towardsdatascience.com/tensorflow-2-4-on-apple-silicon-m1-installation-under-conda-environment-ba6de962b3b8).

**Step 1: Setup conda environment.** 

You need to install miniconda such as *miniforge* first. Once done, move forward to create a conda environment, for instance *appletf*:
```
conda create --name appletf
```
Now activate it in Terminal window with
```
conda activate appletf
```
Continue to install Python and its main libraries. Note that *numpy* and *keras* will be installed later along with the deployment of Tensorflow. 
```
conda install -y python==3.8.6
conda install -y pandas
conda install -y matplotlib
conda install -y scikit-learn
conda install -y jupyterlab
```

**Step 2: Install Tensorflow** 

Grab the most recent Tensorflow version from Apple team in GitHub:
[https://github.com/apple/tensorflow_macos/releases](https://github.com/apple/tensorflow_macos/releases). 

Unfold Assets tab ![Assets tab](images/apple-tensorflow-assets.png) and select the complete version [tensorflow_macos-0.1alpha1.tar.gz](https://github.com/apple/tensorflow_macos/releases/download/v0.1alpha1/tensorflow_macos-0.1alpha1.tar.gz). Unzip it to your download folder. The key thing is do not run the default installation given by Apple. It does not work as in my case. Note that *pip* should be installed before the following sub-steps as well.

Change directory to this folder
```
cd tensorflow_macos/arm64
```
First, we install numpy and other Tensorflow add-on
```
pip install --upgrade --no-dependencies --force numpy-1.18.5-cp38-cp38-macosx_11_0_arm64.whl grpcio-1.33.2-cp38-cp38-macosx_11_0_arm64.whl h5py-2.10.0-cp38-cp38-macosx_11_0_arm64.whl tensorflow_addons-0.11.2+mlcompute-cp38-cp38-macosx_11_0_arm64.whl
```
Then install additional packages
```
pip install absl-py astunparse flatbuffers gast google_pasta keras_preprocessing opt_einsum protobuf tensorflow_estimator termcolor typing_extensions wrapt wheel tensorboard typeguard
```
Finall install all TensorFlow package by Apple manually
```
pip install --upgrade --force --no-dependencies tensorflow_macos-0.1a1-cp38-cp38-macosx_11_0_arm64.whl
```

### Install OpenCV and Darknet

OpenCV is a well-known library for computer vision orginally developed by Intel. We will build OpenCV in your Mac from scratch. Darknet is an open source neural network framework written in C/C++ and CUDA, which contains the state-of-the-art real-time object detection system, so-called YOLO. We will install both of them here.

**Step 1: Install cmake** 
*cmake* is the tool that allow you to build OpenCV in your Mac. First, grab a Mac-based version from [cmake homepage](https://cmake.org/download/). Better to use the *unversal cmake version 3.19.4*  for Mac OS X 10.13 or later from [this link](https://github.com/Kitware/CMake/releases/download/v3.19.4/cmake-3.19.4-macos-universal.dmg)

After openning, drag and drop the file into Applications folder ![Applications folder](images/mac-applications-cmake.png) of your Mac.

To need to config *cmake* so that later on we can use it from the command line.
sudo "/Applications/CMake.app/Contents/bin/cmake-gui" --install

**Step 2: Install Darknet** 

Install darknet is simple and straightforward by [Joseph Redmon](https://pjreddie.com/darknet/install/). You can download it directly it GitHub or clone using [Git](https://git-scm.com/download/mac). 
```
git clone https://github.com/pjreddie/darknet.git
cd darknet
make
```
Make sure that you compile (*make*) it in the folder that you want to run *darknet* in the future. For example I put it in the folder
```
/Users/YOURNAME/opt/darknet/
```
After compilation
```
./darknet
```
you will be succesful if you see something like
```
usage: ./darknet <function>

```

**Step 3: Build OpenCV** 

First, we download the latest version of OpenCV (4.5.1) in GitHub ![OpenCV from GitHub ](images/github-opencv-download.png) to your Mac, either directly or using git. We need both OpenCV [main packages](https://github.com/opencv/opencv/) and their [extra modules](https://github.com/opencv/opencv_contrib). Unzip both folders, so that you now having two folders after decompression
```
opencv-main
opencv_contrib-main
```
Now assume you want to allow *darknet* to use *OpenCV* later, you should configure the path such as
```
/Users/YOURNAME/opt/darknet/opencv/opencv_contrib-master/modules
```
You should also to point to the python compiler correctly in the *appletf* conda environment setup earlier.
```
/Users/YOURNAME/opt/miniforge/envs/appletf/bin/python3
```
If you are happy with both paths, modify the following statements to include both of them
```
arch -arm64 cmake \
  -DCMAKE_SYSTEM_PROCESSOR=arm64 \
  -DCMAKE_OSX_ARCHITECTURES=arm64 \
  -DWITH_OPENJPEG=OFF \
  -DWITH_IPP=OFF \
  -D CMAKE_BUILD_TYPE=RELEASE \
  -D CMAKE_INSTALL_PREFIX=/usr/local \
  -D OPENCV_EXTRA_MODULES_PATH=/Users/YOURNAME/opt/darknet/opencv/opencv_contrib-master/modules \
  -D PYTHON3_EXECUTABLE=/Users/YOURNAME/opt/miniforge/envs/autotf/bin/python3 \
  -D BUILD_opencv_python2=OFF \
  -D BUILD_opencv_python3=ON \
  -D INSTALL_PYTHON_EXAMPLES=ON \
  -D INSTALL_C_EXAMPLES=OFF \
  -D OPENCV_ENABLE_NONFREE=ON \
  -D BUILD_EXAMPLES=ON ..
```
Note that *arch -arm64* is to tell your Mac silicon to compile using the ARM architecture (not x86_64). 

If you intend to use all 8 cores for OpenCV then setup with the statement 
```
arch -arm64 make -j8
```
The installation goes on with
```
arch -arm64 sudo make install
```
You may need to link cv2 to your environment
```
mdfind cv2.cpython
```
where you may find the satements like 
```
/usr/local/lib/python3.8/site-packages/cv2/python-3.8/cv2.cpython-38-darwin.so
```
go to the site-package in your *appletf* environment
```
cd /Users/luuquanghung/opt/miniforge/envs/appletf/lib/python3.8/site-packages
```
then link it with
```
ln -s /usr/local/lib/python3.8/site-packages/cv2/python-3.8/cv2.cpython-38-darwin.so cv2.so
```
This work is credited to the instruction of [Sayak Paul](https://sayak.dev/install-opencv-m1/).
