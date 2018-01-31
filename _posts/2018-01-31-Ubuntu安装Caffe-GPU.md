---
layout: post
title: Ubuntu安装Caffe-GPU
key: 20180131
tags: 学习 环境搭建 ubuntu opencv 新手 caffe-GPU nvidia驱动 环境搭建 opencv
---

[TOC]

#### 写在前面

​	由于Nvidia对于Ubuntu的不够重视，使得NVIDIA显卡驱动在Ubuntu上的安装变得异常困难，问题多多，这也致使使用GPU进行Deeplearning的caffe变得遥不可及。琢磨了很久，装好了Ubuntu下的caffe-GPU，写一篇博客，作为给自己的备份，也作为分享给大家的资料。

------

# Ubuntu安装Caffe-GPU

  惯例声明配置环境

软件: `Ubuntu 16.04`、`CUDA 9.1.0` 、`cuDNN 7.0.5 、`opencv 3.4` 、`caffe 1` 、`CMake3.10.2`、`g++5.4.0`

硬件:`NVIDIA 965m ` 、`cpu i7-6700HQ`

## 一、安装依赖包

### 1.`ctrl+Alt+T`打开控制台

```
$ sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler

$ sudo apt-get install --no-install-recommends libboost-all-dev

$ sudo apt-get install libopenblas-dev liblapack-dev libatlas-base-dev

$ sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev

$ sudo apt-get install git cmake build-essential
```

### 2.再次逐条运行上述指令

出现像下图这种的，依赖安装成功

![依赖验证](https://raw.githubusercontent.com/KenRamzes/KenRamzes.github.io/master/MarkdownPhotos/Res/%E4%BE%9D%E8%B5%96%E9%AA%8C%E8%AF%81.png)

### 3.防止caffe配置出错，添加系统环境变量

打开配置文件

```
sudo gedit ~/.bashrc
```

在文件末尾添加

```
export LD_LIBRARY_PATH=/usr/lib/x86_64-linux-gnu:$LD_LIBRARY_PATH
```

```
export LD_LIBRARY_PATH=/lib/x86_64-linux-gnu:$LD_LIBRARY_PATH 
```



## 二、显卡驱动

### 1.ban掉nouveau

终端中运行：

```
$ lsmod | grep nouveau                  如果有输出则代表nouveau正在加载。
```

终端输入

```
$ sudo gedit /etc/modprobe.d/blacklist-nouveau.conf   
```

文件输入

```
blacklist nouveau option nouveau modeset=0 
```

保存

出现

```
** (gedit:4243): WARNING **: Set document metadata failed: 不支持设置属性 metadata::gedit-position
```

忽视并输入

```
sudo update-initramfs -u
```

再次输入

```
$ lsmod | grep nouveau
```

   查看是否禁用成功

### 2.官网下载最新版本驱动

先查到自己的显卡型号

[NVIDIA驱动下载](http://www.nvidia.cn/Download/index.aspx?lang=cn)

sudo service lightdm stop下载最新版本驱动

文件save在/Home下(关闭图形化界面方便查找)

### 3.卸载现有显卡驱动

```
sudo apt-get remove –purge nvidia*
```

### 4.关闭图形化界面(GUI)

按住`Ctrl+Alt+F1`进入非GUI界面(类似于终端全屏的界面)

输入用户名

输入密码

登录

输入

```
sudo service lightdm stop（关闭图形界面）
```

### 5.授予权限并打开安装包

输入

```
ls
```

查看/Home下的文件找到驱动包

```
sudo chmod a+x NVIDIA-Linux-x86_64-{版本号}.run
```

安装，同时只安装驱动文件，不安装OpenGL文件、不检查X服务、不检查nouveau

```
sudo ./NVIDIA-Linux-x86_64-{版本号}.run –no-opengl-files –no-x-check –no-nouveau-check
```

### 6.关闭图形化界面

安装完成输入

```
sudo service lightdm start
```

### 7.常见问题

安装过程会有英文，自己慢慢看。

或者有问`accept？`的输入accept问`countinue?`输入countinue,其余的一路输入`y`

显卡安装出现问题进不去界面等的问题，可以在grub应到界面进入高级系统选择recover模式，恢复驱动

### 8.验证安装

输入

```
nvidia-smi
```

出现下图则成功否则重新安装

![驱动验证](https://raw.githubusercontent.com/KenRamzes/KenRamzes.github.io/master/MarkdownPhotos/Res/驱动验证.png)

## 三、安装CUDA

### 1.官网下载

[CUDA 9.1 Download](https://developer.nvidia.com/cuda-downloads)

还是放在/Home里

### 2.关闭图形化界面

输入`Ctrl+Alt+F1`进入文本模式

登录用户并输入

```
sudo service lightdm stop
```

### 3.安装

输入

```
ls
```

查看cuda安装包

输入

```
sudo chmod 777 cuda_{版本号}_linux.run
```

```
sudo ./cuda_{版本号}_linux.run
```

打开~/.bashrc文件：

```
sudo gedit ~/.bashrc 
```

将以下内容写入到~/.bashrc尾部：

```
export  PATH=/usr/local/cuda-9.1/bin${PATH:+:${PATH}}

