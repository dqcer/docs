# 介绍
> 一个简单的秒表工具类，不是线程安全的，常用平常开发中对于性能的验证。比如：验证这个方法执行需要耗时多少纳米，不建议作为生成环境中使用
>其原理是利用`System.nanoTime()`，然后进行封装，提高可读性。


# 用法

```java
public class StopWatchTests {

    private final StopWatch stopWatch = new StopWatch("dqcer");

    @Test
    void test() {
        stopWatch.start("task 1");
        //  执行methodsA()
        methodsA();
        stopWatch.stop();

        stopWatch.start("task 2");
        // 执行methodsA()
        methodsA();
        stopWatch.stop();
        System.out.println("getId" + stopWatch.prettyPrint());
    }

    public static void methodsA() {
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

控制台输出

```log
Connected to the target VM, address: '127.0.0.1:17352', transport: 'socket'

getIdStopWatch 'dqcer': running time = 4004983100 ns
---------------------------------------------
ns         %     Task name
---------------------------------------------
2002906600  050%  task 1
2002076500  050%  task 2


Disconnected from the target VM, address: '127.0.0.1:17352', transport: 'socket'

Process finished with exit code 0

```