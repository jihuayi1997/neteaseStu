# Golang语法

0开头：八进制

0x开头：十六进制

***

%s 字符串

%d 十进制

%b 二进制

%o 八进制

%x 十六进制

%T 变量类型

%v 相应值默认格式

***

带符号整型：int8、int16、int32、int64 

无符号整型：uint8、uint16、uint32、uint64

int和uint：32位系统为uint32，64位系统为uint64

小数分为float64和float32，默认是float64，float32不能直接赋值给float64

complex64实部虚部各32，complex128实部虚部各64

uintptr：无符号整型，存放指针

bool默认false，不能转换其他类型

***

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

***

数组长度是数组类型的一部分，数组是值类型

切片可变长度，是引用类型，都指向底层数组，底层数组保存真正数据

切片长度是元素个数 len()

切片容量是底层数组从切片第一个元素到最后 cap()

判断切片是否空 用`len(a)==0`而不是`a==nil`

扩容：

​	首先判断若申请容量>2倍旧容量，最终容量就是新申请的容量

​	否则判断旧切片长度小于1024，最终容量是旧容量2倍

​	否则最终容量从旧容量开始循环增加原来的1/4，直到最终容量大于申请容量

​	最终容量计算值溢出，则最终容量就是新申请容量