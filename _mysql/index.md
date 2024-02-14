# 索引

## MySQL索引失效的场景

1. **联合索引** 不满足最左匹配原则。

2. 使用 **select** 不会走 **覆盖索引**（属于优化项）。

3. 索引列**参与计算**。

   `select * from t_user where id + 1 = 2 `

4. 索引列使用了**函数**。

   `select * from t_user where SUBSTR(id_no,1,3) = '100'`

5. **like** 的使用(实际也是不符合最左匹配原则)。

   `select * from t_user where id_no like '%00%'`

6. **类型的隐式转换**。

   `select * from t_user where id_no = 1002`

   此处 **id_no** 的类型为 **varchar**，而SQL中使用了 **int**，则导致全表扫描。

7. 使用 **or** 操作

   `select * from t_user where id = 2 or username = 'Tom2'`，由于 **username** 没有索引，则会引发全表扫描；

   `select * from t_user where id  > 1 or id  < 80`，**or** 两边同时使用 **<** 和 **>** ，索引也会失效。

8. 两列做**比较**

   `select * from t_user where id > age`, **两列数据做比较，即便两列都创建了索引，索引也会失效**。

9. 其他

   **Mysql优化器的其他优化策略，比如优化器认为在某些情况下，全表扫描比走索引快，则它就会放弃索引。**
