### ch00.C++介绍目录
1. 什么是 C++
2. C++ 的开发环境与相关工具
3. C++ 的编译 / 链接模型

```c
# 预处理
g++ -std=c++11 -E -o problem1.i problem1.cpp
# 编译
g++ -std=c++11 -S -o problem1.s problem1.i
# 汇编
g++ -c problem1.s
# 链接
g++ problem1.o
# 执行
./a.out
----
预处理 　　.i .ii文件　　　　     gcc -E hello.cpp -o hello.i
编译 　　　.s文件　　　　         gcc -S hello.cpp -o hello.s
汇编 　　　.o .obj文件　　　　 　　gcc -c hello.cpp -o hello.o
链接 　　　.lib .a文件
---
mangling过程:nm a.out
解mangling过程:nm a.out | c++filt -t

```
### 一.什么是 C++
- 一门比较流行的编程语言
  -  https://www.tiobe.com/tiobe-index//
  
- C 语言的扩展
  -  关注性能
        a.与底层硬件紧密结合: https://godbolt.org/z/xPq6e9
        <div align = center>
        <img src="../pic/ch00/03.png "width="50% height="70%">
        <div align = left>

        b.对象生命周期的精确控制
        <div align = center>
        <img src="../pic/ch00/04.png "width="50% height="70%">
        <div align = left>

        c.Zero-overhead Abstraction
        <div align = center>
        <img src="../pic/ch00/05.png "width="60% height="70%">
        <div align = left>


    - 引入大量特性,便于工程实践  
        a.三种编程范式:面向过程、面向对象、泛型  
        b.函数重载、异常处理、引用  

- 一系列不断衍进的标准集合
    - C++98/03 , C++11 , C++14 , C++17 , C++20 , C++23 ?
    - 语言本身的改进
        a.Memory Model
        b.Lambda Expression
    - 标准库的改进
        a.type_traits / ranges
        b.auto_ptr

- C++ 标准的工业界实现
    - MSVC / GCC / Clang...
    - 每个编译器可能并不完全遵照标准
        https://godbolt.org/z/cKMjK3
    - 不同的实现存在差异
        https://godbolt.org/z/6hnPhY


- 不能脱离具体的语境讨论 C++
    - 编写程序时要注重
        a.性能
        b.标准


### 二.C++ 的开发环境与相关工具

- Visual C++ / GCC (G++) / Clang (Clang++)...
- 集成开发环境: Visual Studio / CodeLite / Code::blocks / Eclipse...
- 工具
    - /usr/bin/time(控制程序运行时间:sleep 1)
    - valgrind(执行的检测,内存泄露)
    - cpp reference(C++标准的使用,类似于opencv网页教程)
    - Compiler explorer(网页编译器,可选择不同的编译器,缺点:只能运行一段代码)
    - C++ insights(将新语法翻译成老代码,牛逼!!)

### 三.C++ 的编译 / 链接模型
#### 3.1.简单的加工模型

<div align = center>
<img src="../pic/ch00/00.png "width="40% height="70%">
<div align = left>

####  3.2.问题:无法处理大型程序
- 加工耗时较长
- 即使少量修改,也需要全部重新加工


#### 3.3.解决方案:分块处理
<div align = center>
<img src="../pic/ch00/01.png "width="50% height="70%">
<div align = left>


#### 3.4.好处

- 编译耗资源但一次处理输入较少
- 链接输入较多但处理速度较快
- 便于程序修改升级

#### 3.5.由“分块处理”衍生出的概念 “ 分块处理 ” 衍生出的概念
  - 定义/声明:该变量为其他文件定义的变量
  - 头文件/源文件:引用其他头文件
  - 翻译单元
      - 源文件 + 相关头文件(直接 / 间接) - 应忽略的预处理语句(#if #else if)
  - 一处定义 原则:
      - a.程序级:一般函数
      - b.翻译单元级:内连函数、类、模板

<div align = center>
<img src="../pic/ch00/02.jpg "width="60% height="70%">
<div align = left>

<div align = center>
<img src="../pic/ch00/05.png "width="60% height="70%">
<div align = left>


#### 3.6.预处理
- 将源文件转换为翻译单元的过程
- 防止头文件被循环展开
    - #ifdef 解决方案:`#ifndef #def #endif`
    - #pragma once 解决方案 -->新语法
#### 3.7.编译
- 将翻译单元转换为相应的汇编语言表示
- 编译优化
    - https://godbolt.org/z/zh9aqx  汇编优化--$O_3$,3级别优化,但是优化后无法单步调试,与CMakeLists.txt
- 增量编译 V.S. 全部编译(增量编译:只重新编译有更改的部分)

#### 3.8.链接
- 合并多个目标文件,关联声明与定义 
- 连接( Linkage )种类:内部连接、外部连接、无连接
- 链接常见错误:找不到定义 (可能只有声明没有找到定义)

#### 3.9.other
- C++ 的编译 / 链接过程是复杂的,预处理、编译与链接都可能出错
- 编译可能产生警告、错误,都要重视

### 四.小结
- C++ 是一门注重性能的程序设计语言
- C++ 的标准经历类一系列的衍化,还在不断发展
- 标准与具体实现之间存在差距
- C++ 源程序转换成可执行文件是相对复杂的过程,主要包含预处理、编译、链接等阶段,每一阶段都可能引入错误