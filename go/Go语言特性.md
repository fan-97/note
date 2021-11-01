# 变量和常量

常量

​	const identifier type

- var 语句用于声明一个变量列表，类型在最后

## 变量

​	var identifier type

- 变量初始化：
  - 变量声明可以包含初始值，每个变量对应一个
  - 如果初始化值已存在，则可以省略类型；变量会从初始值中获得类型
  - var i , j int = 1,2
- 短变量声明
  - 在函数中，简洁赋值语句` :=` 可以在类型明确的地方代替var声明。
  - 函数外的每个语句都必须以关键字开始(var func等等)，因此`:=` 结构不能在函数外使用
  - a,b,c := true,false,"no!" ：定义三个变量 两个布尔值 一个字符串类型的变量

# 类型转换与推导

- 类型转换
  - 表达式T(v) 将值v转换为类型T
    - 一些关于数值的转换：
      - var i int = 22
      - var f float64 = float64(i)
      - var u uint = uint(f)
    - 或者,更加简单的形式：
      - i := 22
      - f :=float64(i)
      - u:=uint(f)
- 类型推导
  - 在声明一个变量而不指定类型时，变量的类型由右值推导出：
    - var i int
    - j := i // 那么 j也是一个 int类型

# 流程控制语法

## if

- 基本形式

  ```go
  if condition1 {
      // do something
  }else if condition2 {
      //do something
  }else {
      // catch-all or default
  }
  ```
  
- if 的简短语句

```go
// 在条件表达式之前添加一个简单的语句。
if v:=x - 100; v<0{
    return v
}
```

## switch

> 每个case都会默认 有break

```go
switch var1{
	case val1: //空分支
	case val2:
    fallthrough //执行case3中的f()
	case val3:
    f()
    default://默认分支
    
}
```

## For

Go只有一种循环结构：for循环。

- 计入计数器的循环

for 初始化语句;条件语句；修饰语句{}

```go
for i:=0;i<10;i++{
    sum +=i
}
```

- 初始化语句和后置语句是可选的，与while等价（go不支持while循环）

```go
for;sum<1000;{
    sum +=sum
}
```

- 无限循环

```go
for {
    if condition1{
        break
    }
}
```

## for-range

遍历数组，切片，字符串，Map等

```go
for index,char:=range myString{
    ....
}

for key,value:=range MyMap{
    ....
}

for index,value:=range MyArray{
    ....
}
```

**注意：如果for range 遍历指针数组，则value取出的指针地址为原指针地址的拷贝**

---

# 数组

- 相同类型且长度固定连续内存片段

- 以编号访问每个元素

- 定义方法

  - var identifier [len] type

  `var arr [10] int` 定义一个长度为10的整型数组



# 切片（slice）

- 切片是对数组一个连续片段的引用

- 数组定义中不指定长度则为切片

  ```go
  var i [] int
  ```

- 切片在未初始化之前默认为nil,长度为0

- 常用方法

```go
func main() {
	myArray := [5]int{1, 2, 3, 4, 5}
	mySlice := myArray[1:3]
	mySlice = append(mySlice,10)
	mySlice = append(mySlice,11)
	mySlice = append(mySlice,12)
	fmt.Printf("mySlice %+v\n", mySlice)
	fullSlice := myArray[:]
	remove3rdItem := deleteItem(fullSlice, 2)
	fmt.Printf("remove3rdItem %+v\n", remove3rdItem)
}

func deleteItem(slice []int, index int) []int {
	return append(slice[:index], slice[index+1:]...)
}
```

## make和new创建切片

- new 返回指针地址
- make 返回第一个元素，可预设初始大小和容量

```go
func main() {
	mySlice1 := new([]int)
	mySlice2 := make([]int, 0)
	mySlice3 := make([]int, 10)
	mySlice4 := make([]int, 10, 20)
	fmt.Printf("mySlice1 %+v\n", mySlice1)
	fmt.Printf("mySlice2 %+v\n", mySlice2)
	fmt.Printf("mySlice3 %+v\n", mySlice3)
	fmt.Printf("mySlice4 %+v\n", mySlice4)
}

// printf
mySlice1 &[]
mySlice2 []
mySlice3 [0 0 0 0 0 0 0 0 0 0]
mySlice4 [0 0 0 0 0 0 0 0 0 0]
```

# Map

定义一个map,

map的key只能是`基本类型` ，而value则可以是基本类型和复杂类型（函数，结构体等）。

```go
myMap := make(map[string]string,10)
myMap["name"] = "zhangsan"

// map的value还能是func
myFuncMap := map[string]func() int{
    "funcA": func() int{return 1},
}
```

