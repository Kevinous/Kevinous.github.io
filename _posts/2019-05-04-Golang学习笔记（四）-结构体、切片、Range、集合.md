[TOC]

# Go语言笔记

## 结构体

### 定义

``` go
type struct_variable_type struct{
    member1 type
    member2 type
    ...
}
//type语句是设定类型
//比如之前的 type cb func(int) int  就是命名cb为一个函数的类型

//定义了结构体类型后可以声明结构体变量
variable_name := struct_variable_type {val1, val2, ...}
//或者
variable_name := struct_variable_type {key1: val1, key2: val2, ...}
```



实例如下

```go
package main

import "fmt"

type Books struct {
   title string
   author string
   subject string
   book_id int
}


func main() {

    // 创建一个新的结构体
    fmt.Println(Books{"Go 语言", "www.runoob.com", "Go 语言教程", 6495407})

    // 也可以使用 key => value 格式
    fmt.Println(Books{title: "Go 语言", author: "www.runoob.com", subject: "Go 语言教程", book_id: 6495407})

    // 忽略的字段为 0 或 空
   fmt.Println(Books{title: "Go 语言", author: "www.runoob.com"})
}
```

### 访问结构体成员

```go
struct_name.member_name

//上面的初始化也可以这样进行！
func main(){
    var Book1 Books
    var Book2 Books
    
    Book1.title = "Golang"
    Book1.author = "zyx"
    ...
}
```

### 结构体作为函数参数

只需要在函数参数中加入定义的结构体类型即可

```go
func printBook( book Books ){
    ...
}
```

### 结构体指针

可以同样定义结构体的指针

```go
var struct_pointer *Books

struct_pointer = &Book1	//不像C，类似对象
//作用体现在函数的形参传参和传址上
//不用指针就是传值，不改变原值；用指针就是传址，改变原值
```

## 切片

切片是对数组的抽象

==其实就是之前说的不给定size的数组==

Go中数组长度不可变，因此提供了一种灵活的内置类型切片（“动态数组"）

与数组相比切片的长度==不固定==，可以追加元素

### 定义

```go
var identifier []type
```

切片不需要说明长度

或者使用make()函数创建

```go
var slice []type = make([]type, len)
//也可以简写为
slice1 := make([]type, len)
```

也可以指定容量，capacity为可选参数

```go
make([]type, length, capacity)
```

*len为数组长度同时也是切片的初始长度*

### 初始化切片

#### 方式一

直接初始化切片，其中cap = len = 3

```go
s := []int {1,2,3}
```

#### 方式二

初始化切片s，是数组arr的引用

```go
s := arr[:]
```

#### 方式三

通过数组切片来初始化

```go
s := arr[startIndex : endIndex]

s := arr[startIndex : ]//表示一直到最后

s:= arr[: endIndex]//表示从第一个开始

s1 := s[startIndex : endIndex]//利用切片s1初始化切片s
```

#### 方式三.5

make也会初始化

```go
s :=make([]int,len,cap) 
```

### len()和cap()

可以通过len()获取长度

通过cap()计算容量，即测量切片最长可以到达多少

### 空切片 

未被初始化的时候默认为nil，长度为0

### 切片截取

可以通过设置下限和上限来截取切片[lower-bound:upper-bound]

```go
package main

import "fmt"

func main() {
   /* 创建切片 */
   numbers := []int{0,1,2,3,4,5,6,7,8}   
   printSlice(numbers)

   /* 打印原始切片 */
   fmt.Println("numbers ==", numbers)

   /* 打印子切片从索引1(包含) 到索引4(不包含)*/
   fmt.Println("numbers[1:4] ==", numbers[1:4])

   /* 默认下限为 0*/
   fmt.Println("numbers[:3] ==", numbers[:3])

   /* 默认上限为 len(s)*/
   fmt.Println("numbers[4:] ==", numbers[4:])

   numbers1 := make([]int,0,5)
   printSlice(numbers1)

   /* 打印子切片从索引  0(包含) 到索引 2(不包含) */
   number2 := numbers[:2]
   printSlice(number2)

   /* 打印子切片从索引 2(包含) 到索引 5(不包含) */
   number3 := numbers[2:5]
   printSlice(number3)

}

func printSlice(x []int){
   fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)
}
```

### append()和copy()函数——扩容

如果想增加切片的容量，我们必须创建一个更大的切片并且把原切片的内容都拷贝过来。

