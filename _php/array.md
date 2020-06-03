# 数组
> 数组的基础和基本方法

## 创建数组

1. 直接创建

```php
// 索引数组
$arr[0] = 'Sam';
$arr[1] = 'Tom';

// 如果省略下标，则自动递增
$arr[] = 'Jean';
print_r($arr);
/*
Array
(
    [0] => Sam
    [1] => Tom
    [2] => Jean
)
*/
// 关联数组
$arr1['name'] = 'Tom';
$arr1['age'] = 15;
print_r($arr1);
/*
Array
(
    [name] => Tom
    [age] => 15
)
*/
```

