# 编码规范

## 1、包名
> 包名统一使用小写

## 2、MyBatis 不要为了多个查询条件而写 1 = 1
数据库系统就无法使用索引等查询优化策略，数据库系统将会被迫对每行数据进行扫描（即全表扫描） 以比较此行是否满足过滤条件

## 3、工具类中屏蔽构造函数，私有无参构造器
工具类是一堆静态字段和函数的集合，其不应该被实例化；但是，Java 为每个没有明确定义构造函数的类添加了一个隐式公有构造函数，为了避免不必要的实例化，应该显式定义私有构造函数来屏蔽这个隐式公有构造函数。

## 4、返回空集合或数组而非null
反例
> ```java
>      public static Result[] getResults() {
>          return null;
>      }
>   
>      public static List<Result> getResultList() {
>          return null;
>      }
>   
>      public static Map<String, Result> getResultMap() {
>          return null;
>      }
> ```

正例
```java
         public static Result[] getResults() {
             return new Result[0];
         }
      
         public static List<Result> getResultList() {
             return Collections.emptyList();
         }
      
         public static Map<String, Result> getResultMap() {
             return Collections.emptyMap();
         }
 
```

## 5、枚举的属性，没有set字段方法，可以加上final修饰符

反列
```java
public enum ActiveStatusEnum {

    UN_COMPLETED(1, "待完成"),
    COMPLETED(2, "已完成"),
    OVERDUE(3, "已逾期完成");

    private String name;
    private Integer index;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getIndex() {
        return index;
    }

    public void setIndex(Integer index) {
        this.index = index;
    }

    ActiveStatusEnum(int index, String name) {
        this.name = name;
        this.index = index;
    }

    public static String getName(int index) {
        for (ActiveStatusEnum c : ActiveStatusEnum.values()) {
            if (c.getIndex() == index) {
                return c.name;
            }
        }
        return null;
    }
}
```

正例
```java
public enum ActiveStatusEnum {

    UN_COMPLETED(1, "待完成"),
    COMPLETED(2, "已完成"),
    OVERDUE(3, "已逾期完成");

    private final String name;
    private final Integer index;

    public String getName() {
        return name;
    }
    
    public Integer getIndex() {
        return index;
    }

    ActiveStatusEnum(int index, String name) {
        this.name = name;
        this.index = index;
    }

    public static String getName(int index) {
        for (ActiveStatusEnum c : ActiveStatusEnum.values()) {
            if (c.getIndex() == index) {
                return c.name;
            }
        }
        return null;
    }
}
```

## 6、字符串优先考虑使用`+`、`concat`、`append`方法进行追加
```java
    public static void main(String[] args) {

        long size = 10000;
        long l = System.nanoTime();
        String str = "";
        for (int i = 0; i < size; i++) {
            str += "123";
        }
        long l2 = System.nanoTime();
        System.out.println(l2 - l);
        str = "";
        for (int i = 0; i < size; i++) {
            str.concat("123");
        }
        long l3 = System.nanoTime();
        System.out.println(l3 - l2);


        StringBuilder str3 = new StringBuilder();
        for (int i = 0; i < size; i++) {
            str3.append("123");
        }
        long l4 = System.nanoTime();
        System.out.println(l4 - l3);

    }
```
执行结果如下

 |  执行次数   | 字符串连接类型  | 耗时纳秒  |
 |  ----  | ----  | ----  |
 | 1  | `+` | 13899 |
 | 1  | `concat` | 145901 |
 | 1  | `append` | 14699 |
 | 100  | `+` | 86000 |
 | 100  | `concat` | 147899 |
 | 100  | `append` | 20701 |
 | 10000  | `+` | 210060900 |
 | 10000  | `concat` | 919299 |
 | 10000  | `append` | 337601 |
 
 总结
 > 单个就用`+`，多个字符串拼接就用`StringBuilder`或者`StringBuffer`

