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

