# Golang

## 基础语法

### 入门

go目录：bin，pkg，src

0开头：八进制

0x开头：十六进制

若需编译linux可执行文件

```shell
set GOOS=linux
set GOARCH=amd64
go build
```

```powershell
$env:GOOS="linux"
$env:GOARCH="amd64"
go build
```

### 格式控制符

%s 字符串

%d 十进制

%b 二进制

%o 八进制

%x 十六进制

%T 变量类型

%v 相应值默认格式

### 基本数据类型

带符号整型：int8、int16、int32、int64 

无符号整型：uint8、uint16、uint32、uint64

int和uint：32位系统为uint32，64位系统为uint64

小数分为float64和float32，默认是float64，float32不能直接赋值给float64

complex64实部虚部各32，complex128实部虚部各64

uintptr：无符号整型，存放指针

bool默认false，不能转换其他类型

### 字符串

多行字符串必须用反引号，反引号内转义字符无效，原样输出

字符串拼接：`s:=fmt.Sprintf(“%s%s”,name,word)`或`s:=name+word`

字符串分割：`s:=strings.Split(s1,” ”)`，返回字符串切片

```go
func main()  {
    //统计字符串中单词出现次数
	ss:="how do you do"
	m:=make(map[string]int,10)
	s:=strings.Split(ss," ")
	s=append(s,"do")
    fmt.Println(s)					//[how do you do do]
	for _,v:=range s{
		m[v]++
	}
	fmt.Println(m)					//map[do:3 how:1 you:1]
}
```

判断字符串包含：`flag:=strings.Contains(s,”hello”)`

判断字符串前缀 ：`flag:=strings.HasPrefix(s,”hello”)`

判断字符串后缀：`flag:=strings.HasSuffix(s,”hello”)`

子串第一次出现的位置：`strings.Index(s,”hello”)`

字串最后一次出现的位置：`strings.LastIndex(s,”hello”)`

字符串连接：`strings.Join(a[]string, sep string)`

go不可以直接修改字符串内容，若需要修改先转成切片然后建立新字符串接收转换

字符串：string（UTF-8。字符为ASCII上字符占1字节，其他字符2-4个字节）

字符：uint8--byte(ASCII)，int32--rune(UTF-8)

判断字符是否为中文：`flag:=unicode.Is(unicode.Han)`

### 数组

初始化：`arr := [...][2]int{{1,2},{3,4},{5,6}}`

数组长度必须是常量，长度是数组类型的一部分，大小不可修改

Go语言支持多维数组，但多维数组只有最外层可以使用`...`来让编译器推导长度

数组是值类型，赋值传参会复制整个数组，改变副本不影响本身

### 切片

切片可变长度，是引用类型，都指向底层数组，底层数组保存真正数据

```go
func main() {
	a := []int{1, 2, 3, 4, 5}
	b := a		   //共享一个底层数组
	fmt.Println(a) //[1 2 3 4 5]
	fmt.Println(b) //[1 2 3 4 5]
	b[0] = 1000
	fmt.Println(a) //[1000 2 3 4 5]
	fmt.Println(b) //[1000 2 3 4 5]
}
```

切片之间不能比较，只能和`nil`比较，`nil`值的切片无底层数组，长度容量都是0

```go
var s1 []int         //声明整型切片，len(s1)=0;cap(s1)=0;s1==nil
s2 := []int{}        //构造整型切片，len(s2)=0;cap(s2)=0;s2!=nil
s3 := make([]int, 0) //构造整型切片，len(s3)=0;cap(s3)=0;s3!=nil
```

构造切片：直接构造；基于数组构造切片；切片再切片；`make([]T,size,cap)`，其中`cap`默认`==len`

切片长度：`len(a)`，元素个数

切片容量：`cap(a)`，底层数组从切片第一个元素到最后

判断切片是否空：`len(a)==0`而不是`a==nil`

为切片追加元素：`a = append(a,ss...)`，支持追加到nil

扩容策略：

​	首先判断若申请容量>2倍旧容量，最终容量就是新申请的容量

​	否则判断旧切片长度小于1024，最终容量是旧容量2倍

​	否则最终容量从旧容量开始循环增加原来的1/4，直到最终容量大于申请容量

​	最终容量计算值溢出，则最终容量就是新申请容量

切片的复制：`copy(dest,src)`，可以将一个切片的数据复制到另一个切片空间中

```go
func main() {
	a := []int{1, 2, 3, 4, 5}
	c := make([]int, 5, 5)
	copy(c, a)     //使用copy()函数将切片a中的元素复制到切片c
	fmt.Println(a) //[1 2 3 4 5]
	fmt.Println(c) //[1 2 3 4 5]
	c[0] = 1000
	fmt.Println(a) //[1 2 3 4 5]
	fmt.Println(c) //[1000 2 3 4 5]
}
```

