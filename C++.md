# C++

## 基础语法

### 指针

```c++
#include<iostream>
using namespace std
void main()
{
	int a[5]={1,2,3,4,5};				// a为数组名，当前为int*型指针常量，它的加减以4字节为单位
	int *ptr=(int *)(&a+1);				// &a是指向整个数组的指针，它的加减会以20字节为单位
	cout<<*(a+1)<<*(ptr-1);				// ptr是int*型指针，加减以4字节为单位，输出：2,5
}
```

## 并发编程

### 引入

* 进程：
* 线程：
* 并发：两个或多个事件在同一时间间隔内发生
* 并行：两个或多个事件在同一时刻发生

### 创建多线程

```c++
#include<thread>
using namespace std;
int main(){
    // 创建可执行对象
	void x1(){};	// 可以是函数
	auto x2=[]{};	// 可以是lambda表达式
	class x3{		// 可以是类对象（需运算符重载）
		void operator()(){};
    };
	// 线程用法
	thread myThread1(可执行对象);
	if(myThread1.joinable()){
        myThread1.join();
		myThread1.detach();
    }
    return 0;
}
```

### 线程参数传递

要将参数传递给线程函数，只需要将参数传递给thread的构造函数，默认情况下所有的参数都将复制到新线程的内部存储中。

若线程函数接收引用作为参数，必须加'const'，而且线程函数的引用是复制在新线程的堆栈中的临时值。

```c++
#include<iostream>
#include<thread>
using namespace std;
void add(const int &a) {
	int& x = const_cast<int&>(a);
	x = x + 1;
	cout << a << endl;			// 2
}
int main()
{
	int a = 1;
	cout << a << endl;			// 1
	thread thread1(add,a);
	thread1.join();
	cout << a ;					// 1
	return 0;
}
```

若需要传递引用，使用std::ref。

```c++
#include<iostream>
#include<thread>
using namespace std;
void add(int &a) {
	a = a + 1;
	cout << a << endl;			// 2
}
int main()
{
	int a = 1;
	cout << a << endl;			// 1
	thread thread1(add,ref(a);
	thread1.join();
	cout << a ;					// 2
	return 0;
}
```

### 线程加锁

 ```c++
#include<mutex>
class Wallet{
    int mMoney;
    mutex::mutex;
public:    
    Wallet():mMoney(0){}
    int getMoney(){return mMoney;}
    void addMoney(int money){
        mutex.lock();
        for(int i=0;i<money;++i)
           mMoney++;
        mutex.unlock();
    }
};
        
 ```

有时会发生函数结束忘记释放锁，所以可以使用lock_guard，这是一个模板类，为mutex实现RALL，它将mutex包裹在他的对象内。

```C++ 
class Wallet {   
	int mMoney;
    mutex mutex;
public:  
	Wallet():mMoney(0){}
	int getMoney(){return mMoney;}
	void addMoney(int money){
 		lock_guard<mutex> lockGuard(mutex);
 		for (int i = 0; i < mMoney; ++i)
    	// 如果在此处发生异常，lockGuard的析构函数将会因为堆栈展开而被调用
    	mMoney++;
    	// 函数退出，lockGuard对象的析构函数将被调用，在析构函数中mutex会被释放
	}
};
```

### 条件变量

`#include<condition_variable>`

唤醒一个等待进程：notify_one() 

唤醒所有等待进程：notify_all() 

## 网络编程

### 概述

* Socket套接字类型：流式套接字(TCP)、数据报套接字(UDP)、原始套接字...
* TCP与UDP区别：
  * TCP面向连接且稳定，UDP面向无连接且不稳定
  * TCP对系统要求资源多，UDP对系统资源要求少
  * UDP程序结构更简单响应速度更高
  * TCP采用流模式保证数据正确性与顺序，UDP采用数据报模式可能丢包不保证顺序