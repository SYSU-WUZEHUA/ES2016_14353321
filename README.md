# linux环境下DOL的配置 #

- ## Description(DOL 框架描述) ##

 DOL(The distributed operation layer)，分布式操作层，是一个程序的并行应用软件开发框架。DOL允许指定基于计算Kahn进程网络模型的应用范围和特点，基于SystemC仿真引擎。此外，DOL提供了基于XML规范的格式来描述一个多处理器系统上的并行应用程序的实现，包括结合和映射



- ## How to install(DOL 安装笔记) ##

 1. 本次实验使用虚拟机VMWARE安装Ubuntu14.04构建linux环境，虚拟机和Ubuntu14.04的下载链接以及安装教程如下：

     - VMware下载 [http://www.vmware.com/products/player/playerpro-evaluation.html](http://www.vmware.com/products/player/playerpro-evaluation.html "VMware下载")

     - VMware的安装步骤
[http://jingyan.baidu.com/article/d169e18657515b436611d8a6.html](http://jingyan.baidu.com/article/d169e18657515b436611d8a6.html "VMware的安装步骤")

     - Ubuntu下载
[http://www.ubuntu.com/download/desktop](http://www.ubuntu.com/download/desktop "Ubuntu下载")

     - Vmware安装Ubuntu14.04
[http://jingyan.baidu.com/article/0320e2c1ef9f6c1b87507bf6.html](http://jingyan.baidu.com/article/0320e2c1ef9f6c1b87507bf6.html "Vmware安装Ubuntu14.04")

 2. 在ubuntu中安装一些必要的环境

     - 更新源

          `sudo apt-get update`

          ![更新源](http://i.imgur.com/pG3kLWI.png)

          ![更新源结果](http://i.imgur.com/ti1gpdD.png)

     - 安装ant

          `sudo apt-get install ant`

         ![安装ant](http://i.imgur.com/tonX2nZ.png)

         ![安装ant](http://i.imgur.com/Cex8iNg.png)

         ![安装ant](http://i.imgur.com/majrGdD.png)

     - 安装openjdk

        `sudo apt-get install openjdk-7-jdk`

        ![安装openjdk](http://i.imgur.com/w1LaKZG.png)

        ![安装openjdk](http://i.imgur.com/RrGiHEt.png)

        ![安装openjdk](http://i.imgur.com/kAJDquP.png)

     - 安装unzip

        `sudo apt-get install unzip`

        ![安装unzip](http://i.imgur.com/EWxDSyY.png)

     - 安装g++

        ![安装g++](http://i.imgur.com/6DXyxys.png)

 3. 下载dol_ethz.zip和systemc-2.3.1.tgz并将其拷贝到虚拟机中
     - dol_ethz.zip下载地址：
    [http://www.tik.ee.ethz.ch/~shapes/dol.html](http://www.tik.ee.ethz.ch/~shapes/dol.html "dol")
    
     - systemc-2.3.1下载地址：
   [http://www.accellera.org/downloads/standards/systemc/files](http://www.accellera.org/downloads/standards/systemc/files "systemc")

 4. 解压编译文件
     - 新建dol文件夹 ，将dolethz.zip解压到 dol文件夹中
    
           mkdir dol
           unzip dol_ethz.zip -d dol

       ![解压dol](http://i.imgur.com/0ygSkaf.png)
     - 解压systemc-2.3.1,解压后进入systemc-2.3.1的目录下;新建一个临时文件夹objdir并进入该文件夹，运行configure
			
			tar -zxvf systemc-2.3.1.tgz
			cd systemc-2.3.1
			mkdir objdir
			cd objdir
			../configure CXX=g++ --disable-async-updates`

       ![解压systemc](http://i.imgur.com/dYU7tG7.png)

       ![新建文件夹](http://i.imgur.com/V7RIPwA.png)

       ![configure结果](http://i.imgur.com/5cAK2LT.png)

     - 找到systemc的位置
     
          `pwd`
     
       ![systemc位置](http://i.imgur.com/iyGcrr2.png)

     - 用记事本打开build_zip.xml文件并修改systemc的位置

            <property name="systemc.inc" value="YYY/include"/>
            <property name="systemc.lib" value="YYY/lib-linux/libsystemc.a"/>

          其中YYY为systemc的位置

          ![build_zip.xml修改结果](http://i.imgur.com/OuqtJan.png)

     - 进入dol文件夹并编译dol
     
          `ant -f build_zip.xml all`
     
       ![编译dol](http://i.imgur.com/EHkFSgf.png)

       ![dol编译结果](http://i.imgur.com/GVBz5fI.png)

 5. 进入build/bin/mian路径下，运行第一个例子
 
         cd build/bin/main

         ant -f runexample.xml -Dnumber=1

     ![运行例子](http://i.imgur.com/5KyRmny.png)

     ![例子运行结果](http://i.imgur.com/ReWcByw.png)



- ## Experimental experience(实验感想、实验心得) ##
就像操作系统pintos环境的配置一样，DOL环境的配置与网络有很大的关系。已经记不清楚我自己从头到尾配置了多少次了，虚拟机版本的更换、ubuntu版本的反复更换，即便到了成功编译dol的情况下，最后运行第一个例子仍然会有fail的出现。配置成功的这一次是我早上8点在图书馆进行的，速度之快、过程之顺利简直到了不可置信的地步