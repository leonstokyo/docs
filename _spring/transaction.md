# Spring事务
> 介绍Spring事务的相关方法和简单实践

## Spring事务的传播机制

#### 概念

Spring **事务传播机制** 可使用 `@Transactional(propagation=Propagation.REQUIRED)` 来定义，
Spring **事务传播机制** 的级别包含以下 **7** 种：

1. `Propagation.REQUIRED`：**默认的** 事务传播级别，它表示如果当前存在事务，则加入该事务；如果当前没有事务，则创建一个新的事务。
2. `Propagation.SUPPORTS`：如果当前存在事务，则加入该事务；如果当前没有事务，则以非事务的方式继续运行。
3. `Propagation.MANDATORY`：如果当前存在事务，则加入该事务；如果当前没有事务，则抛出异常。
4. `Propagation.REQUIRES_NEW`：表示创建一个新的事务，如果当前存在事务，则把当前事务挂起。
5. `Propagation.NOT_SUPPORTED`：以非事务方式运行，如果当前存在事务，则把当前事务挂起。
6. `Propagation.NEVER`：以非事务方式运行，如果当前存在事务，则抛出异常。
7. `Propagation.NESTED`：如果当前存在事务，则创建一个事务作为当前事务的嵌套事务来运行；如果当前没有事务，则该取值等价于 `PROPAGATION_REQUIRED`。

<img width="1000px" src="_media/transaction1.png">

#### 示例

## Spring事物失效的场景与原因分析

1. **方法内的自调用**

   Spring事务是基于 **AOP** 的，只有使用代理对象调用某个方法时，Spring事务才会生效，而在一个方法中调用this.xxx()时，this并不是代理对象，所以会导致事务失效（准确的说是 `@Transactional` 注解失效）

   解决办法：

    1. 把调用的方法拆分到另一个**Bean**中
    2. 自己注入自己
    3. `AopContext.currentProxy()` + `@EnableAspectAutoProxy(exposeProxy = true)`

2. **方法被private修饰**

   Spring 事务会基于 CGLIB 来进行 AOP 。如果父类中的某个方法是 private 修饰的，那么代理子类无法重写该方法，就无法额外添加 Spring 事务的逻辑。

3. **方法被final修饰**

   同上逻辑，子类不能重写父类中的 final 方法。

4. **新的线程调用方法**

   线程之间是隔离的，新的线程会从 ThreadLocal 中去获取数据库连接对象。显然新开的线程无法拿到旧线程的数据库连接对象，因此会新建一个数据库连接对象（autocommit为true），执行完 SQL 就会提交。

5. **未抛出异常**

   没有捕获到异常或者在默认情况下只会捕获**RuntimeException**和**Error**

6. **类没有被Spring管理**

7. **数据库不支持事务**

