# 一、Golang

## 1. 引入

准备工作

```cmd
go version
go env -w GOPROXY=https://goproxy.cn,direct
```

创建hello项目

```cmd
mkdir hello
cd hello
go mod init hello
```

创建hello.go

```go
package main									// main包，表明当前是一个可执行程序

import "fmt"									// 导入内置fmt包

func main() {									// main函数，是程序执行的入口
    fmt.Println("Hello, World!")
}
```

编译hello项目

```cmd
// 1. win to win
go build
// 2. win to Linux
set GOOS=linux
set GOARCH=amd64
go build
// 3. win to mac
SET GOOS=darwin
SET GOARCH=amd64
go build
```

## 2. 数据类型

```go
// "var name =" 在函数体内可与 "name :=" 等价替换
// 定义值类型（自动初始化）：int:0,  bool:false,  string:"",  数组按长度自动填充,  结构体内字段自动填充
// 定义指针类型（自动置为nil），用new定义（自动初始化）：即指向的内存自动填充
// 定义引用类型（自动置为nil），用make定义（自动初始化）：slice:按长度自动填充,  map:[],  chan:传递对应类型的内存通道
```

### (1). 变量

* 标准类型
  * 带符号整型：int8  int16  int32  int64  int(32位系统为int32，64位系统为int64)
  * 无符号整型：uint8  uint16  uint32  uint64  uint(32位系统为uint32，64位系统为uint64)
  * 浮点型：float32  float64  complex64（实部和虚部为32位）  complex128（实部和虚部为64位）
  * 布尔型：true  false（不能转为其他类型）
  * 指针型：uintptr
  * 字符串：string
* 自定义类型：`type ages int`，`type money float32`，`type months map[string]int`
* 类型别名：`rune`和`byte`就是类型别名，他们的定义如下：`type byte = uint8`，`type rune = int32`
* 定义：`var name type`（可批量，自动初始化）
* 定义并初始化：
  * `var name = value`
* 匿名变量：_，多用于占位，表示忽略值，不占用内存
* 格式控制符：%s: 字符串  %d: 十进制  %b: 二进制  %o: 八进制  %x: 十六进制  %T: 变量类型  %v: 相应值默认格式

### (2). 常量

* 定义并初始化：`const pi = 3.1415`（可批量，如果省略值表示和上一行相同）
* 常量计数器：iota，遇const重置为0，const中每新增一行常量声明使iota计数一次

```go
func main() {
    const (
            a = 1000   					// 1000
        	b          					// 1000
        	c = iota   					// 2
        	d = "ha"   					// "ha"
        	e          					// "ha"
        	f = 100    					// 100
        	g          					// 100
        	h = iota   					// 7
        	i          					// 8
    )
    fmt.Println(a,b,c,d,e,f,g,h,i)		// 1000 1000 2 ha ha 100 100 7 8
}
```

### (3). 字符串

* 多行字符串必须用反引号，反引号内转义字符无效，原样输出
* 不可以直接修改字符串内容，若需要修改先转成切片然后建立新字符串接收转换
* 字符：uint8--byte(ASCII)，int32--rune(UTF-8)
* 字符串：string（UTF-8。字符为ASCII上字符占1字节，其他字符2-4个字节）
* 字符串拼接：`s:=fmt.Sprintf(“%s%s”,name,word)`或`s:=name+word`
* 字符串分割：`s:=strings.Split(s1,” ”)`，返回字符串切片

```go
func main()  {
    // 统计字符串中单词出现次数
	ss:="how do you do"
	m:=make(map[string]int,10)
	s:=strings.Split(ss," ")
	s=append(s,"do")
    fmt.Println(s)					// [how do you do do]
	for _,v:=range s{
		m[v]++
	}
	fmt.Println(m)					// map[do:3 how:1 you:1]
}
```

* 判断字符串包含：`flag:=strings.Contains(s,”hello”)`
* 判断字符串前缀 ：`flag:=strings.HasPrefix(s,”hello”)`
* 判断字符串后缀：`flag:=strings.HasSuffix(s,”hello”)`
* 子串第一次出现的位置：`strings.Index(s,”hello”)`
* 字串最后一次出现的位置：`strings.LastIndex(s,”hello”)`
* 字符串切片拼接：`strings.Join(a[]string, sep string)`

* 判断字符是否为中文：`flag:=unicode.Is(unicode.Han)`

### (4). 数组

* 定义：`var arr [3]int`（自动初始化）
* 定义并初始化：
  * `var arr = [...][2]int{{1,2},{3,4},{5,6}}`
* 数组长度必须是常量，长度是数组类型的一部分，大小不可修改
* Go语言支持多维数组，但多维数组只有最外层可以使用`...`来让编译器推导长度

