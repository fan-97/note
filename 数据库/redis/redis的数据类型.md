

>  redis中有五种数据类型：string、hash、list、set、zset
>
>  在redis中可以通过 type key 来获取当前键值的类型

## 1.字符串（string）

通过 **set key value** 来设置键的值，**get key** 来获取键值

如果存储的值的是一个整数，那么可以通过`incr` 来对当前键值进行自增操作并放回自增后的结果。包括`incr`在内的所有redis命令都是原子操作

## 2.散列（hash）

## 3.列表（list）

## 4.集合（set）

## 5.有序集合（zset）

