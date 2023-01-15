# Go语言学习笔记

## 变量的声明

#### 短声明

```go
i:=0  //这种类型的 其实也是类型推断 一般写循环的时候可以用
for(i:=0;i<size;i++)
```

#### 让编译器推断

(注意是让编译器推断,这个推断是放在编译时的而不是运行时的,与python等不同)

```go
var variable1 = "string type"
var variable2 = 123
var variable3 = 123.45
```

#### 标准的变量声明格式

```go
var varName int = 123
```

## 常量的声明

常量是在编译的时候就必须确定数值的,编译器会逐行检查并替换(有点像C的宏)

所以这样的写法是不被允许的:

```cpp
const myConst = math.Sin(1.234)
```

编译器会报错: `Const initializer 'math.Sin(1.234)' is not a constant`

可以声明在函数外,比如

```go
const (
    _  = iota
    KB = 1 << (iota * 10)
    MB
    GB
    TB
    PB
    ZB
)

const (
    nothing = iota
    canRead = 1<<iota
    canWrite
    canExecute
)

func main() {
    size := 10010201.
    fmt.Printf("size is %.2f KB\n", size/KB)
    fmt.Printf("size is %.2f MB\n", size/MB)
    fmt.Printf("size is %.2f GB\n", size/GB)

    var admin byte = canRead | canWrite | canExecute
    fmt.Printf("%b\n",admin)
    fmt.Printf("admin canRead ? %v",admin&canRead==canRead)
}

/************运行结果*********
size is 9775.59 KB
size is 9.55 MB
size is 0.01 GB
1110
admin canRead ? true
*/
```

## 容器类

### 数组与切片

数组是不可变的  切片是可变的(类似于C++里的vector+python中的切片操作)

```go
//注意声明方式的区别
sliceA := []int{1, 2, 3, 4, 5}
arrA := [...]int{1, 2, 3, 4, 5}
sliceB := sliceA
sliceB[2]=100
arrB := arrA
arrB[3]=100
fmt.Println(sliceA)
fmt.Println(sliceB)
fmt.Println(arrA)
fmt.Println(arrB)

/*****结果****
[1 2 100 4 5]
[1 2 100 4 5]
[1 2 3 4 5]
[1 2 3 100 5]
*/
```

可以看出  对于数组,赋给一个新的变量相当于传值(深拷贝,全部复制一遍)  而对于切片,赋给一个新变量相当于传地址(浅拷贝)

切片也可以这样声明:

```go
    //使用make声明一个切片 第一个参数为有效元素个数(长度)  第二个参数为最大元素个数(容量)
    a:=make([]int,2,3)
    fmt.Printf("capacity %d\n",cap(a))
    fmt.Printf("len %d\n",len(a))

/*********
capacity 3
len 2
**/
```

### map

map的创建 遍历 与 删除键值对

```go
// 创建
m := make(map[string]int)
// init
m = map[string]int{
    "a": 1,
    "b": 2,
    "c": 3,
    "d": 4,
    "e": 5,
}
// 遍历(这里使用了匿名变量)
for _, v := range m {
    fmt.Println(v)
}
//add
m["new"] = 100
fmt.Println(m)
//delete
delete(m, "a")
fmt.Println(m)

/************

1
2
3
4
5
map[a:1 b:2 c:3 d:4 e:5 new:100]
map[b:2 c:3 d:4 e:5 new:100]

**/
```

注意,map赋值给一个新的变量是浅拷贝

map没有清空,要清空的话不如直接赋一个新的map,Go的垃圾回收比清空还快

### 广义零值nil

[Go语言nil：空值/零值 (biancheng.net)](http://c.biancheng.net/view/4776.html)

golang中的nil并不是一个关键字,而是一个广义的零值,不同类型的nil值大小是不一样的

```go
func main() {
    var p *struct{}
    fmt.Println( unsafe.Sizeof( p ) ) // 8
    var s []int
    fmt.Println( unsafe.Sizeof( s ) ) // 24
    var m map[int]bool
    fmt.Println( unsafe.Sizeof( m ) ) // 8
    var c chan string
    fmt.Println( unsafe.Sizeof( c ) ) // 8
    var f func()
    fmt.Println( unsafe.Sizeof( f ) ) // 8
    var i interface{}
    fmt.Println( unsafe.Sizeof( i ) ) // 16
}
```