```go
func main() {
	var a = [...]string{"北京", "上海", "深圳"}
	// 方法1：for循环遍历
	for i := 0; i < len(a); i++ {
		fmt.Println(a[i])
	}

	// 方法2：for range遍历
	for i, v := range a {
		fmt.Println(i, v)
	}
}
```

* 指针数组：[3]\*int，数组指针：\*[3]int
* 数组是值类型，支持 "=="、"!=" 操作符，赋值传参会复制整个数组，改变副本不影响本身

```go
func modifyArray(x [3][2]int) {
	x[2][0] = 100
}

func main() {
	a := [3][2]int{
		{1, 1},{1, 1},{1, 1},
	}
	modifyArray(a) 					// 在modify中修改的是a的副本x
	fmt.Println(a)  				// [[1 1] [1 1] [1 1]]
}
```

### (5). 结构体

* 结构体占用一块连续的内存，空结构体不占用空间

* 声明：

```go
type person struct {
	name, city string
	age        int8
}
```

* 定义：
  * `var p1 person`（自动初始化）
  * `var p1 = new(person)`（自动初始化）（虽然返回结构体指针，一般用`(*p1).name = "wang"`但也支持`p1.name = "wang"`的方式赋值）
  
* 定义并初始化：（若按顺序初始化所有字段时，则key可省略）

  * `P := person{age:24, name:"Tom"}`

  * `p := struct{name string; age int8}{name: "liming", age:18}`（不再声明，采用匿名结构体的方式）

  * `func newPerson(a string, b int8) person { p := person{Name: a, Age: b}; return p }`（先写个构造函数包装）

    `p := newPerson("liming", 18)`（再调用构造函数）

* 支持匿名字段，嵌套结构体（字段和method可以继承和重载，被重载的进入内层）

```go
type human struct {
	name  string
	age   int
	phone string	 	// human的phone字段
}

type student struct {
	human  				// 匿名字段human, 那么student继承了human所有字段及method
	school string
}

type employee struct {
	human  			 	// 匿名字段human, 那么employee继承了human所有字段及method
	company string
    phone   string 		// employee的phone字段(重载), 被重载的phone成为human.phone
}

// human的method
func (h *human) SayHi() {
	fmt.Printf("Hi, I am %s you can call me on %s\n", h.name, h.phone)
}

// employee的method(重载), 被重载的SayHi()成为human.SayHi()
func (e *employee) SayHi() {
	fmt.Printf("Hi, I am %s, I work at %s. Call me on %s. My personal phone is:%s\n", e.name,
		e.company, e.phone, e.human.phone)
}

func main() {
	mark := student{human{"Mark", 25, "222-222-YYYY"}, "MIT"}
	Bob := employee{human{"Bob", 34, "777-444-XXXX"}, "Golang Inc", "333-222"}
	mark.SayHi()
	Bob.SayHi()
    Bob.human.SayHi()
}
```
* 支持序列化与反序列化

```go
// 声明结构体person, 提出解析要求（注意结构体内字段首字母大写，不然json包读取不到）
type person struct {
	Name string `json:"name" db:"name" ini:"name"`
	Age  int8   `json:"age"`
}

func main() {
	// 定义并初始化 p1
	p1 := person{Name: "liming", Age: 18}
	fmt.Printf("%+v\n", p1)											// %+v, 字段+值, {Name:liming Age:18}
    
    // 序列化 参数类型: 结构体 返回类型: []byte, err
	jsonbyte, err := json.Marshal(p1)
	if err != nil {
		fmt.Printf("Marshal failed, err:%v", err)
	}
	fmt.Println(string(jsonbyte))									// {"name":"liming","age":18}
    
    // 反序列化 参数类型: []byte, 接收的结构体指针 返回类型: err
	jsonstr := `{"name":"liming","age":18}`
	var p2 person
	err = json.Unmarshal([]byte(jsonstr), &p2)
	if err != nil {
		fmt.Printf("Unmarshal failed, err:%v", err)
	}
	fmt.Printf("%#v", p2)											// %#v, 名+字段+值, main.person{Name:"liming", Age:18}
}
```

### (6). 指针（指针类型）

* 定义：
  * `var name *type`（自动置为nil）
  * `var name = new(type)`（自动初始化）
* 定义并初始化：
  * `var name = &a`
* 不支持指针运算，取地址：'&'，指针取值：'*'
* new：一般用来给基本数据类型申请内存，返回对应类型指针
* make：只用于slice, map, chan的内存创建，返回这三个类型本身
* Go知道你要做的一切，会自动转换：
  * 如果一个method的receiver是\*T，可以在一个T类型的实例变量V上面调用这个method，而不需要&V去调用这个method
  * 如果一个method的receiver是T，可以在一个\*T类型的变量P上面调用这个method，而不需要\*P去调用这个method

### (7). 切片（引用类型）

* 引入：有点类似动态数组

* 定义：
  * `var name []type`（自动置为nil）
  * `var name = make([]type, len, cap)`（自动初始化）
