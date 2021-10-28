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

