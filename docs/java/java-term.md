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



- 享元模式
本质上其实就是一个对象池，利用享元模式创建对象的逻辑也很简单：创建之前，首先去对象池里看看是不是存在；如果已经存在，就利用对象池里的对象；如果不存在，就会新创建一个对象，并且把这个新创建出来的对象放进对象池里