* 定义并初始化：
  * `var name = []int{1,2,3,4}`
  * `var a = s[low:high]`（切片表达式，左闭右开，low缺省为0，high缺省为len(s)，支持基于数组构造切片，切片再切片）

```go
func main() {
    // 基于数组构造切片
	a := [5]int{1, 2, 3, 4, 5}
	s := a[1:3]															// high<=数组长度
	fmt.Printf("s:%v len(s):%v cap(s):%v\n", s, len(s), cap(s))			// s:[2 3] len(s):2 cap(s):4
    
    // 切片再切片
    s1 := s[:]
    s2 := s[0:4]														// high<=切片容量
    fmt.Printf("s1:%v len(s1):%v cap(s1):%v\n", s1, len(s1), cap(s1))	// s1:[2 3] len(s1):2 cap(s1):4
    fmt.Printf("s2:%v len(s2):%v cap(s2):%v\n", s2, len(s2), cap(s2))	// s2:[2 3 4 5] len(s2):4 cap(s2):4
}
```

* 切片之间不能比较，只能和`nil`比较，`nil`值的切片无底层数组，长度容量都是0

```go
// 切片长度：len(a)，元素个数
// 切片容量：cap(a)，底层数组从切片第一个元素到最后，用make缺省定义时数值同len
// 判断切片是否为空切片：len(a)==0
var s1 []int         		// 定义整型切片，置为nil，len(s1)=0;cap(s1)=0;s1==nil
s2 := make([]int, 0)        // 定义整形切片，自动初始化，len(s2)=0;cap(s2)=0;s2!=nil
s3 := []int{} 				// 定义并初始化整型空切片，len(s3)=0;cap(s3)=0;s3!=nil
```

* 指向底层数组，底层数组保存真正数据

```go
func main() {
	a := []int{1, 2, 3, 4, 5}
	b := a		   			// 共享一个底层数组
	fmt.Println(a) 			// [1 2 3 4 5]
	fmt.Println(b) 			// [1 2 3 4 5]
	b[0] = 1000
	fmt.Println(a) 			// [1000 2 3 4 5]
	fmt.Println(b) 			// [1000 2 3 4 5]
}
```

* 为切片追加元素：`a = append(a,1)`，支持追加到nil

* 扩容策略：
  * 首先判断若申请容量>2倍旧容量，最终容量就是新申请的容量
  * 否则判断旧切片长度小于1024，最终容量是旧容量2倍
  * 否则最终容量从旧容量开始循环增加原来的1/4，直到最终容量大于申请容量
  * 最终容量计算值溢出，则最终容量就是新申请容量

```go
func main(){
	var s []int
	s1 = append(s1, 1)        // [1]，可以添加一个元素
	s1 = append(s1, 2, 3, 4)  // [1 2 3 4]，可以添加多个元素
	s2 := []int{5, 6, 7}  
	s1 = append(s1, s2...)    // [1 2 3 4 5 6 7]，可以添加另一个切片中的元素（后面加…）
}
```

* 切片的复制：`copy(dest,src)`，将一个切片的数据复制到另一个切片空间中

```go
func main() {
	a := []int{1, 2, 3, 4, 5}
	c := make([]int, 5, 5)
	copy(c, a)     			// 使用copy()函数将切片a中的元素复制到切片c
	fmt.Println(a) 			// [1 2 3 4 5]
	fmt.Println(c) 			// [1 2 3 4 5]
	c[0] = 1000
	fmt.Println(a) 			// [1 2 3 4 5]
	fmt.Println(c) 			// [1000 2 3 4 5]
}
```

* 从切片中删除元素：Go没有专用方法，可以用`a=append(a[:index],a[index+1:]...)`

```go
func main() {
    x := [...]int{1,3,5,7,9} 		// 数组
    a := x[1:5]			 			// 切片[3,5,7,9]
									// 要删除切片中索引为1的元素
    a = append(a[:1], a[2:]...) 	// 此时修改了切片也即修改了底层数组
	fmt.Println(a)		 			// [3,7,9]
    fmt.Println(x)		 			// [1,3,7,9,9]
}
```

* 对切片排序：`sort.Ints(a)`

* 元素可以为map类型：

```go
func main() {
	var mapSlice = make([]map[string]string, 3)						// make了len为3的切片，自动填充3个map
	for i, v := range mapSlice {
		fmt.Printf("index:%d value:%v nil:%v\n", i, v, v == nil)	// index:0 value:map[] nil:true
	}																// index:1 value:map[] nil:true
	fmt.Println("after init")										// index:2 value:map[] nil:true
    
	// 对切片中的前两个map进行初始化
	mapSlice[0] = make(map[string]string, 10)
    mapSlice[1] = make(map[string]string, 10)
	mapSlice[0]["name"] = "王"
	mapSlice[0]["password"] = "1"
	mapSlice[0]["address"] = "北"
	for i, v := range mapSlice {
		fmt.Printf("index:%d value:%v nil:%v\n", i, v, v == nil)	// index:0 value:map[address:北 name:王 password:1] nil:false
	}																// index:1 value:map[] nil:false
}																	// index:2 value:map[] nil:true
```