# 结构体和指针

- 通过 type ... struct 关键字自定义结构体
- Go语言支持指针，但不支持指针运算
  - 指针变量的值为内存地址
  - 未赋值的指针为nil

```go
type Human struct {
	firstName, lastName string
}

type Plane struct {
	vendor string
	model  string
}
```

## 结构体标签

- 结构体中的字段除了有名字和类型外，还可以有一个可选的标签（tag）
- 使用场景：K8s APIServer 对所有资源的定义都用json tag和protoBuff tag

```go
type MyType struct {
	Name string `json:"name"`
}

func main() {
	mt := MyType{Name: "test"}
	myType := reflect.TypeOf(mt)
	name := myType.Field(0)
	tag := name.Tag.Get("json")
	println(tag)
	tb := TypeB{P2: "p2", TypeA: TypeA{P1: "p1"}}
	//可以直接访问 TypeA.P1
	println(tb.P1)
}

type TypeA struct {
	P1 string
}

type TypeB struct {
	P2 string
	TypeA
}
```

# 函数

## init函数

- init 函数： 会在包初始化时运行
- 当多个依赖项目引用统一项目，且被引用项目的初始化在init中完成，并且不可重复运行时，会导致启动错误

## 参数解析

- 给main函数传入参数的方式：
  - 方式1：
    - fmt.Println("os args is :",os.Args)
  - 方式2：
    - name := flag.String("name","world","specify  the name you want to say hi")
    - flag.Parse()

## 返回值

- 多值返回

- 命名返回值

  - Go 的返回值可以被命名，他们会被视作定义在函数顶部的变量
  - 返回值的名称应当具有一定的意义，它可以做为文档使用
  - 直接使用 `return ` 返回已经命名的返回值

  ```go
  func test(input string) (err error, result string) {
  	if input == "aaa" {
  		err = fmt.Errorf("not support aaa")
  		return
  	}
  	result = input + input
  	return
  }
  ```

  

- 调用者忽略部分返回值

  result,_ = strconv.Atoi(origStr)

## 内置函数

| 函数名            | 作用                            |
| ----------------- | ------------------------------- |
| close             | 管道关闭                        |
| len,cap           | 返回数组、切片，Map的长度或容量 |
| new,make          | 内存分配                        |
| copy,append       | 操作切片                        |
| panic,recover     | 错误处理                        |
| print,println     | 打印                            |
| complex,real,imag | 操作复数                        |

## 回调函数

- 函数可以作为参数传入其他函数，并在其他函数内部调用执行

```go
func main() {
	// DoOperation(1, increase)
	DoOperation(11, decrease)
}
// 第二个参数就是一个回调函数
func DoOperation(y int, f func(int, int)) {
	f(y, 1)
}
func increase(a, b int) int {
	return a + b
}


func decrease(a, b int) {
	println("decrease result is:", a-b)
}
```

## 闭包

- 匿名函数
  - 不能独立存在
  - 可以赋值给其他变量
    - x:=func(){}
  - 可以直接调用
    - func(x,y int ){println(x+y)}(1,2)
  - 可作为函数返回值
    - func Add() (func (b int) int){}



## 方法

- 作用在接收者上的函数

`func (recv receiver_type) methodName (parameter_list)(return_value_list)` 

- 使用场景

  - 很多场景下，函数需要的上下文可以保存在receiver属性中，通过定义receiver的方法，该方法可以直接访问receiver属性，减少参数传递需求

  ```go
  func (s *Server) StartTLS(){
  	if s.URL != "" {
  		panic("Server already started")
  	}
  	if s.client == nil{
  		s.client = &http.Client{Transport: &http.Transport{}}
  	}
  }
  ```


## 接口

- 接口定义一组方法集合

```go
type IF interface {
    Method1(param_list) return_type
}
```

- 适用场景：Kubernetes中有大量的接口抽象和多种实现
- Struct无需显示声明实现interface，只需要直接实现方法
- Struct除实现interface定义的接口外，还可以有额外的方法
- 一个类型可实现多个接口
- Go语言中接口不接受属性定义
- 接口可以嵌套其他接口

