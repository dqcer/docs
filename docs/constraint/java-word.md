# 命名规约

### 包名
> 包名统一使用小写

# 编码规范

## MyBatis 不要为了多个查询条件而写 1 = 1
数据库系统就无法使用索引等查询优化策略，数据库系统将会被迫对每行数据进行扫描（即全表扫描） 以比较此行是否满足过滤条件

## 工具类中屏蔽构造函数，私有无参构造器
工具类是一堆静态字段和函数的集合，其不应该被实例化；但是，Java 为每个没有明确定义构造函数的类添加了一个隐式公有构造函数，为了避免不必要的实例化，应该显式定义私有构造函数来屏蔽这个隐式公有构造函数。

## 返回空集合或数组而非null
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

## 枚举的属性，没有set字段方法，可以加上final修饰符

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