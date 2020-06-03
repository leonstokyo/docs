# PHP面试题集锦一

1.打印当前是时间，打印格式：2020-4-10 22: 22: 22。
该题主要是考察`date()`函数的常见格式，前导0的有无。
```php
date_default_timezone_set('PRC'); // 设置当前页面的时区。

// 常见的几个时间格式
Y：4位数年份
y：2位数年份

m：有前导0的月份  01-12
n：无前导0的月份  1-12

d：有前导0当月第几天 01-31
j：无前导0当月第几天 1-31

H：24小时制，有前导0  00-23
G：24小时制，无前导0  0-23

h：12小时制，有前导0  01-12
g：12小时制，无前导0  1-12

i：有前导0，分钟 00-59
s：有前导0，秒 00-59

echo date('Y-n-d h:i:s'); // 2020-4-21 08:55:45
```

2.字符串转数组、数组转字符串、字符串截取、字符串替换和字符串查找的函数分别是什么？
* 字符串转数组

```php
str_split(string $string, int $split_length = 1) 按若干个一组拆分字符串为数组

$str = 'Hello World!';
print_r(str_split($str, 2));
/**
Array
(
    [0] => He
    [1] => ll
    [2] => o 
    [3] => Wo
    [4] => rl
    [5] => d!
)
**/
```

```php
explode(string $delimiter, string $string [, int $limit])
/* 以$delimiter为分隔符将$string分割为数组
   $limit为正数，则返回数组最多包含$limit个元素
   $limit为负数，则返回除最后-$limit个元素外的所有元素
   $limit是0则会当作1
   若$delimiter为''，则返回flase
   若$delimiter不存在$string中，且$limit为负数，则返回空数组，否则返回包含$string单个元素的数组。
*/

$str = 'HelloxxWorldxx!';
print_r(explode('', $str, 2)); // Warning: explode(): Empty delimiter

$str = 'HelloxxWorldxx!';
print_r(explode('xxx', $str, 2));
/**
Array
(
    [0] => HelloxxWorldxx!
)
**/

$str = 'HelloxxWorldxx!';
print_r(explode('xx', $str, 2));
/**
Array
(
    [0] => Hello
    [1] => Worldxx!
)
**/

$str = 'HelloxxWorldxx!';
print_r(explode('xx', $str, -1));
/**
Array
(
    [0] => Hello
    [1] => World
)
**/

$str = 'HelloxxWorldxx!';
print_r(explode('xxx', $str, -1));
/**
Array
(
)
**/

$str = 'HelloxxWorldxx!';
print_r(explode('xx', $str, 0));
/**
Array
(
    [0] => HelloxxWorldxx!
)
**/
```

* 数组转化成字符串

```php
implode(string $glue , array $pieces) // 别名join $glue默认为空字符串

$arr = [1, 2, 3];
echo implode($arr);
// 123

$arr = [1, 2, 3];
echo join($arr);
// 123
```

* 截取字符串

```php
substr(string $string , int $start [, int $length ])

$start 为负数则从倒数$start的位置开始
没有提供$length则一直到结尾
如果提供的$length为负数，则忽略$string末尾的length字符
```

* 字符串替换

```php

```