export  LD_LIBRARY_PATH=/usr/local/cuda9.1/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}123
```

### 4.验证

测试CUDA的samples 

```
cd /usr/local/cuda-9.1/samples/1_Utilities/deviceQuery 
```

```
make 
```

```
sudo ./deviceQuery 
```

如果显示一些关于GPU的信息，则说明安装成功。 

![CUDA验证](https://raw.githubusercontent.com/KenRamzes/KenRamzes.github.io/master/MarkdownPhotos/Res/CUDA验证.png)

### 5.常见问题

#### 5.1安装过程

执行此命令约1分钟后会出现 0%信息，此时长按回车键让此百分比增长，直到100%，然后按照提示操作即可，先输入 accept ，然后让选择是否安装 nvidia 驱动，若未安装则输入 “y”，若确保已安装正确驱动则输入“n”，剩下的选择则都输入“y”确认安装或确认默认路径安装。

安装失败直接输入`指令reboot`重启

#### 5.2测试过程

若出现CUDA和显卡驱动不兼容，则需要重装合适的显卡驱动，一般是因为驱动不是官方最新版

## 四、安装cuDNN

### 1.官网下载

[cuDNN下载](https://developer.nvidia.com/rdp/cudnn-download)

注册下载，注意和CUDA的**兼容性**问题

CUDA9.1就要下载cuDNN v7.0.5 Library for Linux

### 2.安装

官方教程

[cuDNN Install Guide](https://developer.nvidia.com/compute/machine-learning/cudnn/secure/v7.0.5/prod/Doc/cuDNN-Installation-Guide)

解压并进入解压后的文件夹，输入

```
$ sudo cp cuda/include/cudnn.h /usr/local/cuda/include
$ sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
$ sudo chmod a+r /usr/local/cuda/include/cudnn.h
/usr/local/cuda/lib64/libcudnn*
```

### 3.验证

输入

```
nvcc -V
```

![cudnn验证](https://raw.githubusercontent.com/KenRamzes/KenRamzes.github.io/master/MarkdownPhotos/Res/cudnn%E9%AA%8C%E8%AF%81.png)

### 4.常见问题

这部分比较简单没有太大问题，出错的话，就手动把相应文件拷贝到相应位置

## 五、安装OpenCV

### 1.Github Download

[OpenCV下载](https://opencv.org/releases.html)

我的是3.4

### 2.安装依赖

输入

```
sudo apt-get install cmake  
```

```
sudo apt-get install build-essential libgtk2.0-dev libavcodec-dev libavformat-dev libjpeg.dev libtiff4.dev libswscale-dev libjasper-dev 
```

### 3.cmake编译

```
mkdir my_build_dir
```

```
cd my_build_dir
```

```
cmake -D CMAKE_BUILD_TYPE=Release -D 
```

```
CMAKE_INSTALL_PREFIX=/usr/local ..
```

```
sudo make
```

```
sudo make install
```

### 4.验证

输入

```
pkg-config --modversion opencv
```

出现**版本号**说明安装成功

### 5.常见问题

出现

```
error1:lib/libopencv_videoio.so.3.1.0: undefined reference to `gst_app_sink_pull_sample'
collect2: error: ld returned 1 exit status
modules/video/CMakeFiles/opencv_test_video.dir/build.make:388: recipe for target 'bin/opencv_test_video' failed
```

找到"OpenCVCompilerOptions.cmake"文件

> ```
> add_extra_compiler_option(-Werror=non-virtual-dtor)
> 一行添加 #http://stackoverflow.com/questions/27828740/opencv-3-0-videoio-error
>   error2:GstMiniObjectClass’ does not name a type 
>   solution:sudo apt-get install libgstreamer1.0-dev  libgstreamer-plugins-base1.0-dev 
> ```

