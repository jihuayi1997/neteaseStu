# 动态链接库的使用

## 1.引入头文件

* 通过

  ```c++
  #include”../../MyDll/MyDll/Math.h”
  ```

* 通过右键项目-属性-C/C++-常规-附加包含目录..\\..\MyDll\Mydll，再

  ```c++
  #include”Math.h”
  ```

* 通过手动复制Math.h到项目目录，再

  ```c++
  #include”Math.h”
  ```

## 2.引入lib（dll的地址）

* 通过

  ```c++
  #pragma comment(lib,”../../MyDll/Debug/MyDll.lib”)
  ```

* 通过右键项目-属性-链接器常规-附加库目录..\\..\MyDll\Debug，再

  ```c++
  #pragma comment(lib,”MyDll.lib”)
  ```

* 通过手动复制MyDll.lib到项目目录，再

  ```c++
  #pragma comment(lib,”MyDll.lib”)
  ```

注：链接器输入-附加依赖项里输入MyDll.lib可替代`#pragma comment(lib,”MyDll.lib”)`

## 3.引入dll到运行目录

* 通过右键项目-属性-所有配置-生成事件-生成后事件-命令行

  `xcopy /y /d "..\..\MyDll\$(IntDir)MyDll.dll" "$(OutDir)"`
  
* 通过手动复制Mydll.dll到项目运行（Debug）目录