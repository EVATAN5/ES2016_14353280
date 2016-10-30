# Deadlock.md

***
### 死锁的截图

![dl1](http://a2.qpic.cn/psb?/V12QVqkX2sMUU9/SZjmUOKp3P5yQjSyk2Leau9lJgKekqr.GVG.1WyFQGk!/m/dAkBAAAAAAAA&bo=qgHMAQAAAAADB0Q!&rf=photolist)

***
###产生死锁的4个必要条件

1. 互斥条件：一个资源每次只能被一个进程使用。
2. 请求与保持条件：一个进程因请求资源而阻塞时，保持对原有，已获得的资源的占有。
3. 不剥夺条件：资源申请者不能强行的从资源占有者手中夺取资源，资源只能由其占有者主动释放，即非抢占调度。
4. 循环等待条件：若干进程之间形成一种头尾相接的循环等待资源关系，比如说，存在一个进程等待队列{p1, p2, … pn}其中p1等待p2占有的资源，p2等待p3占有的资源, …, pn等待p1占有的资源，这样就形成了一个进程等待资源的环路。

***
###解释为何上述程序会产生死锁

首先看Deadlock函数

![dl2](http://a4.qpic.cn/psb?/V12QVqkX2sMUU9/KqTYuVnN0QfyxZhcqO2kLHzkIMok6U4TQnCH7j8sXT8!/m/dHcBAAAAAAAA&bo=rgFmAQAAAAADB.o!&rf=photolist)
![dl3](http://a1.qpic.cn/psb?/V12QVqkX2sMUU9/id7kDgStHPebBzyWU5hFyX1MFQk9lZP*O0IWEQ*Tg9Q!/m/dHwBAAAAAAAA&bo=6QF6AQAAAAADB7E!&rf=photolist)

1. 可以看到在构造函数中，新建了一个t线程。然后t.start()之后，线程t会被放到调度队列里面等待被调度。当调度它的时候，就会运行run函数。
2. 在run()中，调用了类B的实例b的methodB函数，参数为A的实例a，则又会调用a.last()函数。
3. 可以看到在t.start()函数后有个while循环，表示循环count时间这么就，经过count时间后就会执行下一句a.methodA(b)，同理这个函数会去执行b.last()函数。
4. 另外，在A,B类中所有函数都有synchronized关键字，表明当某一线程执行其中一个类的函数的时候，其他线程不能去访问这个类中的所有函数。
5. 通过上面的分析，可以知道，若是某一个时刻，t线程在执行b.methodB(a)，而主线程也在执行a.methodA(b)，那么此时t线程接着要去执行a.last()，由于主线程在执行A类的synchronized函数，所以线程t就会被阻塞；同理主线程要去执行b.last()，而t线程在执行B类的函数，所以主线程也会被阻塞。这样两个线程都被阻塞了，不抢占资源也不释放资源，那么此时就会产生死锁。
