平常开发会使用单例，即仅仅被实例化一次的类，常见的实现如下
- 常见实现
```java
public class Singleton {

    public static final Singleton INSTANCE = new Singleton();

    private Singleton(){}

    public static Singleton getInstance() {
        return INSTANCE;
    }
    public static void main(String[] args) {
        Singleton instance = Singleton.getInstance();
        System.out.println(instance.equals(Singleton.getInstance()));
    }
}
```
> - **优点**: 基于classload机制避免了多线程的同步问题，线程安全。
> - **缺点**: 容易产生垃圾对象，没有达到懒加载效果。无法抵御反射机制；不支持序列化，每次反序列化一个序列化实例时，都是生产新的实例。

- 处理序列化和抵御反射
> 实现`Serializable`接口，为了保证单例必须提供`readResolve`方法，
```java
public class Singleton implements Serializable {

    private static final long serialVersionUID = -7483565312720601386L;

    public static final Singleton INSTANCE = new Singleton();

    private Singleton(){
        if (null != INSTANCE) {
            // 抵御反射攻击
            throw new InstantiationError();
        }
    }

    public static Singleton getInstance() {
        return INSTANCE;
    }

    /**
     * 1、ObjectInputStream#readObject()
     * 2、ObjectInputStream#readObject0()
     * 3、ObjectInputStream#readOrdinaryObject()
     * 4、ObjectStreamClass#hasReadResolveMethod()
     * 5、ObjectStreamClass构造器
     *          readResolveMethod = getInheritableMethod(
     *                         cl, "readResolve", null, Object.class);
     *
     * @return {@link Object}
     */
    public Object readResolve() {
        return INSTANCE;
    }

    public static void main(String[] args) throws IOException, ClassNotFoundException {
        Singleton instance = Singleton.getInstance();

        // 将instance序列化到磁盘
        FileOutputStream fos = new FileOutputStream("Singleton.txt");
        ObjectOutputStream oos = new ObjectOutputStream(fos);
        oos.writeObject(instance);
        oos.flush();

        // 从磁盘反序列化
        FileInputStream fis = new FileInputStream("Singleton.txt");
        ObjectInputStream ois = new ObjectInputStream(fis);
        Singleton newSingleton = (Singleton) ois.readObject();
        System.out.println(instance.equals(newSingleton));
    }
}
```
- 利用枚举处理
> 声明一个包含单个元素的枚举类型，实现更加简洁，自动提供了序列化机制，同样能自动处理多个实例化和抵御反射攻击，是实现单例的最佳方法。
```java
public enum Singleton {
    INSTANCE;
    public static void main(String[] args) throws IOException, ClassNotFoundException {
        Singleton instance = Singleton.getInstance();

        // 将instance序列化到磁盘
        FileOutputStream fos = new FileOutputStream("Singleton.txt");
        ObjectOutputStream oos = new ObjectOutputStream(fos);
        oos.writeObject(instance);
        oos.flush();

        // 从磁盘反序列化
        FileInputStream fis = new FileInputStream("Singleton.txt");
        ObjectInputStream ois = new ObjectInputStream(fis);
        Singleton newSingleton = (Singleton) ois.readObject();
        System.out.println(instance.equals(newSingleton));
    }
}
```

