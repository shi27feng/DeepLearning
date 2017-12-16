# Tensorflow Setup #
I use Python 3 instead of Python 2 because it is the future. Python 2 will be deprecated within the next few years.

## Install Python 3 Core Packages ##
```
sudo apt install python3-dev python3-pip python-virtualenv
```

## Install additional Data Science Packages ##
```
sudo apt install python3-matplotlib python3-numpy python3-pandas python3-scipy python3-sklearn
```

## Create a Virtual Environment ##
I use the official [Tensorflow Install Guide](https://www.tensorflow.org/install/install_linux) as basis.

- create a specific directory which contains one or more separate virtual environments (e.g. CPU version, GPU version, ...)
```
mkdir $HOME/virtualenvs
```
- create a virtual environment with the name "tensorflow"
```
virtualenv --system-site-packages -p python3 $HOME/virtualenvs/tensorflow
```

### Activate a Virtual Environment ###
- activate the virtual environment before you use the "pip" command to install stuff (e.g. a self compiled/packaged Tensorflow) or to run stuff (e.g. Tensorflow code) within the virtual environment
```
source $HOME/virtualenvs/tensorflow/bin/activate
```
- the prompt should look like this now
```
(tensorflow) steven@box:~$
```
- make sure "pip" is up to date
```
easy_install -U pip
```

### Install Tensorflow ###
- install the current official not optimized Tensorflow CPU build
```
pip3 install --upgrade tensorflow
```

### Deactivate a Virtual Environment ###
```
deactivate
```

## Create a Tensorflow Package from Source ##
### Install Compile related Packages ###
```
sudo apt install build-essential curl git libcurl3-dev python3-wheel
```

### Bazel ###
- needed to build/compile Tensorflow
- install Java 8
```
sudo apt install openjdk-8-jdk
```
- add the Bazel Repo
```
sudo vi /etc/apt/sources.list.d/bazel.list
```
- and the related content
```
deb [arch=amd64] http://storage.googleapis.com/bazel-apt stable jdk1.8
```
- get the related GPG key and add it
```
curl https://bazel.build/bazel-release.pub.gpg | sudo apt-key add -
```
- rescan repos and install Bazel
```
sudo apt update
sudo apt install bazel
```

### OpenCL Devel ###
- only needed if you want to compile an OpenCL version of Tensorflow
```
sudo apt install ocl-icd-opencl-dev opencl-headers
```

### ComputeCPP ###
- only needed if you want to compile an OpenCL version of Tensorflow
```
sudo mkdir /usr/local/computecpp
```
- download the latest computecpp version: http://computecpp.codeplay.com/downloads/computecpp-ce/latest/Ubuntu-16.04-64bit.tar.gz
- unpack it and copy/move the content to "/usr/local/computecpp"
- add "LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/computecpp/lib" to your environment setup because a OpenCL Tensorflow need to know where the computecpp libs are, i use the ".bashrc" file
```
vi $HOME/.bashrc
```
- and add
```
# computecpp
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/computecpp/lib
```
- source the ".bashrc" file
```
source $HOME/.bashrc
```
- check if it detects the OpenCL device, see [AMD_OpenCL_Setup](https://github.com/AlphasCodes/DeepLearning/blob/master/AMD_OpenCL_Setup.md) for setting up AMD OpenCL
```
/usr/local/computecpp/bin/computecpp_info
```
- the result should be something like this, it depends on the used GPU and driver version
```
steven@box:~$ /usr/local/computecpp/bin/computecpp_info
********************************************************************************

ComputeCpp Info (CE 0.5.0)

********************************************************************************

Toolchain information:

GLIBC version: 2.26
GLIBCXX: 20160609
This version of libstdc++ is supported.

********************************************************************************


Device Info:

Discovered 1 devices matching:
  platform    : <any>
  device type : <any>

--------------------------------------------------------------------------------
Device 0:

  Device is supported                     : UNTESTED - Untested OS
  CL_DEVICE_NAME                          : Carrizo
  CL_DEVICE_VENDOR                        : Advanced Micro Devices, Inc.
  CL_DRIVER_VERSION                       : 2527.3
  CL_DEVICE_TYPE                          : CL_DEVICE_TYPE_GPU 

If you encounter problems when using any of these OpenCL devices, please consult
this website for known issues:
https://computecpp.codeplay.com/releases/v0.5.0/platform-support-notes

********************************************************************************
```

## Tensorflow CPU Build ##
You can select another location if you want. Then you have to change the commands/script accordingly.

- create a directory where you store the Tensorflow Git tree, is use "$HOME/opt/git"
```
mkdir -p $HOME/opt/git
```
- change the directory to "$HOME/opt/git"
```
cd $HOME/opt/git
```
- clone the repository
```
git clone https://github.com/tensorflow/tensorflow.git tensorflow
```
- change the directory to "tensorflow"
```
cd tensorflow
```
- check the available Tensorflow versions
```
git tag
```
- checkout/select the current version (e.g. 1.4.1)
```
git checkout v1.4.1
```
- configure the sources
```
./configure
```
- do the build, Bazel will use all your available CPU threads to compile the source code
- the vanilla version
```
bazel build --config=opt //tensorflow/tools/pip_package:build_pip_package
```
- or the MKL [Intel Math Kernel Library] version
```
bazel build --config=opt --config=mkl //tensorflow/tools/pip_package:build_pip_package
```
- package the compiled sources, the related *.whl file will appear in the "$HOME/Downloads" directory
```
bazel-bin/tensorflow/tools/pip_package/build_pip_package $HOME/Downloads
```
- clean up
```
bazel clean
```
- you can install the self compiled Tensorflow package via (remember to activate the virtual environment first)
```
pip3 install --upgrade <PATH_TO_TENSORFLOW_CPU_ONLY_WHL_FILE>
```

## Tensorflow AMD OpenCL ##
- change the directory to "$HOME/opt/git"
```
cd $HOME/opt/git
```
- clone the "dev/amd_gpu" branch of the https://github.com/lukeiwanski/tensorflow.git repository
```
git clone -b dev/amd_gpu https://github.com/lukeiwanski/tensorflow.git tensorflow-lukeiwanski-amdgpu
```
- change the directory to "$HOME/opt/git/tensorflow-lukeiwanski-amdgpu"
```
cd $HOME/opt/git/tensorflow-lukeiwanski-amdgpu
```
- configure the sources
```
./configure
```
- do the build, Bazel will use all your available CPU threads to compile the source code, the build will take about 3 times longer than a Tensorflow CPU only build
```
bazel build --config=opt --config=sycl //tensorflow/tools/pip_package:build_pip_package
```
- package the compiled sources, the related *.whl file will appear in the "$HOME/Downloads" directory
```
bazel-bin/tensorflow/tools/pip_package/build_pip_package $HOME/Downloads
```
- clean up
```
bazel clean
```
- now you can install it in an own virtual environment
```
virtualenv --system-site-packages -p python3 $HOME/virtualenvs/tensorflow-opencl
source $HOME/virtualenvs/tensorflow-opencl/bin/activate
easy_install -U pip
pip3 install --upgrade <PATH_TO_TENSORFLOW_OPENCL_WHL_FILE>
deactivate
```

## Validate a Tensorflow Setup ##
- change the directory to "$HOME/opt/git"
```
cd $HOME/opt/git
```
- clone the repository
```
git clone https://github.com/tensorflow/models tensorflow-models
```

### MNIST ###
- change the directory to "$HOME/opt/git/tensorflow-models/tutorials/image/mnist"
```
cd $HOME/opt/git/tensorflow-models/tutorials/image/mnist
```
- run "convolutional.py", the first run takes longer because it will download related files
```
python3 convolutional.py
```
