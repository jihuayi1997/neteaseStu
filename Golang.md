# Golang语法

### 入门

go目录：bin，pkg，src

0开头：八进制

0x开头：十六进制

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

字符串分割：`s:=strings.Split(s1,”\\”)`

判断字符串包含：`flag:=strings.Contains(s,”hello”)`

判断字符串前缀 ：`flag:=strings.HasPrefix(s,”hello”)`

判断字符串后缀：`flag:=strings.HasSuffix(s,”hello”)`

子串第一次出现的位置：`strings.Index(s,”hello”)`

字串最后一次出现的位置：`strings.LastIndex(s,”hello”)`

字符串连接：`strings.Join(a[]string, sep string)`

go不可以直接修改字符串内容，若需要修改先转成切片然后建立新字符串接收转换

字符串：string（UTF-8。字符为ASCII上字符占1字节，其他字符2-4个字节）

字符：uint8--byte(ASCII)，int32--rune(UTF-8)

### 数组

数组长度必须是常量，长度是数组类型的一部分，大小不可修改

Go语言支持多维数组，但多维数组只有第一层可以使用`...`来让编译器推导长度

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

构造切片：直接构造；基于数组构造切片；切片再切片；`make([]T,size,cap)`，其中`cap`默认`==size`

切片长度：`len(a)`，元素个数

切片容量：`cap(a)`，底层数组从切片第一个元素到最后

判断切片是否空：`len(a)==0`而不是`a==nil`

为切片追加元素：`a = append(a,ss...)`

扩容策略：

​	首先判断若申请容量>2倍旧容量，最终容量就是新申请的容量

​	否则判断旧切片长度小于1024，最终容量是旧容量2倍

​	否则最终容量从旧容量开始循环增加原来的1/4，直到最终容量大于申请容量

​	最终容量计算值溢出，则最终容量就是新申请容量

切片的复制：`copy(dest,src)`，可以将一个切片数据复制到另一个切片空间中

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