```go
package main

import "fmt"

func main() {
   var numbers []int
   printSlice(numbers)

   /* 允许追加空切片 */
   numbers = append(numbers, 0)
   printSlice(numbers)

   /* 向切片添加一个元素 */
   numbers = append(numbers, 1)
   printSlice(numbers)

   /* 同时添加多个元素 */
   numbers = append(numbers, 2,3,4)
   printSlice(numbers)

   /* 创建切片 numbers1 是之前切片的两倍容量*/
   numbers1 := make([]int, len(numbers), (cap(numbers))*2)

   /* 拷贝 numbers 的内容到 numbers1 */
   copy(numbers1,numbers)
   printSlice(numbers1)   
}

func printSlice(x []int){
   fmt.Printf("len=%d cap=%d slice=%v\n",len(x),cap(x),x)
}
```

## Range范围

用于for循环中迭代数组、切片、通道或集合的元素，在数组和切片中返回元素的索引和对应的索引值，在集合中返回key-value对的key。

```go
package main
import "fmt"
func main() {
    //这是我们使用range去求一个slice的和。使用数组跟这个很类似
    nums := []int{2, 3, 4}
    sum := 0
    for _, num := range nums {
        sum += num
    }
    fmt.Println("sum:", sum)
    //在数组上使用range将传入index和值两个变量。上面那个例子我们不需要使用该元素的序号，所以我们使用空白符"_"省略了。有时侯我们确实需要知道它的索引。
    for i, num := range nums {
        if num == 3 {
            fmt.Println("index:", i)
        }
    }
    //range也可以用在map的键值对上。
    kvs := map[string]string{"a": "apple", "b": "banana"}
    for k, v := range kvs {
        fmt.Printf("%s -> %s\n", k, v)
    }
    //range也可以用来枚举Unicode字符串。第一个参数是字符的索引，第二个是字符（Unicode的值）本身。
    for i, c := range "go" {
        fmt.Println(i, c)
    }
}
```

输出为

```go
sum: 9
index: 1
a -> apple
b -> banana
0 103
1 111
```

## 集合Map

Map是一种无序的键值对的集合，通过key键值来快速检索数据，key类似于索引，指向数据的值。

*Map是使用hash表实现的*

### 定义Map

可以使用内建函数Make也可以使用map关键字来定义

```go
/* 声明变量，默认 map 是 nil */
var map_variable map[key_data_type]value_data_type

/* 使用 make 函数 */
map_variable := make(map[key_data_type]value_data_type)
```

*若不初始化则会创建一个nil map，它不能用来存放键值对。*

下面例子展示了简单的使用

```go
package main

import "fmt"

func main() {
    var countryCapitalMap map[string]string /*创建集合 */
    countryCapitalMap = make(map[string]string)

    /* map插入key - value对,各个国家对应的首都 */
    countryCapitalMap [ "France" ] = "Paris"
    countryCapitalMap [ "Italy" ] = "罗马"
    countryCapitalMap [ "Japan" ] = "东京"
    countryCapitalMap [ "India " ] = "新德里"

    /*使用键输出地图值 */ 
    for country := range countryCapitalMap {
        fmt.Println(country, "首都是", countryCapitalMap [country])
    }
    //这里也可以 for country, capital := range ...
    //那么循环体内就不用再利用key从map里找value了
    
    
    /*查看元素在集合中是否存在 */
    capital, ok := countryCapitalMap [ "美国" ] /*如果确定是真实的,则存在,否则不存在 */
    /*fmt.Println(capital) */
    /*fmt.Println(ok) */
    if (ok) {
        fmt.Println("美国的首都是", capital)
    } else {
        fmt.Println("美国的首都不存在")
    }
}
```

### delete()函数

用于删除集合的元素，参数为map和对应的key

```go
package main

import "fmt"

func main() {
        /* 创建map */
        countryCapitalMap := map[string]string{"France": "Paris", "Italy": "Rome", "Japan": "Tokyo", "India": "New delhi"}

        fmt.Println("原始地图")

        /* 打印地图 */
        for country := range countryCapitalMap {
                fmt.Println(country, "首都是", countryCapitalMap [ country ])
        }

        /*删除元素*/ delete(countryCapitalMap, "France")
        fmt.Println("法国条目被删除")

        fmt.Println("删除元素后地图")

        /*打印地图*/
        for country := range countryCapitalMap {
                fmt.Println(country, "首都是", countryCapitalMap [ country ])
        }
}
```