- 反射机制

  在运行时，获取对象的类型和值

  - reflect.TypeOf() 返回被检查对象的类型

  - reflect.ValueOf()返回被检查对象的值

    ```go
    func main() {
    	// basic type
    	myMap := make(map[string]string, 10)
    	myMap["a"] = "b"
    	myMap["b"] = "c"
    	t := reflect.TypeOf(myMap)
    	fmt.Println("type:", t)
    	v := reflect.ValueOf(myMap)
    	fmt.Println("value:", v)
    	// struct
    	myStruct := T{A: "a"}
    	v1 := reflect.ValueOf(myStruct)
    	for i := 0; i < v1.NumField(); i++ {
    		fmt.Printf("Field %d: %v\n", i, v1.Field(i))
    	}
    	for i := 0; i < v1.NumMethod(); i++ {
    		fmt.Printf("Method %d: %v\n", i, v1.Method(i))
    	}
    	// 需要注意receive是struct还是指针
    	result := v1.Method(0).Call(nil)
    	fmt.Println("result:", result)
    }
    
    type T struct {
    	A string
    }
    
    // 需要注意receive是struct还是指针
    func (t T) String() string {
    	return t.A + "1"
    }
    
    ```

    ```
    # 输出
    type: map[string]string
    value: map[a:b b:c]
    Field 0: a
    Method 0: 0xf29040
    result: [a1]
    ```

---

# 常用语法

## 错误处理

通过errors.New 或fmt.Errorf 创建新的error

```go
var errNotFound error = errors.New("NotFound")
```

判断error是否为空来处理error

## defer

函数返回之前执行某个语句或函数

如果有多个`defer `  ，执行顺序则是`先进后出`

> 等同于java的fianny

- 常见defer使用场景:
  - defer file.Close()
  - defer.muUnlock()
  - defer println("")

## Panic 和 recover

- panic: 可在系统出现不可恢复错误时主动调用panic,会直接让线程crash
- defer：用来保证panic之后函数还能执行
- recover：函数从panic场景恢复

```go
defer func(){
    fmt.Println("defer func is called")
    if err := recover(); err != nil{
        fmt.Println(err)
    }
}()
panic("a panic is triggered ")
```



# 多线程

在go里面使用 `go` 关键字来使用`协程`

## channel——多线程通信

Channel 是多个协程之间通信的管道.**只有发送方需要关闭通道** 

- 一端发送数据，一端接收数据
- 同一时间只有一个协程可以访问数据，无共享内存模式可能出现内存竞争
- 协调协程的执行顺序

声明方式：

- `var identifier chan datatype`
- 操作符 <-

for examlple:

```go
ch := make(chan int) //定义一个channel
go func(){
    fmt.Println("hello from goroutine")
    ch <- 0// 数据写入Channel
}()
i := <- ch// 从Channel中取数据并赋值
```

### 单向通道

只写通道 writeOnly

```go
var writeOnly chan<- int
```

只读通道 readOnly

```go
var readOnly <-chan int
```

双向通道转换：

```go
var c =  make( chan int) 
go prod(c)
go consume(c)
func prod( ch chan<- int){
	for {  ch<- 1 }
}
func consume( ch<-chan int) {
	for { <-ch}
}
```



### select

当多个协程同时运行时，通过select轮询多个通道

类似于 `switch....case....default`

多个通道都准备就绪则随机选择

```go
select {
case v:= <- ch1:// 从通道1读取数据
    ....
case v:= <- ch2:// 从通道2读取数据
    ...
}
```

### Timer——定时器

可以用来判断通道超时 自带通道`time.C` 

time.NewTime(seconds)// 起一个timer，每隔seconds时间向time.C通道中写数据，

### Context——上下文

- 超时、取消操作或者一些异常情况，往往需要进行抢占操作或者中断后续操作

- Context 是设置截止日期、同步信号，传递请求相关值的结构体

- 用法

  - context.Background

    > 顶层context，一般来说创建的context都基于Background

  - context.TODO

    > TODO 是在不确定用什么context的时候使用

  - context.WithDeadline

    > 超时时间

  - context.WithValue

    > 向context添加键值对

  - context.WithCancel

    > 创建一个可取消的context

```go
func main() {
    // 顶层context
	baseCtx := context.Background()
    // 给context设置value
	ctx := context.WithValue(baseCtx, "a", "b")
    // 起一个协程来从context获取值
	go func(c context.Context) {
		fmt.Println(c.Value("a"))
	}(ctx)
	timeoutCtx, cancel := context.WithTimeout(baseCtx, time.Second)
	defer cancel()
	go func(ctx context.Context) {
		ticker := time.NewTicker(1 * time.Second)
		for _ = range ticker.C {
			select {
			case <-ctx.Done():
				fmt.Println("child process interrupt...")
				return
			default:
				fmt.Println("enter default")
			}
		}
	}(timeoutCtx)
	select {
	case <-timeoutCtx.Done():
		time.Sleep(1 * time.Second)
		fmt.Println("main process exit!")
	}
	// time.Sleep(time.Second * 5)
}
```

# 锁