### (8). map（引用类型）

* 提供映射关系容器为map，内部使用散列表(hash)实现，无序的基于key-value的数据结构
* 定义：
  * `var name map[key_type]value_type`（自动置为nil）
  * `var name = make(map[key_type]value_type,cap)`（自动初始化）
* 定义并初始化：`var name = map[int]string{1: "C++", 2: "Golang"}`
* 判断某个键是否存在：`value, ok := map[key]`

```go
func main() {
	scoreMap := make(map[string]int)
    // 添加两个元素，此方式也可修改某key对应的value
	scoreMap["张三"] = 90
	scoreMap["小明"] = 100
    // 若key存在: ok为true, v为对应的值
    // 若key不存在: ok为false, v为值类型的零值
	v, ok := scoreMap["张三"]
	if ok {
		fmt.Println(v)
	} else {
		fmt.Println("查无此人")
	}
}
```

* 删除某组键值对：`delete(map,key)`，删除不存在的也无所谓
* 按照指定顺序遍历map：把key移至slice，对slice排序

```go
func main() {
	rand.Seed(time.Now().UnixNano()) 		// 初始化随机数种子

	var scoreMap = make(map[string]int, 200)

	for i := 0; i < 100; i++ {
		key := fmt.Sprintf("stu%02d", i) 	// 生成stu开头的字符串
		value := rand.Intn(100)          	// 生成0~99的随机整数
		scoreMap[key] = value
	}
	// 取出map中的所有key存入切片keys
	var keys = make([]string, 0, 200)
	for key := range scoreMap {
		keys = append(keys, key)
	}
	// 对切片进行排序
	sort.Strings(keys)
	// 按照排序后的key遍历map
	for _, key := range keys {
		fmt.Println(key, scoreMap[key])
	}
}
```

* 值可以为slice类型：

```go
func main() {
	var sliceMap = make(map[string][]string, 3)			// make了cap为3的空map[]
	fmt.Println(sliceMap, sliceMap == nil)				// map[] false
	fmt.Println("after init")
	key := "中国"
	value, ok := sliceMap[key]
	if !ok {
		value = make([]string, 0, 2)					// make了len为0的切片
		value = append(value, "北京", "上海")
		sliceMap[key] = value
	}
	fmt.Println(sliceMap)								// map[中国:[北京 上海]]
}
```

## 3. 函数

### (1). 普通函数

**基础**

```go
func calculate(x, y int, m string, z ...int) (ret1 int, ret2 string) { 	// 支持参数类型简写
	sum := 0															// 支持可变长参数：...type，放在参数列表最后，传进去被存为切片
	for _, v := range z {												// 支持多返回值
		sum = sum + v
	}
	ret1 = x + y + sum
	ret2 = m
	return 																// 使用命名的返回值，return后面可以省略返回值变量
}

func main() {
	fmt.Println(calculate(1, 2, "nihao", 3, 4, 5))						// 15 nihao
}
```

* 不支持默认参数，在一个命名函数中不能再声明命名函数，只可以匿名


```go
func main(){
    // 正常匿名函数并调用
    var f1 = func(x,y int){
        fmt.Println(x+y)
    }
    f1(100, 200)
    // 只调用一次也可以简写成立即执行函数
    func(x,y int){
        fmt.Println(x+y)
    }(100, 200)
}
```

* 作用域：全局作用域，函数作用域，语句块作用域
* 函数类型：可以作为参数和返回值

```go
func f1(x func() int) func(int, int) int {  // 参数类型: func() int，返回值类型: func(int,int) int
    ret := func(a,b int)int{
        return a + b
    }
    return ret
}
```

**defer**

* `defer`把后面语句延迟到函数即将返回时执行，多个defer则按先进后出顺序执行（file, 数据库, socket...）

```go
func calc(index string, a, b int) int {
	ret := a + b
	fmt.Println(index, a, b, ret)
	return ret
}

func main() {
	x := 1
	y := 2
	defer calc("AA", x, calc("A", x, y))	// 先calc("A", 1, 2): A 1 2 3, 再defer calc("AA", 1, 3)
	x = 10
	defer calc("BB", x, calc("B", x, y))	// 先calc("B", 10, 2): B 10 2 12, 再defer calc("BB", 10, 12)
	y = 20
}											// return之前执行defer：BB 10 12 22, AA 1 3 4
```

* `return`在底层并不是原子操作，它分为**给返回值赋值**和**RET指令**两步

  `defer`语句执行的时机就在返回值赋值操作后，RET指令执行前

