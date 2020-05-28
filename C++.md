# C++

## 基础语法

### 指针

```c++
#include<iostream>
using namespace std
void main()
{
	int a[5]={1,2,3,4,5};				//a为数组名，当前为int*型指针常量，它的加减以4字节为单位
	int *ptr=(int *)(&a+1);				//&a是指向整个数组的指针，它的加减会以20字节为单位
	cout<<*(a+1)<<*(ptr-1);				//ptr是int*型指针，加减以4字节为单位，输出：2,5
}
```

