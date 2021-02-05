# Hung Q. Luu
Some useful tips.

### Apple silicon Mac
Apple silicon-based products released in the end of 2020 are the game-changer. Mac computers with Apple silicon can run much faster than the Intel-based competitors for the same configuration. More interestingly, you can use your Mac for data science and machine learning tasks. I have configured my Mac Mini M1 to run deep learning tasks as follows.

#### Install Tensorflow on Mac M1

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

Unfold Assets tab ![Assets tab](images/apple-tensorflow-assets.png) and select the complete version [tensorflow_macos-0.1alpha2.tar.gz](https://github.com/apple/tensorflow_macos/releases/download/v0.1alpha2/tensorflow_macos-0.1alpha2.tar.gz). Unzip it to your download folder. The key thing is do not run the default installation given by Apple. It does not work as in my case. Note that *pip* should be installed before the following sub-steps as well.

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

#### Install OpenCV

OpenCV is a well-known library for computer vision orginally developed by Intel. We will build OpenCV in your Mac from scratch.

**Step 1: Install cmake** 
*cmake* is the tool that allow you to build OpenCV in your Mac. First, grab a Mac-based version from [cmake homepage](https://cmake.org/download/). Better to use the *unversal cmake version 3.19.4*  for Mac OS X 10.13 or later from [this link](https://github.com/Kitware/CMake/releases/download/v3.19.4/cmake-3.19.4-macos-universal.dmg)

After openning, drag and drop the file into Applications folder ![Applications folder](images/mac-applications-cmake.png) of your Mac.

To need to config *cmake* so that later on we can use it from the command line.
sudo "/Applications/CMake.app/Contents/bin/cmake-gui" --install

**Step 2: Build OpenCV** 

First, we download the latest version of OpenCV (4.5.1) in GitHub ![OpenCV from GitHub ](images/mac-applications-cmake.png) to your Mac. We need both OpenCV [main packages]() and their [extra modules](https://github.com/opencv/opencv_contrib). Unzip both folders, so that you now having two folders after decompression
```
opencv-main
opencv_contrib-main
```