```go
func f1() int {						// 返回值 = x, x++, RET返回值
	x := 5
	defer func() {
		x++
	}()
	return x
}
func f2() (x int) {					// 返回值x = 5, x++, RET返回值x
	defer func() {
		x++
	}()
	return 5
}
func f3() (y int) {					// 返回值y = x, x++, RET返回值y
	x := 5
	defer func() {
		x++
	}()
	return x
}
func f4() (x int) {					// 返回值x = 5, x的副本++, RET返回值x
	defer func(x int) {
		x++
	}(x)
	return 5
}
func main() {
	fmt.Println(f1())				// 5
	fmt.Println(f2())				// 6
	fmt.Println(f3())				// 5
    fmt.Println(f4())				// 5
}
```

**闭包**

函数 + 引用环境

```go
func calc(base int) (func(int) int, func(int) int) {
	add := func(i int) int {
		base += i
		return base
	}

	sub := func(i int) int {
		base -= i
		return base
	}
	return add, sub
}

func main() {
	f1, f2 := calc(10)				// f1, f2即闭包, base为f1及f2的一个公有引用环境
	fmt.Println(f1(1), f2(2)) 		// 11 9
	fmt.Println(f1(3), f2(4)) 		// 12 8
	fmt.Println(f1(5), f2(6)) 		// 13 7
    f3, f4 := calc(20)        		// f3, f4即闭包, base为f3及f4的一个公有引用环境
	fmt.Println(f3(1), f4(2)) 		// 21 19
	fmt.Println(f3(3), f4(4)) 		// 22 18
	fmt.Println(f3(5), f4(6)) 		// 23 17
}
```

**错误处理**

`recover()`必须搭配`defer`使用，`defer`一定要在可能引发`panic`的语句之前定义

```go
func funcA() {
	fmt.Println("func A")
}

func funcB() {
	defer func() {
		err := recover()
		// 如果程序出出现了panic错误,可以通过recover恢复过来
		if err != nil {
			fmt.Println("recover in B")
		}
	}()
	panic("panic in B")
}

func funcC() {
	fmt.Println("func C")
}

func main() {
	funcA()
	funcB()
	funcC()
}
```

### (2). method

`method`是附属在一个给定的类型上的，他的语法和函数的声明语法几乎一样，只是在`func`后面增加了一个receiver(也就是method所依从的主体)

定义结构体的方法一般在receiver处用指针形式（否则的话默认值传递无法修改原值 + 反正支持p.name方式取值也不麻烦）

```go
type Rectangle struct {
	width, height float64
}

type Circle struct {
	radius float64
}

func (r Rectangle) area() float64 {					// Rectangle的method
	return r.width*r.height
}

func (this *Circle) area() float64 {				// *Circle的method, 如果method的接收者不一样，那么method就不一样
	return this.radius * this.radius * math.Pi
}

func main() {
	r1 := Rectangle{12, 2}
	c1 := &Circle{10}
	fmt.Println("Area of r1 is: ", r1.area())		// 通过.调用method
    fmt.Println("Area of c1 is: ", c1.area())		// 一般用(*c1).area(), 但也支持c1.area()的方式调用method
}
```

### (3). 接口

* interface是一组method的集合，如果某个类型实现了某个接口类型的所有方法，则称此类型实现了此接口

  接口类型的变量能存储：实现了该接口的类型的值

```go
type cat struct{ num int }
type dog struct{}

type Adder interface { 							// 定义一个Adder接口类型
	add(a int)
}
type Sayer interface { 							// 定义一个Sayer接口类型
	say()
}

func (c cat) say() { 							// cat实现了Sayer接口
	fmt.Println("喵喵喵")
}

func (d dog) say() { 							// dog实现了Sayer接口
	fmt.Println("汪汪汪")
}
func (c *cat) add(a int) { 						// *cat(cat结构体类型的指针)实现了Adder接口
	c.num = c.num + a
}

func main() {
	var x Adder 								// 定义一个Adder接口类型的变量x
    var y Sayer 								// 定义一个Sayer接口类型的变量y
	a := cat{1} 								// 定义并初始化一个cat结构体类型的对象a
	b := dog{}  								// 定义并初始化一个dog结构体类型的对象b
	x = &a      								// 可以把cat结构体类型对象的指针直接赋值给x
	fmt.Println(x, a)							// &{1} {1}
	a.add(1)
	fmt.Println(x, a)							// &{2} {2}
	y = a       								// 可以把cat结构体类型的对象直接赋值给y
	y.say()     								// 喵喵喵
	y = b       								// 可以把dog结构体类型的对象直接赋值给y
	y.say()     								// 汪汪汪
}

```
```go
// 查看文档可知：任何实现了"String() string"方法的类型就实现了fmt.Stringer接口, 从而使此类型可以作为参数被fmt.Println调用
type Human struct {
	name  string
	age   int
	phone string
}

// 通过这个方法Human实现了fmt.Stringer接口
func (h Human) String() string {
	return "❰" + h.name + " - " + strconv.Itoa(h.age) + " years -  ✆ " + h.phone + "❱"
}

func main() {
	Bob := Human{"Bob", 39, "000-7777-XXX"}
	fmt.Println(Bob)  // ❰Bob - 39 years -  ✆ 000-7777-XXX❱
}
```
* 空接口：`interface{}`，包含0个method的interface，可以被任何类型实现，故可存储任何类型的值

  因此，若一个函数把interface{}作为参数，那么他可以接受任意类型的值作为参数，若一个函数返回interface{},那么也就可以返回任意类型的值