从切片中删除元素：Go没有专用方法，可以用`a=append(a[:index],a[index+1:]...)`

```go
func main() {
    x := [...]int{1,3,5} 		//数组
    a := x[:]			 		//切片
								//要删除索引为1的元素
    a = append(a[:1], a[2:]...) //此时修改了底层数组(向前覆盖)
	fmt.Println(a)		 		//[1,5]
    fmt.Println(x)		 		//[1,5,5]
}
```

对切片排序：`sort.Ints(a)`

元素可以为map类型：

```go
func main() {
	var mapSlice = make([]map[string]string, 3)
	for index, value := range mapSlice {
		fmt.Printf("index:%d value:%v\n", index, value)
	}
	fmt.Println("after init")
	// 对切片中的map元素进行初始化
	mapSlice[0] = make(map[string]string, 10)
	mapSlice[0]["name"] = "小王子"
	mapSlice[0]["password"] = "123456"
	mapSlice[0]["address"] = "沙河"
	for index, value := range mapSlice {
		fmt.Printf("index:%d value:%v\n", index, value)
	}
}
```

### 内存申请

&...*

new：一般用来给基本数据类型申请内存，返回对应类型指针

make：只用于slice, map, chan的内存创建，返回这三个类型本身

不支持指针运算

### map

Go提供映射关系容器为map，内部使用散列表(hash)实现

map是无序的基于key-value的数据结构，Go语言map是引用类型，初始化才能使用

构造map：`make(map[KetType]ValueType,[cap])`

判断某个键是否存在：`value, ok := map[key]`

删除某组键值对：`delete(map,key)`，删除不存在的也无所谓

按照指定顺序遍历map：

```go
func main() {
	rand.Seed(time.Now().UnixNano()) //初始化随机数种子

	var scoreMap = make(map[string]int, 200)

	for i := 0; i < 100; i++ {
		key := fmt.Sprintf("stu%02d", i) //生成stu开头的字符串
		value := rand.Intn(100)          //生成0~99的随机整数
		scoreMap[key] = value
	}
	//取出map中的所有key存入切片keys
	var keys = make([]string, 0, 200)
	for key := range scoreMap {
		keys = append(keys, key)
	}
	//对切片进行排序
	sort.Strings(keys)
	//按照排序后的key遍历map
	for _, key := range keys {
		fmt.Println(key, scoreMap[key])
	}
}
```

值可以为切片类型：

```go
func main() {
	var sliceMap = make(map[string][]string, 3)
	fmt.Println(sliceMap)
	fmt.Println("after init")
	key := "中国"
	value, ok := sliceMap[key]
	if !ok {
		value = make([]string, 0, 2)
	}
	value = append(value, "北京", "上海")
	sliceMap[key] = value
	fmt.Println(sliceMap)
}
```

### 函数

```go
func calculate(x,y int, m string) (ret1 int, ret2 string){
    ret1 = x+y
    ret2 = m
    return			//使用命名的返回值，return后面可以省略返回值变量
}
```

支持类型简写

支持可变长参数：参数名后加`...`，实际类型为切片，放在函数参数的最后

支持多返回值

不支持默认参数

在一个命名函数中不能再声明命名函数

```go
func main(){
    //正常匿名函数
    f1:=func(x,y int){
        fmt.Println(x+y)
    }
    //只调用一次也可以简写成立即执行函数
    func(x,y int){
        fmt.Println(x+y)
    }(100,200)
}
```

作用域：全局作用域，函数作用域，语句块作用域

defer语句：把后面语句延迟到函数即将返回时执行，多个defer则按先进后出顺序执行（file, 数据库, socket...）

```go
func f1() int {
	x := 5
	defer func() {
		x++
	}()
	return x
}
func f2() (x int) {
	defer func() {
		x++
	}()
	return 5
}
func f3() (y int) {
	x := 5
	defer func() {
		x++
	}()
	return x
}
func f4() (x int) {
	defer func(x int) {
		x++
	}(x)
	return 5
}
func main() {
	fmt.Println(f1())						//5
	fmt.Println(f2())						//6
	fmt.Println(f3())						//5
    fmt.Println(f4())						//5
}
```

函数类型：可以作为参数和返回值

```go
func f1(x func() int) func(int, int) int {	//参数类型func()int，返回值类型func(int,int)int
    ret := func(a,b int)int{
        return a + b
    }
    return ret
}
```

闭包：