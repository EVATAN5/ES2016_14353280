# README.md
***
### DOL框架描述
DOL框架能够使得应用程序自动地映射到多处理架构平台上，也因此适合用于高效地编程并行应用程序。DOL提供了高性能分析，多任务映射优化，模拟仿真等功能。另外，DOL提供了xml的规范格式来描述多处理系统上的并行程序的实现。DOL框架主要包括三个部分：

* 应用程序编程接口：DOL定义了一组计算和通信的程序，使得编程分布式，应用程序能够并行化。这样，在没有关于底层架构的详细知识下，我们也能够编写程序。
* 功能仿真：功能仿真框架的建立是为了让程序员能够测试应用程序。
* 映射优化：计算出一组应用程序到多任务系统的最优映射。
***
### ubuntu下的DOL安装
1. 安装好ubuntu后，在其中安装一些必要的环境：

	$sudo apt-get update 更新软件源
	
	$sudo apt-get install ant 安装ant工具，一种基于Java的build工具
	
	$sudo apt-get install openjdk-7-jdk 安装jdk，其为Java的软件开发工具包
	
	$sudo apt-get install unzip 安装解压工具
2. 从网站上下载必须的文件：

	$sudo wget [http://www.accellera.org/images/downloads/standards/systemc/systemc-2.3.1.tgz](http://www.accellera.org/images/downloads/standards/systemc/systemc-2.3.1.tgz)
	
	$sudo wget [http://www.tik.ee.ethz.ch/~shapes/downloads/dol_ethz.zip](http://www.tik.ee.ethz.ch/~shapes/downloads/dol_ethz.zip)
3. 解压文件

	首先新建一个dol的文件夹来存放解压后的dol文件 $ sudo mkdir dol
	
	将dolethz.zip解压到dol文件夹中 $ sudo unzip dol_ethz.zip -d dol
	
	再解压systemc $ sudo tar -zxvf systemc-2.3.1.tgz
	
4. 编译systemc

	进入解压后的systemc-2.3.1的目录下 $ cd systemc-2.3.1
	
	新建一个文件夹objdir $ sudo mkdir objdir
	
	进入该新建的文件夹 $ cd objdir
	
	运行configure，进行环境测试 $ sudo ../configure CXX=g++ --disable-async-updates 运行成功后的截图如下
	
	![configure](http://a1.qpic.cn/psb?/V12QVqkX2sMUU9/oip5QEizViJ9l0nQ86kI17vgkNt*3OdPNA3Ssh.rDxc!/b/dKsAAAAAAAAA&bo=bwLmAAAAAAADB6k!&rf=viewer_4)
	
	进行编译 $ sudo make install 然后回到上级目录 $ cd .. 可以看到文件目录下多了文件include,lib-linux64(若是32位系统则是lib-linux)
	
	通过$ pwd命令得到当前所在的路径，并记录下来，例如我的路径为/home/parallels/systemc-2.3.1
	
	![path](http://a1.qpic.cn/psb?/V12QVqkX2sMUU9/5B*2DB9dRUaBsii.Yo45pigmMG*16HSRQN102hBm920!/b/dPYAAAAAAAAA&bo=hgEfAAAAAAADB7o!&rf=viewer_4)
	
5. 编译dol

	进入dol文件夹 $ cd ../dol
	
	修改build_zip.xml 由于直接打开此xml文件修改后不能够保存，需要用到命令 $ sudo gedit build_zip.xml 输入命令后，文件会自动打开，找到下面的这两句话：
		
	property name="systemc.inc" value=".../include"
	
	property name="systemc.lib" value=".../lib-linux/libsystemc.a"
	
	将上面省略的...部分修改为上一步中记录的工作路径（第二句话中，64为系统需要将lib-linux改为lib-linux64）
	
	![modify](http://a1.qpic.cn/psb?/V12QVqkX2sMUU9/7PHZEL7EEM9qAUYOvXCPM.EMuXfbRCMEiTtnZhbFjH8!/b/dLEAAAAAAAAA&bo=WQMgAAAAAAADB1g!&rf=viewer_4)
	
	进行编译 $ sudo ant -f build_zip.xml all 编译成功后如图所示会出现build successful
	
	![dol](http://a1.qpic.cn/psb?/V12QVqkX2sMUU9/VzxL5XtTSsaGxR2xeqjxKNYXsJZVT7rUBDOFHmgeDeA!/b/dPYAAAAAAAAA&bo=CgEzAAAAAAADBxo!&rf=viewer_4)
	
6. 测试

	进入build/bin/main路径下，运行第一个例子
	
	$ cd build/bin/main
	
	$ sudo ant -f runexample.xml -Dnumber=1
	
	成功的结果如图所示
	
	![success](http://a1.qpic.cn/psb?/V12QVqkX2sMUU9/H.vWMi538MGnum*V8pdRaADf3l4pGAU*UeQYCBSOdGk!/b/dLEAAAAAAAAA&bo=jwFWAQAAAAADB*s!&rf=viewer_4)
	
###### 自己遇到的问题以及解决办法和自己觉得需要注意的事项
1. 再修改文件build_zip.xml的时候，需要用命令$ sudo gedit build_zip.xml来修改保存文件，但我输入这条指令之后，会出现如下错误：

	No protocol specified
	
	这是linux终端启动图形化程序界面时会报的错，因为默认情况下不允许别的用户的图形程序的图形显示在当前屏幕上。解决方法是要切换身份前的用户执行如下命令，输入命令：
	
	$ sudo xhost +
	
	另外，在我自己的ubuntu上执行这条指令时如果没有加sudo是会报错的。执行这条指令后即可输入gedit指令了，发现文件会自动弹出打开并可以保存。
	
2. 在最后一步编译的时候失败了一次，但是看了error的信息之后很容易就发现了原因是因为自己的jdk的安装出了些问题，因为error中有清楚提到jdk，但具体是什么报错记不太清了，总之，重新装一遍jdk就可以了： 

	$ sudo apt-get install openjdk-7-jdk 
	
	之后回想起来也的确是有印象，当自己一开始装jdk的时候，装完了却有一个warning，大致意思是说有一些部分没有安装成功，当时想着之后有错再看所以就没管它。

3. 自己觉得在整个安装步骤中，能加上sudo的就加上sudo，不要省略掉了。自己在有些步骤中有省略掉sudo，结果都是有报错的，例如：
	![sudo](http://a2.qpic.cn/psb?/V12QVqkX2sMUU9/rlUvnQS3wSQ0lJu6RsCeo0D28i3cRKQORsCbe108qEs!/b/dK8AAAAAAAAA&bo=zgJWAAAAAAADB7g!&rf=viewer_4)但加上sudo后就能够成功编译了。
***
### 实验感想
* 当在实验过程出现问题后，要学会自己去查找资料解决。比如说当我遇到上面第一个问题的时候，想问别人怎么解决，但他们都没有遇到这个问题，所以只能自己上网去搜。其实网上遇到这个问题的人挺多的，也有关于这个问题的解释以及具体解决办法。最主要的是，在自己查询资料的过程中，自己还知道了其他的知识，例如linux中使用gedit时经常遇到的问题以及解决办法；另外，学会如何从报错的信息中找到重点去网上搜索解决办法，我觉得是一件挺重要的事情。
* 由于不经常用linux系统，所以一旦出现error，自己就很怕，不知道怎么找到哪里出了错。经过这次实验，觉得一旦没有出现预期的结果，首先找到error信息，有时候error信息不会出现在最下面，而在一大串文字的上面，然后在error信息中找到关键字，然后自己推测是哪里除了错，或者自己再根据关键字去网上搜索。
* 在做实验的时候，要知道每一步是干什么的，不能跟着实验步骤一步一步来却不思考，那实验就没有意义。在做完这个实验后至少是要知道我们安装了哪些文件或者工具，这些东西的作用是什么也要了解，脑海中大致清楚整个实验的过程步骤。
