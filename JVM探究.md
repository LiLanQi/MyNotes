# JVM探究

1.JVM的位置

![image-20220505234645122](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220505234645122.png)

2. JVM体系结构

   ![image-20220505235103265](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220505235103265.png)

3. 类加载器

   作用：加载class文件

   ![image-20220505235941599](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220505235941599.png)

​	3.1 虚拟机自带的加载器

​	3.2 启动类（根）加载器（BOOT）

​	3.3 扩展类加载器（EXC）

​	3.4 应用程序加载器（APP）

​	3.5 百度：双亲委派机制-->APP--->EXC---->BOOT      1.类加载器收到类加载请求(new) 2.将这个请求向上委托给父类加载器去完成，一直向上委托，直到启动类加载器 3.启动类加载器检测是否能加载当前这个类，能加载就结束，使用当前的加载器，否则，抛出异常，通知子加载器进行加载 4.重复步骤3

4. Native关键字：凡是带了native关键字的，说明java的作用范围达不到了，会去调用底层C语言的库，会进入本地方法栈，调用本地方法接口 JNI，JNI作用：扩展Java的使用，融合不同的编程语言为Java所用！最初是C、C++，它在内存区域中专门开辟了一块标记区域：Native Method Stack，登记native方法，它最终执行的时候，加载本地方法库中的方法（通过JNI）

5. 方法区：方法区是被所有线程共享，所有字段和方法字节码以及一些特殊方法，如构造函数，接口代码也在此定义，简单说，所有定义的方法的信息都保存在该区域，此区域属于共享区间

   静态变量、常量、类信息（构造方法、接口定义）、运行时的常量池也存在方法区中，但是实例变量存在堆内存中，和方法区无关，方法区在堆中的元空间中

6. JVM类型：Sun公司的Hotspot、BEA的JRockit、IBM的j9VM，我们学习的都是Hotspot，在 Hotspot JVM 中，直接将本地方法栈和虚拟机栈合二为一

7. 堆

   - heap，一个JVM只有一个堆内存，堆内存的大小是可以调节的

   - 类加载器读取了类文件后，一般会把什么东西放到堆中？类、方法、常量、变量、保存我们所有引用类型的真实对象

   - 堆内存中还要细分为三个区域：

     - 新生区（伊甸园区）

     - 养老区

     - 永久区            

       ![image-20220506214600416](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220506214600416.png)

   					GC垃圾回收，主要是在伊甸园区和养老区

​						假设内存满了，会爆OOM（OutofMemoryError），堆内存不够

​						在JDK8以后，永久存储区改了个名字（元空间）

**新生区**

- 类：诞生和成长的地方，甚至死亡
- 伊甸园，所有的对象都是在伊甸园区new出来得
- 幸存者区(0,1)

**永久区**（这个区域长存内存的，用来存放JDK自带的Class对象，存储的是java运行时的一些环境或类信息，这个区域不存在垃圾回收，关闭VM虚拟机就会释放这个区域的内存）

- jdk1.6之前，永久代，常量池是在方法区
- jdk1.7：永久代，但是慢慢的退化了，去永久代，常量池在堆中
- jdk1.8之后：无永久代，常量池在元空间

![image-20220506221917374](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220506221917374.png)

元空间：逻辑上存在，物理上不存在，JDK1.8 之前是占用 JVM 内存，JDK1.8 之后直接使用物理内存

![image-20220507155456840](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220507155456840.png)

在一个项目中，突然出现了OOM故障，那么该如何排除~ 研究为什么出错~

- 能够看到代码的第几行出错：内存快照分析工具，MAT（eclipse），JProfiler（idea）
- Dubug，一行行分析代码！

MAT,JProfiler作用：

- 分析Dump内存文件，快速定位内存泄漏

- 获得堆中的数据

- 获得大的对象

  可以通过设置-Xms1m -Xmx8m -XX:+HeapDumpOnOutOfMemoryError，（输出DUMP文件，可以使用JProfiler查看）其中Xms为设置初始化内存分配大小（1/64），Xmx为设置最大分配内存，默认为1/4，还可以设置-XX:+PrintGCDetails打印GC信息

8. GC

   GC种类：轻GC,重GC

   GC算法：

   - 引用计数法
   - 复制算法
   - 标记清除法
   - 标记压缩
