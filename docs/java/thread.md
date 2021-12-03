# 线程
## 进程
> 进程是一个程序分配的基本单位，即一个程序就是一个进程，它是静态的
## 线程
> 线程是cpu调度运行的基本单位，动态分配的，它是动态的
## 协程

并发编程：原则
synchronized 解决 可见、原子性
可见、有序、原子

单线程
happens-before

精通 多线程
精通 JVM
精通 MYSQL调优 
## 超线程

指令重排指的是单个线程，汇编码的数字是指令偏移量顺序发生变化 volatile

核心线程数：
> - CPU密集型：核心线程数=CPU核心数(或 核心线程数=CPU核心数+1)
> - I/O密集型：核心线程数=2*CPU核心数（或 核心线程数=CPU核心数/（1-阻塞系数））

# 高并发
- 1、多线程查询再聚合
- 2、异步返回， web容器主线程 + 子线程执行
- 3、多线程定时器批量查询下游系统根据唯一标识再返回 CompletableFuture.complete(get(id))


DCL => double check lock (双重检查锁)

//  返回一下，又进入到等待状态
Thread.yield();

```java
    if(INSTANCE == null) {
        synchronized(Demo.class) {
            if (INSTANCE == null) {
            }
        }
}
```