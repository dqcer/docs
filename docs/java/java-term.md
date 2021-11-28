## 1、`final`常量域
> 被关键性`final`修饰的对象，称为常量域，必须在每个构造器执行之后，这个域被设置后，后面的操作都不能对其进行修改
> 应用于基本类型和不可变类的域
```java
public class FinalDomain {

    /**
     * 常量域
     */
    private final String init;

    public FinalDomain(String init) {
        this.init = init;
    }

    public void demo() {
        System.out.println(init);
    }

    public static void main(String[] args) {
        new FinalDomain("good job").demo();
    }
}
```

## 2、静态域
> 被关键字`static`修饰的对象\代码块，称为静态域。每个类中只有一个这样的域，属于类，不属于任务独立的对象
```java
public class StaticDomain {

    /**
     * 静态域
     */
    private static String init = demo();

    /**
     * 静态域
     */
    {
        System.out.println(init);
    }

    private static String demo() {
        System.out.println("这是方法");
        return "静态代码块";
    }
    
    public static void main(String[] args) {
        new StaticDomain();
//      这是方法 
//      静态代码块
    }
}
```
## 3、静态常量
> 被关键字`static` 和 `final`修饰的对象\基本类型，称为静态常量

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


DCL => double check lock (双重检查锁)

```java
    if(INSTANCE == null) {
        synchronized(Demo.class) {
            if (INSTANCE == null) {
}

}
    



```

# 架构
## 防腐层
- 职责
> -  异常降级：对RPC调用可能抛出的异常捕获，降级处理（输出异常日志、返回empty结果 / 抛出业务异常errorcode）；
> -  超时/重试：RPC接口的超时、重试 统一管理
> -  数据校验：对返回值的正确性、边界值进行校验，进行数据的基本防御、业务代码边界值的解耦；
> -  接口防腐：转换成Vo值对象，避免下游接口的修改 导致 自身系统的修改；  
- 目的
> -  代码复用：RPC的调用场景 差异化小，proxy层复用，提高开发效率；
> -  提供便利性：调用方可以直接调用，对RPC结果可以直接使用，不需要考虑NPE（结果为空）、RPC异常等情况

