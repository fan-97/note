记录一下学习python的几种基本类型

[toc]

## 1.字符串

在python中 字符串可以用 引号来表示，包括单引号`''` 和双引号`""`

```python
string='hello world!'
string="hello world!"
```

![image-20201203215952563](https://fnoteimg.oss-cn-chengdu.aliyuncs.com/img/20201203215952.png)



### 1.1 如何定义一个字符串

### 1.2 字符串的常用操作

## 2.数字

数字分为整型和浮点型

整型：`a=1`

浮点型：`f = 1.0`

数字的运算使用`+`、`-`、`*`、`/` 来进行加减乘除运算

在python中 使用`**` 两个星号表示次方 比如：`2**10 表示2 的10次方`

## 3.列表

python中使用`[]` 来定义一个列表，列表里面的值可以是任何的类型。比如

`list = [1,23,'2','zhangsan']` 既可以存在数字也可以存在字符串

### 3.1 列表的基本操作

- 定义一个列表
- 往列表里面添加数据
- 删除里面中数据
- 获取列表的长度
- 获取列表中的数据

### 3.2 切片

在列表有个很重要的操作，那就是**切片**

切片可以用来处理列表的一部分数据，比如你想获得列表中前几个元素，就可以这样来写代码 




list = [1,2,3,4,5]

取前2个元素

list[:2]

![image-20201203225842383](https://fnoteimg.oss-cn-chengdu.aliyuncs.com/img/20201203225842.png)

从第2个元素开始一直到最后一个元素

list[1:]

![image-20201203225857279](https://fnoteimg.oss-cn-chengdu.aliyuncs.com/img/20201203225857.png)

或者是取中间的元素

list[2:4]

![image-20201203225909695](https://fnoteimg.oss-cn-chengdu.aliyuncs.com/img/20201203225909.png)



切片还可以用来进行列表的复制操作



### 3.3 元组

列表里面还有另一个种类型，就是**元组**

> 元组是一个不可变的列表，不能修改元组中的任何元素

定义一个元组

metadata=(1,2,3)

现在去尝试去删除一个元素



## 4.字典

>  在python中字典是一个 key，value的存储格式。可以存储多对key,value在同一个字典中

### 4.1 如何定义一个字典

### 4.2 字典的基本操作

#### 4.2.1 遍历字典

#### 4.2.1 添加字典元素

#### 4.2.1 修改字典元素

#### 4.2.1 删除字典元素

