# Hung Q. Luu
Some useful tips.

### Apple silicon Mac
Apple silicon-based products released in the end of 2020 are the game-changer. Mac computers with Apple silicon can run much faster than the Intel-based competitors for the same configuration. More interestingly, you can use your Mac for data science and machine learning tasks. I have configured my Mac Mini M1 to run deep learning tasks as follows.

#### Install Tensorflow on Mac M1

Apple did release a hardware-accelerated TensorFlow and TensorFlow Addons for macOS 11.0+, which supports Tensorflow r2.4rc0 and TensorFlow Addons 0.11.2. It requires Python 3.8 which can be installed via [XCode Command Line Tool](https://developer.apple.com/download/more/?=command%20line%20tools) (you need an Apple Developer account to install XCode). Please check out the nice instruction of [Fabrice Daniel](https://towardsdatascience.com/tensorflow-2-4-on-apple-silicon-m1-installation-under-conda-environment-ba6de962b3b8).

*Step 1*: Grab the most recent Tensorflow version from Apple team in GitHub:
[https://github.com/apple/tensorflow_macos/releases](https://github.com/apple/tensorflow_macos/releases). 

Unfold Assets tab [!](images/apple-tensorflow-assets.png) and select the complete version [tensorflow_macos-0.1alpha2.tar.gz](https://github.com/apple/tensorflow_macos/releases/download/v0.1alpha2/tensorflow_macos-0.1alpha2.tar.gz). Unzip it to your download folder.

