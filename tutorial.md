# CARLA + ROS平台安装文档
## 1. 安装CARLA
### 1.1 推荐的系统配置
**操作系统** 64位的Windows 8或者Ubuntu 16.04及以上版本

**CPU** 4核Intel或AMD，2.5GHz及以上

**内存** 8GB RAM

**显卡要求** NVIDIA GTX 970及以上
> 显卡驱动必须正确安装
> 
> **关于AMD显卡**
> 
> * 部分CARLA分支需要用到NVIDIA-Docker
> * Ubuntu系统对AMD系列显卡驱动支持不足
> 
> **关于达不到要求的低配显卡**
> * 可以试一下，有些能跑，就比较卡，有些跑不了直接卡死......

**Python版本** Python2.7或Python3.7

> **本篇教程所用系统配置**
> * ubuntu 18.04.4-desktop
> * NVIDIA RTX2070 (驱动NVIDIA 440.100)
> * python 2.7 
> * pip 2.7


### 1.2 下载发行版CARLA
打开github发行版CARLA下载界面[Carla/download](https://github.com/carla-simulator/carla/blob/master/Docs/download.md)，选择一个CARLA版本进行下载

本篇教程以CARLA 0.9.9为例，下载[CARLA_0.9.9.4.tar.gz](https://carla-releases.s3.eu-west-3.amazonaws.com/Linux/CARLA_0.9.9.4.tar.gz)
> 推荐在windows系统上下载完成后复制到Ubuntu系统，下载速度更快

 新建文件夹CARLA_0.9.9，将tar.gz文件复制到该文件夹下，解压缩
```
mkdir ~/CARLA_0.9.9
cp <where is the package>/CARLA_0.9.9.4.tar.gz ~/CARLA_0.9.9/CARLA_0.9.9.4.tar.gz
cd ~/CARLA_0.9.9
tar -zxvf CARLA_0.9.9.4.tar.gz
```

### 1.3 启动CARLA服务端
切换到CARLA安装目录
```
cd ~/CARLA_0.9.9
```
启动CARLA服务端
```
./CarlaUE4.sh
```
![avatar](a1.png)

### 1.4 启动CARLA客户端
本篇教程以python 2.7为例

查看python和pip版本是否均为2.7
```
python -V
pip -V
```
![avatar](a2.png)

安装pygame和numpy两个python包
```
pip install --user pygame numpy
```

切换到CARLA安装目录
```
cd ~/CARLA_0.9.9/PythonAPI/examples
```
启动键盘控制车辆的Python脚本manual_control.py
```
python manual_control.py
```
![avatar](a3.png)

## 2. 安装ROS

ROS有多个版本，分别对应不同的linux发行版,本教程以安装ROS Melodic为例。
> **ROS版本与Ubuntu版本对照**
> 
> * ROS Kinetic -> Ubuntu 16.04
> * ROS Melodic -> Ubuntu 18.04
> * ROS Noetic -> Ubuntu 20.04。
> 
> 

> 安装具体步骤可参见 ROS Wiki上的[Ubuntu install of ROS Melodic](http://wiki.ros.org/melodic/Installation)

### 2.1 Ubuntu更改国内源
在安装软件之前，为了提升下载速度，我们先将Ubuntu的软件源改为国内的镜像。打开终端，首先备份原文件，输入命令
```
sudo mv /etc/apt/sources.list /etc/apt/sources.list.backup
sudo gedit /etc/apt/sources.list
```
在打开的编辑器中输入

```
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
```

这里我们选择的清华源，国内还有其他好的源，如中科大源、南科大源等。

> [清华Ubuntu镜像使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)
> 
> [南方科技大学Ubuntu镜像使用帮助](http://mirrors.sustc.us/help/ubuntu/)

保存更改之后，在终端输入命令，更新列表
```
sudo apt-get update
```

### 2.2 配置sources.list

向sources.list中添加ROS源。
> 官方ROS源下载速度比较慢，这里换成了清华的镜像源。
> 
> 也可以替换其他镜像源: [Mirrors](http://wiki.ros.org/ROS/Installation/UbuntuMirrors)

```
sudo sh -c '. /etc/lsb-release && echo "deb http://mirrors.tuna.tsinghua.edu.cn/ros/ubuntu/ `lsb_release -cs` main" > /etc/apt/sources.list.d/ros-latest.list'
```
### 2.4 添加密钥
```
sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
```
> 网上教程的密钥可能更新不及时，以[官方教程](http://wiki.ros.org/melodic/Installation/Ubuntu)为准。
### 2.5 安装ROS
更新软件源
```
sudo apt update
```
安装完整版的ROS
```
sudo apt install ros-melodic-desktop-full
```
### 2.6 设置环境变量
```
echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```
### 2.7 安装构建ROS软件包所需的依赖

到目前为止，运行核心ROS软件包所需的软件已经安装完成了。 要创建和管理ROS工作区，需要安装其他依赖关系
```
sudo apt install python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential
```

在使用许多ROS工具之前，需要初始化rosdep。 rosdep可以轻松地为要编译的源安装系统依赖性，同时rosdep也是ROS系统中某些核心组件运行所必须的。安装rosdep
```
sudo apt install python-rosdep
```
初始化rosdep
```
sudo rosdep init
rosdep update
```

### 2.8 测试ROS demo
运行roscore
```
roscore
```
若正常出现以下信息，说明已经成功安装
```
... logging to /home/sakamoto/.ros/log/3a5836a2-eb3a-11ea-8edc-dcfb484d8811/roslaunch-sakamoto-PC-4432.log
Checking log directory for disk usage. This may take a while.
Press Ctrl-C to interrupt
Done checking log file disk usage. Usage is <1GB.

started roslaunch server http://sakamoto-PC:46545/
ros_comm version 1.14.9


SUMMARY
========

PARAMETERS
 * /rosdistro: melodic
 * /rosversion: 1.14.9

NODES

auto-starting new master
process[master]: started with pid [4443]
ROS_MASTER_URI=http://sakamoto-PC:11311/

setting /run_id to 3a5836a2-eb3a-11ea-8edc-dcfb484d8811
process[rosout-1]: started with pid [4454]
started core service [/rosout]
```

新开一个terminal，运行以下命令，打开小乌龟窗口
```
rosrun turtlesim turtlesim_node
```
![avatar](a8.png)

新开一个terminal，运行以下命令，打开乌龟控制窗口，可使用方向键控制乌龟运动
```
rosrun turtlesim turtle_teleop_key
```
选中控制窗口的terminal，按方向键，可看到小乌龟窗口中乌龟在运动。
![avatar](a9.png)

新开一个terminal，运行以下命令，可以看到ROS的图形化界面，展示结点的关系
```
rosrun rqt_graph rqt_graph
```
![avatar](a10.png)

至此，ROS demo测试完成，说明ROS安装没有问题。

## 3. 安装CARLA-ROS
本教程以ROS Melodic为例

### 3.1 安装密钥，添加软件源
打开终端，运行以下命令
```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 81061A1A042F527D
sudo add-apt-repository "deb [arch=amd64 trusted=yes] http://dist.carla.org/carla-ros-bridge-melodic/ bionic main"
```
### 3.2 安装CARLA-ROS:

打开终端，运行以下命令
```
sudo apt update
sudo apt install carla-ros-bridge-melodic
```

编辑PYTHON路径，打开编辑器
```
gedit .bashrc
```
注释掉其他版本的carla-python-egg包，添加相对应的carla-python egg包，\<path-to-carla>请自行替换为CARLA安装路径，末尾添加
```
export PYTHONPATH=$PYTHONPATH:<path-to-carla>/PythonAPI/carla/dist/carla-0.9.9-py2.7-linux-x86_64.egg
```

### 3.3 启动CARLA-ROS demo
打开终端，切换到CARLA安装目录，用OpenGL启动CARLA:
```
cd CARLA_0.9.9
./CarlaUE4.sh -opengl
```

打开终端，初始化CARLA-ROS，运行手动控制车辆的demo
```
source /opt/carla-ros-bridge/melodic/setup.bash
roslaunch carla_ros_bridge carla_ros_bridge_with_example_ego_vehicle.launch
```
> 这里的手动控制脚本有些不同，在pygame的窗口，先按B键启动手动控制，然后才能用键盘WASD控制车辆

打开终端，初始化CARLA-ROS，可视化ros topic
```
source /opt/carla-ros-bridge/melodic/setup.bash
rosrun rviz rviz
```
选择File-Open Config，打开`/opt/carla-ros-bridge/melodic/share/carla_ros_bridge/config/carla_default_rviz.cfg.rviz`，在右侧Display窗口，在PointCloud2下Topic选择point_cloud即可生成点云图像
![avatar](a6.png)
![avatar](a7.png)

> rviz左侧两个Image窗口没有图像的原因是，这两个Image对应的Topic分别是image_depth和image_segmentation，而默认的ego vehicle没有添加深度摄像机和语义摄像机的传感器

> ego_vehicle对应的sensor文件在/opt/carla-ros-bridge/melodic/share/carla_ego_vehicle/config/sensor.json，如需添加新的sensor，请参考[Sensors Reference](https://carla.readthedocs.io/en/latest/ref_sensors/)



## 4. 参考教程 

[CARLA官方文档](https://carla.readthedocs.io/en/latest/)

[CARLA-ROS官方文档]([CARLA-ROS-Bridge](https://github.com/carla-simulator/ros-bridge))

[Windows10下安装Ubuntu双系统](https://blog.csdn.net/mtllyb/article/details/78586540)

[Ubuntu系统装崩了后彻底删除Ubuntu系统](https://blog.csdn.net/mtllyb/article/details/78635757)

[Ubuntu 18.04安装NVIDIA驱动](https://blog.csdn.net/tjuyanming/article/details/80862290)