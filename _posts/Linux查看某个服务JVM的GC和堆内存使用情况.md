### 一、 使用 jps 命令查看配置了JVM的服务

 **jps**

![image-20211220144237040](../../Library/Application Support/typora-user-images/image-20211220144237040.png)

### 二、查看某个进程JVM的GC使用情况

 jstat -gc 21677 3000

 **jstat -gc PID 刷新时间**

![image-20211220144145027](../../Library/Application Support/typora-user-images/image-20211220144145027.png)

> ​    S0C：年轻代中第一个survivor（幸存区）的容量 (字节) 
> ​    S1C：年轻代中第二个survivor（幸存区）的容量 (字节) 
> ​    S0U：年轻代中第一个survivor（幸存区）目前已使用空间 (字节) 
> ​    S1U：年轻代中第二个survivor（幸存区）目前已使用空间 (字节) 
> ​    EC：年轻代中Eden（伊甸园）的容量 (字节) 
> ​    EU：年轻代中Eden（伊甸园）目前已使用空间 (字节) 
> ​    OC：Old代的容量 (字节) 
> ​    OU：Old代目前已使用空间 (字节)
> ​    YGC：从应用程序启动到采样时年轻代中gc次数 
> ​    YGCT：从应用程序启动到采样时年轻代中gc所用时间(s) 
> ​    FGC：从应用程序启动到采样时old代(全gc)gc次数 
> ​    FGCT：从应用程序启动到采样时old代(全gc)gc所用时间(s) 
> ​    GCT：从应用程序启动到采样时gc用的总时间(s) 

### 三、查看堆内存使用情况

jmap -heap 21677

> 执行 `jmap -heap 21677 `出现异常：`Error attaching to process: sun.jvm.hotspot.debugger.DebuggerException: cannot open binary file`
>
> 可参考：https://help.mulesoft.com/s/article/sun-jvm-hotspot-debugger-DebuggerException-cannot-open-binary-file-when-trying-to-use-jstack-or-jmap
>
> sudo jmap -heap 21677

**jmap -heap 进程号

![image-20211221145800852](../../Library/Application Support/typora-user-images/image-20211221145800852.png)



> Heap Configuration:   #堆配置情况 
>    MinHeapFreeRatio         = 40  #堆最小使用比例
>    MaxHeapFreeRatio         = 70  #堆最大使用比例
>    MaxHeapSize              = 8589934592 (8192.0MB)  #堆最大空间
>    NewSize                  = 1363144 (1.2999954223632812MB) #新生代初始化大小
>    MaxNewSize               = 5152702464 (4914.0MB)          #新生代可使用最大容量大小
>    OldSize                  = 5452592 (5.1999969482421875MB) #老生代大小
>    NewRatio                 = 2   #新生代比例
>    SurvivorRatio            = 8   #新生代与suvivor的占比
>    MetaspaceSize            = 21807104 (20.796875MB) #元数据空间初始大小
>    CompressedClassSpaceSize = 1073741824 (1024.0MB) #类指针压缩空间大小, 默认为1G
>    MaxMetaspaceSize         = 17592186044415 MB  #元数据空间的最大值, 超过此值就会触发 GC溢出( JVM会动态地改变此值)
>    G1HeapRegionSize         = 2097152 (2.0MB) #区块的大小
>
> Heap Usage:
> G1 Heap:
>    regions  = 4096
>    capacity = 8589934592 (8192.0MB)
>    used     = 175481296 (167.3520050048828MB)
>    free     = 8414453296 (8024.647994995117MB)
>    2.042871154844761% used
> G1 Young Generation:
> Eden Space:
>    regions  = 29
>    capacity = 117440512 (112.0MB)
>    used     = 60817408 (58.0MB)
>    free     = 56623104 (54.0MB)
>    51.785714285714285% used
> Survivor Space:
>    regions  = 3
>    capacity = 6291456 (6.0MB)
>    used     = 6291456 (6.0MB)
>    free     = 0 (0.0MB)
>    100.0% used
> G1 Old Generation:
>    regions  = 52
>    capacity = 136314880 (130.0MB)
>    used     = 108372432 (103.35200500488281MB)
>    free     = 27942448 (26.647994995117188MB)
>    79.50154231144832% used



| -Xmx               | 设置JVM最大可用堆内存大小。                                  |
| ------------------ | ------------------------------------------------------------ |
| -Xms               | 设置初始堆大小，一般和Xmx保持一致。                          |
| -Xmn               | 设置年轻代堆大小。                                           |
| -Xss               | 设置每个线程的堆大小。JDK 1.5以后每个线程堆栈大小默认为1MB，1.5以前为256K。 |
| -XX:NewRatio=      | 设置年轻代（包括Eden和两个Survivor区）与老年代的比值（不包括永久代）。如设置为4，则年轻代与老年代所占比值为1:4，年轻代占整个堆栈的1/5。 |
| -XX:SurvivorRatio= | 设置年轻代中Eden区与Survivor区的大小比值。如设置为4，则两个Survivor区与一个Eden区的比值为2:4，一个Survivor区占整个年轻代的1/6。 |
| -XX:MaxPermSize=   | 设置永久代堆大小。                                           |

```bash
# 设置最大堆空间为3600MB，初始化堆大小为3600MB，年轻代大小为2GB，线程堆大小为128KB。
java -Xmx3600m -Xms3600m -Xmn2g -Xss128k
```



**Reference**

# [Java垃圾收集必备手册](https://www.bookstack.cn/read/gc-handbook/README.md)