```go
func myfunc(a interface{}) {
	fmt.Println("myfunc is running")
}

func main() {
	var i int = 5
	s := "Hello world"
    var a interface{} 								// 定义一个空接口类型的变量a
	a = i
	fmt.Println(a)
	a = s
	fmt.Println(a)
	myfunc(i)
	myfunc(s)
}
```

* 支持嵌套接口

```go
// 例子一: container/heap包
type Interface interface {
		sort.Interface      	// 嵌入字段sort.Interface, 包含了sort.Interface的所有method
		Push(x interface{}) 	// a Push method to push elements into the heap
		Pop() interface{}   	// a Pop elements that pops elements from the heap
	}
// 例子二: io包
type ReadWriter interface {
		Reader					// 嵌入字段Reader, 包含了Reader的所有method
		Writer					// 嵌入字段Writer, 包含了Writer的所有method
	}
```

## 4. 反射

* 变量内置pair：type, value
* 类型断言：interface的变量里面可以存储任意类型的值(该类型实现了interface)，类型断言可以判断这个变量里面实际保存的是哪个类型的值

```go
type Element interface{}

type Person struct {
	name string
	age  int
}

// 打印
func (p Person) String() string {
	return "(name: " + p.name + " - age: " + strconv.Itoa(p.age) + " years)"
}

func main() {
	list := make([]Element, 3)     			// 做一个用来存放Element接口类型数据的，长度为三的切片
	list[0] = 1                    			// 第一个存: int类型值
	list[1] = "Hello"              			// 第二个存: string类型值
	list[2] = Person{"Dennis", 70} 			// 第三个存: Person类型对象

	for i, v := range list { 				// 方法一
		switch value := v.(type) {
		case int:
			fmt.Printf("list[%d] is an int and its value is %d\n", i, value)
		case string:
			fmt.Printf("list[%d] is a string and its value is %s\n", i, value)
		case Person:
			fmt.Printf("list[%d] is a Person and its value is %s\n", i, value)
		default:
			fmt.Printf("list[%d] is of a different type", i)
		}
	}
	for i, v := range list {				// 方法二
		if value, ok := v.(int); ok {
			fmt.Printf("list[%d] is an int and its value is %d\n", i, value)
		} else if value, ok := v.(string); ok {
			fmt.Printf("list[%d] is a string and its value is %s\n", i, value)
		} else if value, ok := v.(Person); ok {
			fmt.Printf("list[%d] is a Person and its value is %s\n", i, value)
		} else {
			fmt.Printf("list[%d] is of a different type", i)
		}
	}
}
```

* reflect包

```go
type User struct {
	Name string `info:"name" doc:"我的名字"`
	Age  int    `info:"age" doc:"我的年龄"`
}

func (this User) Call() {
	fmt.Println("user is called ..")
}

func main() {
	user1 := User{"Aceld", 18} 											// 创建User类对象user1, 准备进行解析
	getFieldAndMethod(user1)
	getTag(&user1)
}

func getFieldAndMethod(input interface{}) {
	// 解析input的type
	inputType := reflect.TypeOf(input)
	fmt.Println("inputType is :", inputType.Name()) 					// inputType is : User

	// 解析input的value
	inputValue := reflect.ValueOf(input)
	fmt.Println("inputValue is:", inputValue) 							// inputValue is: {Aceld 18}

	// 解析input的所有字段
	for i := 0; i < inputType.NumField(); i++ {
		fmt.Printf("%s: %v = %v. ", inputType.Field(i).Name, 			// Name: string = Aceld. Age: int = 18. 
			inputType.Field(i).Type, inputValue.Field(i).Interface())
	}

	// 解析input的所有method
	for i := 0; i < inputType.NumMethod(); i++ {
		m := inputType.Method(i)
		fmt.Printf("\n%s: %v\n", m.Name, m.Type) 						// Call: func(main.User)
	}
}

func getTag(input interface{}) {
	// 解析input的所有tag
	inputElem := reflect.TypeOf(input).Elem()
	for i := 0; i < inputElem.NumField(); i++ {
		taginfo := inputElem.Field(i).Tag.Get("info")
		tagdoc := inputElem.Field(i).Tag.Get("doc")
		fmt.Printf("info: %v, doc: %v. ", taginfo, tagdoc)				// info: name, doc: 我的名字. info: age, doc: 我的年龄. 
	}
}
```

