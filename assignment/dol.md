# DOL.md
## DOL实例

***
### example1&2的dot图

![example1dot](http://a4.qpic.cn/psb?/V12QVqkX2sMUU9/Rp6KQdy5U3eOu28QN2VkNNntvpqQ4E2eZLqKpBlyTLc!/m/dFsBAAAAAAAA&bo=AALaAQAAAAAFB*0!&rf=photolist)

上图为example1的dot图，有三个模块：生产者，平方，消费者，和两个通道：c1，c2。即从生产者进程中读入数据传到平方进程中进行平方处理（实际修改后，为立方处理），再把数据传到消费者进程中去。

![exampel2dot](http://a4.qpic.cn/psb?/V12QVqkX2sMUU9/WJ*eTu*r0behOsP5rD9ru2e5sX*Oyob9cwRj0roeZJY!/m/dAMBAAAAAAAA&bo=*AJmAQAAAAAFB70!&rf=photolist)

上图为example2的dot图，相对于exampe1来说，它多了一个平方进程，也就是说最后输入到消费者进程的数据为生产者进程输出的数据的4次方。

***
### 修改要求及过程

* 修改example1，使其输出三次方数。

	1. 原始代码中，实现的是输出二次方数。实现的代码是在square.c文件中，可以看到其中的square_fire信号处理函数里面，读入了输入端的信号i，然后令i=i*i，然后再输出i，结果就是平方的了。
	
	2. 因此，要使其输出三次方的数的话，只需将平方过程改为三次方。更改的代码部分以及运行结果图如下：
	![example1change](http://a1.qpic.cn/psb?/V12QVqkX2sMUU9/VSNcUocmHlle.XjnJE*8iRPzRSaAnCcmYbS6nedHHUg!/m/dHwBAAAAAAAA&bo=vwE0AAAAAAADB6g!&rf=photolist)
	![example1](http://a4.qpic.cn/psb?/V12QVqkX2sMUU9/N0LW*SGes4.5i0t3*YV31lmTQHg43ypY0K.BzT3Nvx0!/m/dMcAAAAAAAAA&bo=zgLMAQAAAAAFByU!&rf=photolist)
	
* 修改example2，让3个square模块变为2个。

	1. 可以从文件下看到只有一个square.c文件，所以要从.xml文件中找为什么会有3个square模块。
	
	2. 在.xml文件的开头就定义了一个value为3，之后通过迭代来建立连接，3就是迭代次数，最后就连接了3个square模块。
	
	3. 所以我们要使square模块变为2个的话，只需要将迭代次数减为2即可。更改的代码以及运行结果如下：
	![example2change](http://a3.qpic.cn/psb?/V12QVqkX2sMUU9/kOG98vamfHPQ*TCchbUKa4cvlCS4UYy4FhcumnAW91w!/m/dHIBAAAAAAAA&bo=GgEWAAAAAAADBy8!&rf=photolist)
	![example2](http://a3.qpic.cn/psb?/V12QVqkX2sMUU9/wiysW43.r1ILOHlUPc5nUgnbF7ClWV7BqkcA**GrkaI!/m/dMYAAAAAAAAA&bo=0ALKAQAAAAAFBz0!&rf=photolist)
	
***
### 实验感想
1. 在分析一个dol实例的时候，最先要看的是它的dot图（需要先运行）和src文件夹中有什么文件。通过dot图可以大致了解这个实例的结构，更好的去分析代码；src中的每一个.c,.h文件就对应图中的一类模块，要了解一个模块干了些什么事就需要去读这些.c,.h文件。
2. 在这些c代码文件中，要注意的函数是xxx_init(初始化函数，xxx为文件名)，xxx_fire(信号产生函数，不可缺少，主要功能都写在这里面)。另外我觉得要注意参数:PORT_OUT和PORT_IN,前者表示输出端口，通常搭配写函数来用(DOL_write)，即写到输出端，表示此模块有输出；后者为输入端口，通常和读的函数用在一起(DOL_read)。
3. .xml文件里面定义了模块与模块之间是怎么连接的。其中，process是进程的定义，定义了它实现的模块的名字，有多少个端口及端口的类型等；sw_channel是通道定义，定义了通道的类型大小名字，连接的端口名字等；connection就是定义模块之间的连接，注意的是连接两个模块的一条线是需要定义2个connection的，标签origin表示的是connection的出发位置，标签target即是达到位置。
