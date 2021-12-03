# 静态方法返回对象

- `from`类型转换，单个入参，返回该类型的实例
```java
    Date date = Date.from(instant);
```
- `of`聚合方法，多个入参，返回该类型的实例
```java
    enum Demo {
        JACK, QUEEN, KING;
    }
    
    public static void main(String[] args) {
        // 指定枚举值创建Set集合
        Set<Demo> demoSet = EnumSet.of(Demo.JACK, Demo.QUEEN);
        // [JACK, QUEEN]
        System.out.println(demoSet);
    }
```
- `valueOf`相比`from`和`of`烦琐的方式
```java
    BigInteger bigInteger = BigInteger.valueOf(Integer.MAX_VALUE);
```
- `instance`/`getInstance`（通过参数）返回实例
```java
    Demo2 demo2 = Demo.getInstance(1);
```
- `create`/`newInstance`（通过参数）返回新的实例
```java
    Object newArray = Array.newInstance(classObject, arrayLen);
```
- `getType`（通过参数）表示通过工厂方式返回对象类型，一般是抽象类和接口
```java
    FilesStore filesStor = File.getFilesStore(path);
```
- `newType`、`type`
```java
    BufferdReader br = file.newBufferedReader(path);
    List<E> list = Collections.list(legacyLitany);
```

# 类名