## 5. 包

* 可见性：若想在另一个包中引用一个包里的标识符（如变量、常量、类型、函数等）时，该标识符必须是对外可见的，即标识符的首字母大写

```go
func Add(x, y int) int {	// 对外可见
	return x + y
}

func age() { 				// 对外不可见
	var Age = 18 			// 对外不可见
	fmt.Println(Age)
}

type Student struct {
	Name  string 			// 对外可见
	class string 			// 对外不可见
}

type Payer interface {
	init() 					// 对外不可见
	Pay()  					// 对外可见
}
```

* 导入：`import`

```go
// 1. 调用这个包的函数时，可以省略前缀的包名：Println("hello world")
 import(
     . "fmt"
 )
// 2. 给这个包起别名：f.Println("hello world")
import(
     f "fmt"
 )
// 3. 引入该包，而不直接使用包里面的函数，而是调用了该包里面的init函数
import (
	    "database/sql"
	    _ "github.com/ziutek/mymysql/godrv"
	)
```

* 初始化函数：`init()`，执行导入包语句时自动调用，不能在代码中主动调用

  执行时机：全局声明之后，`main()`之前

![image-20210221232023592](TyporaPics/image-20210221232023592.png)

## 6. 文件

### (1). 读文件

* 利用bufio

```go
func main() {
    // 打开关闭文件
	file, err := os.Open("./hello.go")
	if err != nil {
		fmt.Println("open file failed, err:", err)
		return
	}
	defer file.Close()
    
    // bufio按行读取
	reader := bufio.NewReader(file)
	for {
		line, err := reader.ReadString('\n')
		if err == io.EOF {
			if len(line) != 0 {
				fmt.Println(line)
			}
			fmt.Println("文件读完了")
			break
		}
		if err != nil {
			fmt.Println("read file failed, err:", err)
			return
		}
		fmt.Print(line)
	}
}
```

* 利用ioutil

```go
func main() {
	content, err := ioutil.ReadFile("./hello.go")
	if err != nil {
		fmt.Println("read file failed, err:", err)
		return
	}
	fmt.Println(string(content))
}
```

### (2). 写文件

* 利用bufio

```go
func main() {
	file, err := os.OpenFile("xx.txt", os.O_CREATE|os.O_TRUNC|os.O_WRONLY, 0666)
	if err != nil {
		fmt.Println("open file failed, err:", err)
		return
	}
	defer file.Close()
	writer := bufio.NewWriter(file)
	writer.WriteString("Golang") 		// 将数据先写入缓存
	writer.Flush()               		// 将缓存中的内容写入文件
}
```

* 利用ioutil

```go
func main() {
	str := "Golang"
	err := ioutil.WriteFile("./xx.txt", []byte(str), 0666)
	if err != nil {
		fmt.Println("write file failed, err:", err)
		return
	}
}
```

## 7. 并发编程

### (1). [GPM模型](https://www.jianshu.com/p/fa696563c38a)

![image-20210404230248931](TyporaPics/image-20210404230248931.png)

![image-20210404230432641](TyporaPics/image-20210404230432641.png)

### (2). goroutine

goroutine：用户级线程，是通过Go的runtime管理的，通过`go`关键字调用函数即可开启一个新的goroutine

```go
func say(s string) {
	for i := 0; i < 5; i++ {
		fmt.Println(s)
	}
}

func main() {
	go say("world") 					// 开一个新的Goroutine执行，建立新Goroutine需要时间
	say("hello")    					// 当前Goroutine执行
	time.Sleep(time.Second)				// 让main中的Goroutine等待其余的执行不然只会输出5个hello就结束了
}
```

一般不用Sleep(), 而是使用`sync.WaitGroup`来实现goroutine的同步

```go
var wg sync.WaitGroup

func hello(i int) {
	defer wg.Done() 					// goroutine结束就done -1
	fmt.Printf("Goroutine%d!\t", i)
}

func main() {

	for i := 0; i < 4; i++ {
		wg.Add(1) 						// 启动一个goroutine就add +1
        go hello(i)						// Goroutine3! Goroutine1! Goroutine2! Goroutine0! (Goroutine调度随机)
	}
	wg.Wait() 							// 等待所有的goroutine都结束
}
```

`runtime.GOMAXPROCS(n) `：设置同时运行逻辑代码的系统线程的最大数量n，并返回之前的设置。如果n < 1，不会改变当前设置，默认跑满CPU

`runtime.Goexit()`：退出当前goroutine

### (3). channel（引用类型）

