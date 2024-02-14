# 常见的并发工具类
> 常见的一些并发工具类的原理和使用

## ThreadLocal

**ThreadLocal** 类用来提供线程内部的 **局部变量**。这种变量在多线程环境下访问（通过 set 和 get 方法）时能保证各个线程的变量相对独立于其他线程内的变量。ThreadLocal实例通常来说都是 **private static** 类型的，用于关联线程和线程上下文。

总结：

1. 线程并发：在多线程场景下使用。
2. 传递数据：通过TheadLocal在同一线程下，不同的组件中传递数据。
3. 线程隔离：每个线程的变量都是独立的，不会相互影响。

---

##### 基本使用

```java
public class ThreadLocalTest {
    ThreadLocal<String> threadLocal = new InheritableThreadLocal<>();

    public String getContent() {
        return threadLocal.get();
    }

    public void setContent(String content) {
        threadLocal.set(content);
    }

    public static void main(String[] args) {
        ThreadLocalTest threadLocalTest = new ThreadLocalTest();

        for (int i = 0; i < 5; i++) {
            Thread thread = new Thread(() -> {
                threadLocalTest.setContent(Thread.currentThread().getName() + "的数据");
                System.out.println("---------------------");
                System.out.println(Thread.currentThread().getName()+ "------->" + threadLocalTest.getContent());
            });

            thread.setName("线程" + i);
            thread.start();
        }
    }
}
```
##### ThreadLocal的常见误解

1. **早期设计**

一般情况下我们可能会猜测 **Thread Local**是这样设计的：每个 ThreadLocal 都创建一个**Map**，然后用线程作为 Map 的 key，要存储的局部变量作为线程的 value，这样就能达到局部变量隔离的效果。这是最简单的设计方法，JDK 早期也确实是这样设计的。

<img width="800px" src="_media/早期ThreadLocal.png">

2. **现在的设计**

JDK8 中 ThreadLocal 的设计是：每个 **Thread** 维护一个 **ThreadLocalMap**，这个 **Map** 的 **key** 是 **ThreadLocal** 实例本身，**value** 才是真正要存储的值 **Object**。

具体的过程是这样的：

* 每个 Thread 线程内部都有一个 Map（ThreadLocalMap）。
* Map里面存储的 **ThreadLocal** 对象（key）和 线程变量副本（value）。
* Thread内部的Map是由ThreadLocal维护的，由ThreadLocal负责向map获取和设置线程的变量值。
* 对于不同的线程，每次获取副本值时，别的线程是不能获取当前线程的副本值，形成副本隔离，互不干扰。

<img width="800px" src="_media/目前ThreadLocal的设计.png">


3. **目前设计的优点**
    * 每个 Map 存储的 **Entry** 的数量变少（减少 Hash 冲突）。
    * 当 Thread 销毁的时候，ThreadLocalMap 也会随之消失，较少内存的使用。

##### 弱引用和内存泄漏

1. **内存泄漏相关概念**

    * Memory overflow：**内存溢出**，没有足够的内存提供给申请者使用。
    * Memory leak：**内存泄漏**，指程序中已经分配的内存未释放或无法释放，造成系统内存的浪费，导致程序运行速度减慢甚至系统崩溃等严重后果。内存泄漏的堆积将导致内存溢出。

2. **弱引用相关概念**

   Java中有四种引用类型： 强、软、弱、虚。

   **强引用（Strong Reference）**，就是我们常见的普通对象的引用，只要还有一个强引用指向一个对象，就能表示对象还"活着"，垃圾回收器就不会回收这种对象。

   **弱引用（Weak Reference）**，垃圾回收器一旦发现弱引用对象，不管内存空间是否足够，都会回收它的内存。

3. **如果使用强引用**

   假设 ThreadLocalMap 中的 key 使用了强引用，那么会出现内存泄漏么？

   <img width="800px" src="_media/ThreadLocal内存图1.png">
   
   假设业务代码中使用完了 ThreadLocal，ThreadLocal Ref就被回收了。
   但是因为 threadLocalMap 中的 Entry 强引用了 threadLocal ，造成了 threadLocal 无法被回收。
   在没有手动删除这个 Entry 以及 CurrentThread 依然在运行的情况下，始终有强引用链 **threadRef->currentThread->threadLocalMap->Entry**, Entry就不会被回收（Entry 中包括了 ThreadLocal 实例和 value），导致内存泄漏
   也就是说，ThreadLocalMap 中 key 使用了强引用，是无法完全避免内存泄漏的。

4. **如果是用弱引用**

   <img width="800px" src="_media/ThreadLocal内存图2.png">
   
   同样假设业务代码中使用完ThreadLocal，ThreadLocal Ref被回收了。
   由于 ThreadLocalMap 只持有 ThreadLocal 的 **弱引用**，没有任何强引用指向 threadLocal 对象，所以 threadLocal 对象就顺利的被 gc 回收了，此时 Entry 中的 key = null。
   但是没有手动删除这个 Entry 以及 CurrentThread 依然在运行的情况下，也存在强引用链 **threadRef->currentThread->threadLocalMap->Entry->value**，value 不会被回收，而且这个 value 永远也访问不到，导致 value 内存泄漏。
   也就是说，ThreadLocalMap 中使用了弱引用，也有可能内存泄漏。

