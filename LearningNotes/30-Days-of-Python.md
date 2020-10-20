#Python30天学习
##概念
编译型语言：一次性将所有程序编译成二进制文件
- 缺点:开发效率低，不能跨平台
- 优点：运行速度快
- 语言：C，C++，Go等

解释型语言:当程序执行时，一行一行的解释
- 优点:开发效率高，开源跨平台
- 缺点：运行速度慢
- 语言：python，php,Perl等等

强类型定义语言：就是说一旦一个变量被指定了某个数据类型，如果不经过强制转换，那么就是这个数据类型了
弱类型定义语言: 数据类型可以被忽略的语言

python是一门动态解释型的强类型定义语言。

在计算机中执行python程序，是通过Python解释器将python语言翻译成计算机CPU能够听懂的机器指令语言。

目前Python的主要应用领域：
- 云计算，典型应用openstack
- web开发：典型WEB框架Django
- 系统运维
- 科学运算，人工智能
- 金融

Python的优点:
- 开发效率高：python有非常强大的第三方库，可以实现任何功能
- 高级语言：
- 可移植性
- 可扩展性
- 可嵌入性

Python的缺点：
- 速度慢：python的运行速度较C语言和java来说比较慢
- 代码不能加密：源码都是以明文形式存放
- 线程不能利用多CPU问题


Python的几种解释器:
- CPython:默认的python实现.。CPython是用C语言写的，当执行代码的时候Pythond代码会被转化成字节码（bytecode）。所以CPython是个字节码解释器。
- PyPy:很多地方都和CPython很像的实现，但是这个解释器本身就是由Python写成.PyPy被认为要比CPython性能更好。因为CPython会把代码转化成字节码，PyPy会把代码转化成机器码。
- Jython:是用java实现的一个解释器。Jython允许程序员写Python代码，还可以把java的模块加载在python的模块中使用。Jython使用了JIT技术，也就是说运行时Python代码会先转化成Java 字节码（不是java源代码），然后使用JRE执行。程序员还可以用Jython把Python代码打成jar包，这些jar和java程序打包成的jar一样可以直接使用。这样就允许Python程序员写Java程序了。
- Cython:允许把Python代码转化成C/C++代码或者使用各种各样的C/C++模块/文件的实现。换句话说，Cython是C/C++ 和Python的一个桥梁。Cython也是Python的一种方言。开发者也可以使用Cython来执行Python脚本，并且执行效率比CPython更快。另外，开发者可以写一个Python脚本，使用Cython来编译成（linux上.so 或者是Windows上的.dll）类库，然后当作一个Python模块来使用。

GIL是在实现Python解释器(CPython)时所引入的一个概念。GIL不是Python的特性，Python完全可以不依赖于GIL.为什么会有GIL?Python最初的设计理念在于，为了解决多线程之间数据完整性和状态同步的问题，设计为在任意时刻只有一个线程在解释器中运行。而当执行多线程程序时，由GIL来控制同一时刻只有一个线程能够运行。即Python中的多线程是表面多线程，也可以理解为fake多线程，不是真正的多线程(并发：不同的代码块交替执行;并行：不同的代码块同时执行)。为什么要保证同一时刻只有一个线程在解释器中运行呢？答案是为了Python解释器中原子操作的线程安全。

由于GIL锁存在，python里一个进程永远只能同时执行一个线程(拿到GIL的线程才能执行)，这就是为什么在多核CPU上，python的多线程效率并不高。分类别讨论一下：
1. CPU密集型代码(各种循环处理、计数等等)，在这种情况下，ticks计数很快就会达到阈值，然后触发GIL的释放与再竞争（多个线程来回切换当然是需要消耗资源的），所以python下的多线程对CPU密集型代码并不友好。
2. IO密集型代码(文件处理、网络爬虫等)，多线程能够有效提升效率(单线程下有IO操作会进行IO等待，造成不必要的时间浪费，而开启多线程能在线程A等待时，自动切换到线程B，可以不浪费CPU的资源，从而能提升程序执行效率)。所以python的多线程对IO密集型代码比较友好。

在Python3.x中，GIL不适用ticks计数，改为使用计时器(执行时间达到阈值后，当前线程释放GIL)，这样对CPU密集型程序更加友好,但依然没有解决GIL导致的同一时间只能执行一个线程的问题，所以效率依然不尽如人意。

老手经常说的一句话:"python下想要充分利用多核CPU，就用多进程"，因为每个进程有各自独立的GIL，互不干扰，这样就可以真正意义上的并行执行，所以在python中，多进程的执行效率优于多线程(仅仅针对多核CPU而言)。

如何避免GIL影响：
1. CPU密集型下的任务尽量采用多进程处理
2. 如果你不想使用Cython解释器，就没有这个限制，同样很多Cython的特性你也放弃了。
3. 利用 ctypes 绕过 GIL.ctypes会在调用C函数前释放GIL,可以通过ctypes和C动态库来让 python充分利用物理内核的计算能力。


GIL参考文献：https://blog.csdn.net/zshluckydogs/article/details/81986649?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param

## 数据类型
