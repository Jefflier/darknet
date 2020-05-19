![Darknet Logo](http://pjreddie.com/media/files/darknet-black-small.png)

# Darknet #
Darknet is an open source neural network framework written in C and CUDA. It is fast, easy to install, and supports CPU and GPU computation.

For more information see the [Darknet project website](http://pjreddie.com/darknet).

* [Requirements (and how to install dependecies)](#requirements)
* [Datasets](#datasets)

1.  How to compile on Linux * [Using make](#how-to-compile-on-linux-using-make)
2.  [How to use](#how-to-use-on-the-command-line)

### Requirements

* Linux
* **CMake >= 3.12**: https://cmake.org/download/
* **CUDA 10.0**: https://developer.nvidia.com/cuda-toolkit-archive (on Linux do [Post-installation Actions](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#post-installation-actions))
* **OpenCV >= 2.4**: use your preferred package manager (brew, apt), build from source using [vcpkg](download from [OpenCV official site](https://opencv.org/releases.html))
* **cuDNN >= 7.0 for CUDA 10.0** https://developer.nvidia.com/rdp/cudnn-archive (copy `cudnn.h`,`libcudnn.so`... as desribed here https://docs.nvidia.com/deeplearning/sdk/cudnn-install/index.html#installlinux-tar)
* **GPU with CC >= 3.0**: https://en.wikipedia.org/wiki/CUDA#GPUs_supported
* **GCC or Clang**

#### Datasets
* **2020年(第13届)中国大学生计算机设计大赛人工智能挑战赛：http://2020.jsjds.cn/AI/
 or just download (2020 02 data01.zip) at http://2020.jsjds.cn/AI/AI%202020%2002%20data01.zip

##### Examples of results

Others: 对数据集的全检测百度云盘链接：(https://pan.baidu.com/s/1JK5jpf-cWqxBkYBIF6_cxQ) 提取码：otwh

### How to compile on Linux (using `make`)

Just do `make` in the darknet directory.
Before make, you can set such options in the `Makefile`: [link](https://github.com/Jefflier/darknet/blob/master/Makefile#L1)

* `GPU=1` to build with CUDA to accelerate by using GPU (CUDA should be in `/usr/local/cuda`)
* `CUDNN=1` to build with cuDNN v5-v7 to accelerate training by using GPU (cuDNN should be in `/usr/local/cudnn`)
* `OPENCV=1` to build with OpenCV 4.x/3.x/2.4.x - allows to detect on video files and video streams from network cameras or web-cams
* `DEBUG=1` to bould debug version of Yolo
* `OPENMP=1` to build with OpenMP support to accelerate Yolo by using multi-core CPU

```
git clone https://github.com/pjreddie/darknet
cd darknet
make
````

#### How to use on the command line
Easy!
down pre-trained weights 
You already have the config file for YOLO in the cfg/ subdirectory. You will have to download the pre-trained weight file here My.weights(百度云盘链接：https://pan.baidu.com/s/1nKmRamIb5vmpeahd_gIWbw）  提取码：xw99 (46 MB). and just run this:
'./darknet detect cfg/my.cfg my.weights data/IM_0000.jpg'
You will see some output like this:

'''
layer     filters    size              input                output
    0 conv     32  3 x 3 / 1   416 x 416 x   3   ->   416 x 416 x  32  0.299 BFLOPs
    1 conv     64  3 x 3 / 2   416 x 416 x  32   ->   208 x 208 x  64  1.595 BFLOPs
    .......
  105 conv    255  1 x 1 / 1    52 x  52 x 256   ->    52 x  52 x 255  0.353 BFLOPs
  106 detection
truth_thresh: Using default '1.000000'
Loading weights from yolov3.weights...Done!
'''

默认情况下，YOLO仅显示置信度为0.25或更高的对象。您可以通过将-thresh <val>标志传递给yolo命令来更改此设置。例如，要显示所有检测，可以将阈值设置为0：
* `./darknet detect cfg/yolov3.cfg yolov3.weights data/dog.jpg -thresh 0`

网络摄像头上的实时检测
如果看不到结果，对测试数据运行YOLO并不是很有趣。不用在一堆图像上运行它，而是在网络摄像头的输入上运行它！

要运行此演示，您将需要使用CUDA和OpenCV编译Darknet。然后运行命令：

./darknet detector demo cfg/obj.data cfg/my.cfg my.weights
YOLO将显示当前的FPS和预测的类别，以及在其顶部绘制边框的图像。

您需要将网络摄像头连接到已编译安装OpenCV的计算机上，否则它将无法正常工作。如果您连接了多个网络摄像头，并且想要选择要-c <num>使用的网络摄像头0，则可以通过该标志进行选择（默认情况下，OpenCV使用网络摄像头）。

如果OpenCV可以读取视频，也可以在视频文件上运行它：
'''
./darknet detector demo data/obj.data cfg/my.cfg my.weights <video file>
 '''
这就是我们制作上面的视频的方式。
