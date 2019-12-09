# 多线程编程

进程：

线程：

并发：

并行：

***

C++11创建多线程

```c++
#include<thread>
using namespace std;
int main(){
    //创建可执行对象
	void x1(){};
	auto x2=[]{};
	class x3{
	void operator()(){};

	//线程用法
	thread myThread1();
	if(myThread1.joinable()){
        myThread1.join();
		myThread1.detach();
    }
    return 0;
}
```

***

要将参数传递给线程的函数，只需要将参数传递给thread的构造函数，默认情况下所有的参数都将复制到新线程的内部存储中。即使线程函数接收引用作为参数，但不会影响main中的值，因为线程函数的引用是复制再新线程的堆栈中的临时值，若需要深copy使用std::ref，例：`thread threadObj(threadCallback,ref(x));`

***

C++11线程加锁

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

***

有时会发生函数结束忘记释放锁，所以可以使用lock_guard，这是一个模板类，位mutex实现RALL，它将mutex包裹在他的对象内。

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
    	//如果在此处发生异常，lockGuard的析构函数将会因为堆栈展开而被调用
    	mMoney++;
    	//函数退出，lockGuard对象的析构函数将被调用，在析构函数中mutex会被释放
	}
};
```

***

C++11条件变量

`#include<condition_variable>`

唤醒一个等待进程：notify_one() 

唤醒所有等待进程：notify_all() 