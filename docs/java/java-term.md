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