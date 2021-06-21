



> 引言：记录学习过程~~





# 内存区域
> JVM中内存的各个区域以及每个区域的一些特性和存放的东西，主要介绍运行时数据区。
> 注意：不要把内存区域和Java内存模型（JMM）搞混了，JMM是JVM定义的一种规范

1. 运行时数据区的内存区域大致分布

![java内存区域.png](https://cdn.nlark.com/yuque/0/2020/png/1726219/1597243651807-5ab1437b-08ab-4f32-ba4b-cff793b54839.png#align=left&display=inline&height=322&margin=%5Bobject%20Object%5D&name=java%E5%86%85%E5%AD%98%E5%8C%BA%E5%9F%9F.png&originHeight=767&originWidth=1343&size=22311&status=done&style=none&width=564)


## 1. 程序计数器
程序计数器（Program counter Register），是存放下一个即将执行的指令的地址。在要执行一条指令（也就是某一些代码）的时候，会从PC中取出对应的指令地址获取到相应的指令并执行。在多个线程的情况下，经常会有上下文切换，如果当 `线程1` 没有记录执行的指令，并且 `线程2` 获得了CPU的执行权，那么下次当 `线程1` 获取到CPU的执行权的时候，将不会知道上一次执行到什么地方了。所以，程序计数器是每个线程私有的。
