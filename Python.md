# Python语法

## turtle库

1. turtle绘图窗体布局

   设置窗体大小及位置：`turtle.setup(width,height,startx,starty)`

   （若不指定窗体位置，默认窗体在屏幕中央）

![image-20191129151959165](TyporaPics/image-20191129151959165.png)

2. turtle空间坐标体系

   * 绝对坐标

     ![image-20191129181022595](TyporaPics/image-20191129181022595.png)

   * 海龟坐标

     向海龟正前方运行：`turtle.fd(d)`

     向海龟反方向运行：`turtle.bk(d)`

     以海归当前位置左侧的某一个点为圆心进行曲线运行：`turtle.circle(r,angle)`

     ![image-20191129191837909](TyporaPics/image-20191129191837909.png)

3. turtle角度坐标体系

   * 绝对角度

     改变海龟行进方向：`turtle.seth(angle)`（若不指定angle，默认画整圆）

     ![image-20191129193107092](TyporaPics/image-20191129193107092.png)

   * 海龟角度

     海龟向左改变运行方向：`turtle.left(angle)`

     海龟向右改变运行方向：`turtle.right(angle)`

4. RGB色彩体系

   turtle库默认使用小数值，可切换为整数值：`turtle.colormode(mode)`

   mode=1.0为小数值模式，mode=255为整数值模式

   ![image-20191129193652968](TyporaPics/image-20191129193652968.png)

   ![image-20191129193745827](TyporaPics/image-20191129193745827.png)

5. 画笔控制

   抬起画笔：`turtle.pu()`

   放下画笔：`turtle.pd()`

   设置画笔宽度：`turtle.width(width)`

   设置画笔颜色：`turtle.pencolor(colorstring||r,g,b||(r,g,b))`

## 基础语法

1. 循环语句：`for i in range(5):`

   `range(N)`：产生0到N-1整数序列，共N个

   `range(M,N)`：产生M到N-1整数序列，共N-M个

2. 整数类型：没取值范围限制

   二进制：0b010，-0B101

   八进制：0o123,-0O456

   十进制：1010,-217

   十六进制：0x9a,-0X89

3. 浮点数类型：存在不确定尾数，不是bug，因为二进制表示小数可以无限接近但不完全相同

   支持科学计数法：`4.3e-3`为4.3*10的-3次方

   浮点数间比较一般采用：`round(x,d)`，对x四舍五入，d是小数截取位数

   `round(0.1+0.2,1)==0.3`为true

4. 复数类型：获取实部`z.real`，获取虚部`z.imag`

5. 运算操作符：支持二元操作符`x op = y`

   ![image-20191130120726081](TyporaPics/image-20191130120726081.png)

   ![image-20191130120833098](TyporaPics/image-20191130120833098.png)

6. 运算函数：

   ![image-20191204151952237](TyporaPics/image-20191204151952237.png)

   最大值函数：`max(x1,x2,...,xn)`

   最小值函数：`min(x1,x2,...,xn)`

   四舍五入函数：`round(x[,d])`，d默认为0

   类型转换函数：`int(x)` `float(x)` `complex()`

7. 字符串：

   字符串切片：`"0123"[m:n:k]`，其中m缺失补0，n缺失补字符个数，k为步长，当k为-1指从后向前逐一取出

   字符串连接：`x+y`

   复制n次字符串x：`x*n`

   x是否为s字串：`x in s`

8. 字符串处理函数：

   返回字符串长度：`len(x)`

   类型转换函数：`str(x)`

   将十进制整数转成十六进制形式字符串：`hex(425)`

   将十进制整数转成八进制形式字符串：`oct(425)`

   返回Unicode编码对应的字符：`chr(u)`

   返回字符对应的Unicode编码：`ord(x)`

9. 字符串处理方法：

   返回字符串的副本，全部字符大写：`str.upper()"`

   返回字符串的副本，全部字符小写：`str.lower()"`

   返回一个列表，由sep分割：`str.split(sep=None)`

   返回子串sub出现次数：`str.count("a")`

   old子串替换为new：`str.replace(old,new)`

   字符串居中：`str.center(width[,fillchar])`

   去掉开头和结尾指定字符们：`str.strip(chars)`，lstrip()和rstrip()分别去掉开头与结尾

   iter变量除最后元素外每个元素后增加个str：`str.join(iter)`

10. 槽的格式控制：

    ![image-20191205160634404](TyporaPics/image-20191205160634404.png)

## time库

1. 常用函数

   时间获取：`time()` ,`ctime()` ,`gmtime()`

   时间格式化：`strftime(tpl,ts)`，其中tpl是格式化模板字符串，定义输出效果，ts是时间类型变量

   时间反格式化：`strptime(str,tpl)`，其中str是字符串形式时间值，tpl是格式化模板字符串，定义输入效果

   程序计时：`perf_counter()`，返回一个CPU级别精确时间计数值，一般连续调用求差值

   程序休眠：`sleep(s)` ，s可以是浮点数

2. 