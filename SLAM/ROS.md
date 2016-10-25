# Lab5:SLAM #

- ## SLAM ##
  SLAM (simultaneous localization and mapping),即时定位与地图构建，也称为CML (Concurrent Mapping and Localization), 并发建图与定位。 
  SLAM问题可以描述为，机器人在未知环境中从一个未知位置开始移动,在移动过程中根据位置估计和地图进行自身定位,同时在自身定位的基础上建造增量式地图，实现机器人的自主定位和导航。


- ## ROS ##
  ROS(Robot Operating System），机器人操作系统，是一个机器人软件平台，它能为异质计算机集群提供类似操作系统的功能。ROS提供一些标准操作系统服务，例如硬件抽象，底层设备控制，常用功能实现，进程间消息以及数据包管理。ROS是基于一种图状架构，从而不同节点的进程能接受，发布，聚合各种信息（例如传感，控制，状态，规划等等）。ROS上有许多SLAM算法及对应的数据集，目前ROS主要支持Ubuntu操作系统。


- ## 安装ROS ## 
 1. Configure your Ubuntu repositories（配置Ubuntu的软件库）
    进入System Settings\Software & Updates,将Ubuntu Software选项卡下的"restricted"、 "universe" 和 "multiverse"选中 ，然后点击"Close"保存更改

    ![configure1](http://p1.bpimg.com/567571/bb88073a8c24bd22.jpg)

    ![configure2](http://p1.bpimg.com/567571/d611e5880ae15d83.jpg)

 2. Setup your sources.list（修改source.list文件）
     修改source.list文件，使得电脑能够在线更新packages.ros.org上的软件
           sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu 
         $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

 3. Set up your keys（加载公钥）
    使用未经验证的第三方软件需要手动加载公钥并更新列表
           sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 
         --recv-key 0xB01FA116
           sudo apt-get update
         
 4. 安装ROS Desktop-Full 
 
    `sudo apt-get install ros-jade-desktop-full`

 5. Initialize rosdep（初始化rosdep）
    在使用ROS之前，需要先初始化rosdep。rosde使得安装一些运行ROS核心部件所需要的源变得更加容易
         sudo rosdep init
         rosdep update

 6. Environment setup（设置环境变量）
         echo "source /opt/ros/jade/setup.bash" >> ~/.bashrc
         source ~/.bashrc

 7. Getting rosinstall（安装rosinstall）
    rosinstall是ROS常用的命令行工具，它让我们可以使用一条命令便可以下载ROS包的源树

    `sudo apt-get install python-rosinstall`
