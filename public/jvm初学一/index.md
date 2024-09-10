# JVM初学（一）


首先上一个经典JVM图，来自[JavaGuide](https://javaguide.cn/java/jvm/memory-area.html)

&lt;img src=&#34;/image/jvm.png&#34; width = 80%&gt;

这一堆东西看的脑壳疼很久了。。。。

## 先讲线程私有部分

### 程序计数器（PC）

它是唯一一个不会出现OOM的区域，因为生命周期固定会随线程结束死亡（大概？

### 虚拟机栈

&lt;img src=&#34;/image/vstack.png&#34; width = 30%&gt;

由栈帧构成，里面存了操作数，返回地址，局部变量（也就是方法调用能用到的一切东西）。当方法创建时栈帧压入，销毁（return）时，栈帧弹出。

#### 动态链接

简单来说就是个指针指向方法真的在的地方。

下面是复杂的……

主要服务一个方法需要调用其他方法的场景。在 Java 源文件被编译成字节码文件时，所有的变量和方法引用都作为符号引用（Symbilic Reference）保存在 Class 文件的常量池里。当一个方法要调用其他方法，需要将常量池中指向方法的符号引用转化为其在内存地址中的直接引用。动态链接的作用就是为了将符号引用转换为调用方法的直接引用。

&lt;img src=&#34;/image/jvmlink.png&#34; width=&#34;80%&#34;&gt;

#### StackOverFlowError

当虚拟机栈内存大小无法动态扩展时，栈帧压冒漾了。

#### **OutOfMemoryError（OOM）**

当虚拟机栈还是疯狂压栈帧，但是可以动态扩展内存，但是扩展也没处扩展了的时候，就oom了。

### 本地方法栈

蒙圈了吧，这玩意和虚拟机栈好像啊。

确实，其他的都一模一样，除了这个东西提供的是Native方法服务，不是Java方法。

啊？**啥叫Native方法？**

看着玄乎其实就是其他语言的方法接口……

## 进程里线程共享的部分

### GC

见了好多遍这个词，没想到是**Garbage Collect**，垃圾收集……

### （GC）堆

感觉有点过分，这就是1.8后运行时jvm剩下的唯一部分了，**此内存区域的唯一目的就是存放对象实例，几乎所有的对象实例以及数组都在这里分配内存。**

虽然这么说，但垃圾收集器大部分工作都是管理这，所以也被称为垃圾堆。

#### 关于垃圾回收

Java的垃圾回收机制感觉和那个操作系统的时间片轮转异曲同工吧，就是到年龄了就回收

还是看一下1.8前后的变化:
&lt;img src=&#34;/image/jdk8.png&#34; width=&#34;80%&#34;&gt;

1. 新生代内存(Young Generation)
    
2. 老生代(Old Generation)：中间这个
    
3. 永久代(Permanent Generation)：下面这个，1.8改为元空间了，可以回顶部看一眼，元空间在本地内存里，使用直接内存（后面再说）。
    

小对象会直接出生在Eden区，要是塞不下了就Minor GC（新生代收集）再往S区塞，S不行就往老年区塞。不过一般情况下是熬过一定的MinorGC（有一个动态年龄计算）再往老年塞。

而大对象（比如：字符串、数组，需要连续大量内存）和长期存活的对象就直接往老年区塞了。

##### GC 分类

部分收集 (Partial GC)：

- 新生代收集（Minor GC / Young GC）：只对新生代进行垃圾收集；
    
- 老年代收集（Major GC / Old GC）：只对老年代进行垃圾收集。需要注意的是 Major GC 在有的语境中也用于指代整堆收集；
    
- 混合收集（Mixed GC）：对整个新生代和部分老年代进行垃圾收集。
    

整堆收集 (Full GC)：收集整个 Java 堆和方法区。

##### 空间分配担保

因为清理年轻的会增加老的，所以清理年轻的之前先看一下老的空间够不够大（不够大就full gc）

##### 死亡对象判断

###### 引用计数法

没有引用的时候就判死缓

###### 可达性分析算法

引用了但永远也无法到达的地方……死缓

为什么是死缓（被弃用了就是）

&lt;img src=&#34;/image/1.1.png&#34; width=&#34;80%&#34;&gt;

### 堆的OOM

堆这里最容易出现的就是 OutOfMemoryError 错误，并且出现这种错误之后的表现形式还会有几种，比如：

1. **java.lang.OutOfMemoryError:** **GC** **Overhead Limit Exceeded** ： 当 JVM 花太多时间执行垃圾回收并且只能回收很少的堆空间时，就会发生此错误。
    
2. **java.lang.OutOfMemoryError: Java** **heap** **space** :假如在创建新的对象时, 堆内存中的空间不足以存放新创建的对象, 就会引发此错误。(和配置的最大堆内存有关，且受制于物理内存大小。最大堆内存可通过-Xmx参数配置，若没有特别配置，将会使用默认值，详见：[Default Java 8 max heap sizeopen in new window](https://stackoverflow.com/questions/28272923/default-xmxsize-in-java-8-max-heap-size))
    
3. ......

---

> Author: Deequoique  
> URL: http://localhost:1313/jvm%E5%88%9D%E5%AD%A6%E4%B8%80/  

