> java类程序大部分情况下均需jvm虚拟机解释执行(除jit生成机器码和部分硬件内建支持),因此无论内存和性能都存在限制,但也正是解释执行,对系统而言安全性要更高,相较C/C++程序参数配置影响更大

> 参考资料基于Java SE 7 Edition

# 一. jvm内存模型
> 每个java类进程均为一个jvm实例    
* **java进程模型**  
![内存模型](https://github.com/PeterLee08/resources/blob/master/icon/png/java/JVM.png?raw=true)

1. class loader     将class文件加载到method area 
2. Execution engine 执行class指令,每个线程一个engine实例
3. Native interface 和本地库文件交互,进行系统调用


* **hotspot 对heap区和methodarea的实现**:   
> 为减小gc扫描消耗,堆区分成了新生代和老生代,永生代   
> 永生代主要存放代码元信息,常量信息

![分代实现](https://github.com/PeterLee08/resources/blob/master/icon/png/java/fendai.PNG?raw=true)

> 这块命名如下图(和上图反过来了):
![命名](https://github.com/PeterLee08/resources/blob/master/icon/png/java/name.PNG?raw=true)

* 新生代(Young Generation)：用于存放新创建的对象，采用复制回收方法，如果在s0和s1之间复制一定次数后，转移到年老代中。这里的垃圾回收叫做minor GC;
* 老生代(Old Generation)：这些对象垃圾回收的频率较低，采用的标记整理方法，这里的垃圾回收叫做 major GC。
* 永生代(Permanent Generation)：存放Java本身的一些数据，当类不再使用时，也会被回收

# 二. jvm垃圾回收
## 1). 生命周期
1. java 所有对象在伊甸园（Eden space）出生，也即是说当你的JAVA 程序运行时，需要创建新的对象，JVM 将在该区
为你创建一个指定的对象供程序使用。创建对象的依据即是永久存储区中的元数
据。 
2. 当 Eden 空间不足时，将该区被引用的对象移动到s0区，销毁其他对象
3. 当s0空间也不足时，将被引用对象对象移动到s1区.销毁其他对象
4. 当对象移动到S1区后,s0清空备用, eden再次不足时,移动到s1,s1不足时移动到s0,如此反复, [S0和S1大小等完全一致可以互换] 
4. 这样当移动到一定次数后,将年龄大的对象被移动到老生代,或者S0S1移动空间不足时,将引用对象移动到老生代
5. 当老生代空间不足时,销毁老生代不再引用对象,依旧不足,OOM

> 发生时机也可能在cpu空闲时,或者这jvm设置的定时触发

## 2). 回收策略
* 新生代的回收最为频繁,要回收的对象多要复制的少,采用复制回收，称为minor GC；
* 老生待对象垃圾回收的频率较低，每次回收都只回收少量对象, 采用的标记整理方法，这里的垃圾回收叫做 major GC


> Copying复制:它将可用内存按容量划分为大小相等的两块，每次只使用其中的一块。当这一块的内存用完了，就将还存活着的对象复制到另外一块上面，然后再把已使用的内存空间一次清理掉，这样一来就不容易出现内存碎片的问题   
> Mark-Sweep（标记-清除）: 标记阶段和清除阶段。标记阶段的任务是标记出所有需要被回收的对象，清除阶段就是回收被标记的对象所占用的空间  
> Mark-Compact（标记-整理）:标记之后，将存活对象都向一端移动，然后清理掉端边界以外的内存

## 3). HotSpot几种垃圾收集器   
![gc](https://github.com/PeterLee08/resources/blob/master/icon/png/java/gc.png?raw=true)   
[简书介绍: https://www.jianshu.com/p/b4a03b5de0d9](https://www.jianshu.com/p/b4a03b5de0d9)

## 4). 分代收集
JVM 采用了分代收集以后，minor gc 只扫描新生代，但是minor gc 怎么判
断是否有老生代的对象引用了新生代的对象，JVM 采用了卡片标记的策略，卡
片标记将老生代分成了一块一块的，划分以后的每一个块就叫做一个卡片，JVM
采用卡表维护了每一个块的状态，当JAVA 程序运行的时候，如果发现老生代对
象引用或者释放了新生代对象的引用，那么就JVM 就将卡表的状态设置为脏状
态，这样每次minor gc 的时候就会只扫描被标记为脏状态的卡片，而不需要扫描
整个堆。具体如下图：  
    
![beyond](https://github.com/PeterLee08/resources/blob/master/icon/png/java/beyond.png?raw=true)


## 5). 不同引用回收策略
4中不同Strong reference,Soft reference,Weak reference,Phantom reference
* 强引用 Strong reference: 平常对象引用    
* 软引用 Soft reference, 只有在内存不足的时候JVM才会回收该对象,适用于有用但是并非必须的对象
```java
SoftReference<Web> webRef = new SoftReference<Web>(web); 
if (webRef.get() != null){ 
    webr = webRef.get();
} else { //如果被回收了使用前创建一下
    webRef = new SoftReference<Web>(new Web());
}

```
* 弱引用Weak reference: 一旦发现了只具有弱引用的对象,即回收 '与用完立即置null类似?'
* 虚引用Phantom reference: 不影响生存时间,唯一影响就是回收时收到系统通知


# 三. jvm调优参数
## 1. jvm 参数图解
![分代实现](https://github.com/PeterLee08/resources/blob/master/icon/png/java/jvm_generation.png?raw=true)

## 2. 设置server模式
## 3. 其他常用

参数 | 解释 | 默认值 | 说明
---|--- | --- | ---
-Xmn | 新生代大小 |  | Sun官方推荐配置为整个堆的3/8
-XX:NewRatio |	新生代与老生代的比值(除去持久代)||	 	-XX:NewRatio=4表示年轻代与年老代所占比值为1:4,年轻代占整个堆栈的1/5 (Xms=Xmx并且设置了Xmn的情况下，该参数不需要进行设置)
-XX:SurvivorRatio |	Eden区与Survivor区的大小比值 | |	 	设置为8,则两个Survivor区与一个Eden区的比值为2:8,一个Survivor区占整个年轻代的1/10, 如果临时对象较少,回收较少则调大survivor区比例
-XX:MaxTenuringThreshold |	垃圾最大年龄||	 	如果设置为0的话,则年轻代对象不经过Survivor区,直接进入年老代. 对于年老代比较多的应用,可以提高效率.如果将此值设置为一个较大值,则年轻代对象会在Survivor区进行多次复制,这样可以增加对象再年轻代的存活 时间,增加在年轻代即被回收的概率,该参数只有在串行GC时才有效.
-XX:PretenureSizeThreshold |	对象超过多大是直接在旧生代分配|	0	|单位字节 新生代采用Parallel Scavenge GC时无效 另一种直接在旧生代分配的情况是大的数组对象,且数组中无外部引用对象.
-XX:+PrintGC |||输出形式:[GC 118250K->113543K(130112K), 0.0094143 secs][Full GC 121376K->10414K(130112K), 0.0650971 secs]
XX:+PrintGCDetails	|| |输出形式:[GC [DefNew: 8614K->781K(9088K), 0.0123035 secs] 118250K->113543K(130112K), 0.0124633 secs][GC [DefNew: 8614K->8614K(9088K), 0.0000665 secs][Tenured: 112761K->10414K(121024K), 0.0433488 secs] 121376K->10414K(130112K), 0.0436268 secs]
-XX:+PrintGCApplicationStoppedTime|	打印垃圾回收期间程序暂停的时间.可与上面混合使用	 ||	输出形式:Total time for which application threads were stopped: 0.0468229 seconds
-XX:+PrintGCApplicationConcurrentTime |	打印每次垃圾回收前,程序未中断的执行时间.可与上面混合使用||	 	输出形式:Application time: 0.5291524 seconds
-XX:+PrintHeapAtGC|	打印GC前后的详细堆栈信息|||	 	 
-Xloggc:filename |	把相关日志信息记录到文件以便分析.||与上面几个配合使用
-XX:+PrintClassHistogram ||||

	 	 

# 四. jvm监控工具
待完善
jmap
jcmd
pmap
jps
jhat
top 


>总结
* 热加载无卸载影响永生代空间累积
* 老生代做缓存
* 不用时置为null
* 超大对象一般直接生成老生代