* Go语言的并发模型是`CSP（Communicating Sequential Processes）`，提倡**通过通信共享内存**
* goroutine运行在相同的地址空间，因此访问共享内存必须做好同步。Go提供了一个很好的通信机制channel
* channel可以与Unix shell 中的双向管道做类比，可以通过它发送或者接收值，这些值只能是特定的类型

* 定义：
  * `var name chan type`（自动置为nil）
  * `var name = make(chan type, cap)`（自动初始化）


* 无缓冲的通道

```go
func recv(ch chan int) {
	ret := <-ch						// 从ch中接收数据，并赋值给ret
	fmt.Println("接收成功", ret)
}

func main() {
	ch := make(chan int)			// 创建可发送接收int型数据的无缓冲通道ch
	go recv(ch) 					// 启用新goroutine从通道接收值，不然无缓冲的通道会在下面阻塞后形成deadlock
	ch <- 10						// 发送数据10到ch中
	fmt.Println("发送成功")
}
```

* 有缓冲的通道，len()：当前通道里值数量，cap()：通道容量

```go
func main() {
	c := make(chan int, 3) 									// 创建一个容量为3的有缓冲区通道

	fmt.Println("len(c) = ", len(c), ", cap(c)", cap(c))

	go func() {												// 启用新goroutine发送值
		defer fmt.Println("子go程结束")

		for i := 0; i < 4; i++ {
			c <- i											// 前三个不会阻塞，第四个会阻塞，直到有数据被接收
			fmt.Println("子go程正在运行, 发送的元素=", i,
				" len(c)=", len(c), ", cap(c)=", cap(c))
		}
	}()

	time.Sleep(2 * time.Second)

	for i := 0; i < 4; i++ {
		num := <-c 											// 从c中接收数据，并赋值给num
		fmt.Println("num = ", num)
	}

	fmt.Println("main 结束")
}
```

* 支持for range遍历channel（需搭配close使用）

```go
func fibonacci(n int, c chan int) {
	x, y := 1, 1
	for i := 0; i < n; i++ {
		c <- x
		x, y = y, x+y
	}
	close(c)								// 显式关闭channel，之后无法再发送任何数据，但可以读取
}

func main() {
	c := make(chan int, 10)
	go fibonacci(20, c)
	for i := range c {						// 能够不断地读取channel里面的数据，直到该channel被显式地关闭
		fmt.Printf("%d ", i)				// 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597 2584 4181 6765 
	}
}
```

* 支持select多路复用，每个case对应一个通道的通信（类似switch）
* select默认是阻塞的，只有当监听的channel中有发送或接收可以进行时才会运行，当多个channel都准备好的时候，select随机的选择一个执行

```go
// 输出：0 2 4 6 8 
func main() {
	ch := make(chan int, 1)
	for i := 0; i < 10; i++ {
		select {
		case x := <-ch:						// 当ch中读出一个值：输出该值
			fmt.Printf("%d ", x)
		case ch <- i:						// 当ch中写入一个值：nothing
        default:							// 当监听的channel都没有准备好的时候，默认执行的(有default时select不再阻塞等待channel)
			ch <- 10
		}
	}
}
```

```go
func fibonacci(c, quit chan int) {
	x, y := 1, 1
	for {
		select {
		case c <- x:						// c可写，继续迭代
			x, y = y, x+y
		case <-quit:						// quit可读，运行结束
			fmt.Println("quit")
			return
		}
	}
}

func main() {
	c := make(chan int)
	quit := make(chan int)
    
	go func() {
		for i := 0; i < 10; i++ {
			fmt.Println(<-c)
		}

		quit <- 0							// 输出完毕，向quit写入元素
	}()
    
	fibonacci(c, quit)
}
```

## 8. 网络编程

# 二、Gin

## 1. 引入

创建schedule项目

```cmd
mkdir schedule
cd schedule
go mod init schedule
```

创建schedule.go

```go
package main

import (
	"github.com/gin-gonic/gin"
)

func sayHello(c *gin.Context){
	c.JSON(200, gin.H{
		"message": "Hello, Golang!",
	})
}

func main() {
	// 创建一个默认的路由引擎
	r := gin.Default()
	// 指定用户使用GET请求访问/hello时，执行sayHello这个函数
	r.GET("/hello", sayHello)
    // RESTful API 示例
    r.GET("/book", func(c *gin.Context) {
		c.JSON(200, gin.H{
			"message": "GET, 查询",
		})
	})

	r.POST("/book", func(c *gin.Context) {
		c.JSON(200, gin.H{
			"message": "POST, 创建",
		})
	})

	r.PUT("/book", func(c *gin.Context) {
		c.JSON(200, gin.H{
			"message": "PUT, 更新",
		})
	})

	r.DELETE("/book", func(c *gin.Context) {
		c.JSON(200, gin.H{
			"message": "DELETE, 删除",
		})
	})
	// 启动服务
	r.Run()
}
```

```cmd
go get -u github.com/gin-gonic/gin
go mod tidy
```