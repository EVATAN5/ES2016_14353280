#安装配置ROS

***

1. 配置ubuntu的软件源

	配置Ubuntu要求允许接受restricted、universe和multiverse的软件源
	
	[***https://help.ubuntu.com/community/Repositories/Ubuntu***](https://help.ubuntu.com/community/Repositories/Ubuntu)
2. 添加软件源到source.list
    
    $ sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu trusty main" > /etc/apt/sources.list.d/ros-latest.list'
    
    添加了软件源之后，系统就知道去哪里下载程序了。
   
3. 设置密钥

	$ wget http://packages.ros.org/ros.key -O - | sudo apt-key add -

4. 安装
	
	* 首先要确保自己的Debian数据包是最新的：$ sudo apt-get update
	
	* 安装ROS的所有函数库与工具：$ sudo apt-get install ros-indigo-desktop-full
	
5. 初始化rosdep

	在用ROS之前先初始化rosdep，能更方便的安装一些系统依赖程序包，还有一些ROS的主要部件的运行也需要它。
	
	$ sudo rosdep init
	
	$ rosdep update
	
	如果已经安装了ROS了，只是想要改变当前shell的环境变量，就可以用命令：
	
	$ source /opt/ros/jade/setup.bash
	
6. 设置环境

	添加ROS的环境变量，ROS环境变量会自动添加到bash中去当打开新的shell的时候。
	
	$ echo "source /opt/ros/indigo/setup.bash" >> ~/.bashrc
	
	$ source ~/.bashrc
	
7. 安装rosinstall

	rosinstall是在ROS中使用非常频繁的命令，用这个命令可以很容易去下载许多ROS数据包。
	
	$ sudo apt-get install python-rosinstall
	
***

####检验是否安装成功

1. 输入命令$ roscore

2. $ rosrun turtlesim turtlesim_node

	会出来如下的乌龟：![11](http://a2.qpic.cn/psb?/V12QVqkX2sMUU9/7qW8gYVs8zDE9q5Jg8pIGsXhA7CBR*cesYtHzSHIaBU!/m/dI0BAAAAAAAAnull&bo=XQBPAAAAAAADBzA!&rf=photolist&t=5)

3. $ rosrun turtlesim turtle_teleop_key
	
	此步会使得乌龟运动：![22](http://a3.qpic.cn/psb?/V12QVqkX2sMUU9/KkpVmott7AGg8Xkxg89Jc5bJSd*QbO5JHZMKwmfqwV4!/m/dAoBAAAAAAAAnull&bo=wwCsAAAAAAADB00!&rf=photolist&t=5)

4. $rosrun rqt_graph rqt_graph

	最后会出现下图![33](http://a4.qpic.cn/psb?/V12QVqkX2sMUU9/eJeAJDweA0d2me.xXZZYnvHobvDHYU1oDuDNU8dzljI!/m/dGcBAAAAAAAAnull&bo=PQOJAQAAAAADB5Q!&rf=photolist&t=5)
