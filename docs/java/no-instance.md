## 控制不被实例化

> 有时候编写的类只有`静态方法`和`静态域`，比如工具类就不应该实例化，因为实例化也没有任何意义，然而
>在缺少显式无参构造器的情况下，编译器会自动生成公有无参的构造器，通常的解决方案是手动添加一个私有的
>构造器，也可以适当抛出异常，处理反射机制

```java
public class StrUtils {

    /**
     * 禁止实例化
     */
    private StrUtils() {
        // 抛出异常是抵制通过反射访问
        throw new AssertionError();
    }

    public static boolean isBlank(String str) {
        if (null == str || str.length() == 0 || str.trim().length() == 0) {
            return true;
        }
        return false;
    }

    public static void main(String[] args) throws ClassNotFoundException, IllegalAccessException, InstantiationException {
        Class<?> clazz = Class.forName("com.dqcer.tools.core.StrUtils");
        Object o = clazz.newInstance();
    }
}
```