## 六、安装caffe

### 1.Github clone

```
git clone https://github.com/BVLC/caffe.git
```

### 2.Makefile.config配置

把caffe文件夹里的`Makefile.config.example`重命名为`Makefile.config`

重要配置一：

> cuDNN acceleration switch (uncomment to build with cuDNN).
>  USE_CUDNN := 1

重要配置二：

按照版本`#`注释掉相应的段落即可

> CUDA architecture setting: going with all of them.
>
> For CUDA < 6.0, comment the *_50 through *_61 lines for compatibility.
>
> For CUDA < 8.0, comment the *_60 and *_61 lines for compatibility.
>
> For CUDA >= 9.0, comment the *_20 and *_21 lines for compatibility.
>
> CUDA_ARCH :=   #-gencode arch=compute_20,code=sm_20 \
> 	       #-gencode arch=compute_20,code=sm_21 \
> 		-gencode arch=compute_30,code=sm_30 \
> 		-gencode arch=compute_35,code=sm_35 \
> 		-gencode arch=compute_50,code=sm_50 \
> 		-gencode arch=compute_52,code=sm_52 \
> 		-gencode arch=compute_60,code=sm_60 \
> 		-gencode arch=compute_61,code=sm_61 \
> 		-gencode arch=compute_61,code=compute_61

### 3.make编译

输入

```
make all
```

不出意料，一定会出错

### 4.常见问题

#### 4.1：src/caffe/common.cpp:2:26: fatal error: glog/logging.h: 没有那个文件或目录

修改Makefile.config配置路径:
INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include
LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib
改为:
INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial
LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu/hdf5/serial

#### 4.2：src/caffe/net.cpp:8:18: fatal error: hdf5.h: 没有那个文件或目录

解决：在**Makefile.config**文件，添加/usr/include/hdf5/serial/ 到 INCLUDE_DIRS，也就是把下面第一行代码改为第二行代码。

把

```
INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include
```

改为

```
INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial/
```

在**Makefile**文件，把 hdf5_hl 和hdf5修改为hdf5_serial_hl 和 hdf5_serial，也就是把下面第一行代码改为第二行代码。

把

```
LIBRARIES += glog gflags protobuf boost_system boost_filesystem m hdf5_hl hdf5
```

改为

```
LIBRARIES += glog gflags protobuf boost_system boost_filesystem m hdf5_serial_hl hdf5_serial
```

#### 4.3：/home/master/caffe-master/include/caffe/blob.hpp:9:34: fatal error: caffe/proto/caffe.pb.h: 没有那个文件或目录

解决：用protoc从caffe/src/caffe/proto/caffe.proto生成caffe.pb.h和caffe.pb.cc

```
wuliwei@wulw:~/caffe/src/caffe/proto$ protoc --cpp_out=/home/wuliwei/caffe/include/caffe/ caffe.proto
在 /caffe/include/caffe/ 新建proto文件夹，复制caffe.proto。
```

#### 4.4:.build_release/lib/libcaffe.so：对‘cv::imread(cv::String const&, int)’未定义的引用

在Makefile文件的第173行，添加 opencv_core opencv_highgui opencv_imgproc opencv_imgcodecs
变为

```
LIBRARIES += glog gflags protobuf boost_system boost_filesystem m hdf5_serial_hl hdf5_serial opencv_core opencv_highgui opencv_imgproc opencv_imgcodecs
```

### 5.test

输入

```
sudo make runtest -j8
```

没关系一定会出错

形如这种的错误

####  libcudnn.so.7: cannot open shared object file: No such file or directory

执行复制粘贴任务就行

```
sudo cp /usr/local/cuda-9.1/lib64/{不能打开的文件} /usr/local/lib/{不能打开的文件} && sudo ldconfig 
```

比如上面`libcudnn.so.7`就是一个不能打开的文件，把文件名输入到大括号并执行命令。

error while loading shared libraries: libcudart.so.8.0: cannot open shared object file: No such file。

这个就是缺少`libcudart.so.8.0`也是缺少文件，还是复制相应文件到指定文件夹

再编译

输入

```
make all
```

下图为运行成功

![caffe运行](https://raw.githubusercontent.com/KenRamzes/KenRamzes.github.io/master/MarkdownPhotos/Res/caffe运行.png)

#### 

