---
title: 编译 C++ 文件基本命令
nav_order: 2
---

### ch00.linux下编译C++文件基本命令
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
- **ESCO**

一个C/C++源代码要变成一个可执行文件，需要经过预处理(Pre-processing)－编译(Compiling)－汇编(Assembling)－链接(Link)

- 基本流程为：
test.c[源文件] --预处理--test.i[预处理后代码] --编译-- test.s[汇编代码] --汇编-- test.o[目标代码文件] --链接-- test.exe[可执行文件]

##### 1.预处理
-E 选项使用g++/gcc将源代码预处理后不执行其他动作。 下面的命令将test.cpp预处理，并在标准输出中显示：`g++ -E test.cpp `
后面加上 -o 选项表示将源代码预处理后输出在指定文件中，比如test.i：`g++ -E test.cpp -o test.i`
##### 2.编译
-S 选项使用g++/gcc将预处理后的文件编译，翻译成汇编代码。只编译不汇编. 下面的命令将会编译test.i文件，并自动在当前文件夹生成test.s文件:`g++ -S test.i`
若要指定其他输出名，则需 -o 指定，比如生成名为xxx.s的汇编代码文件:`g++ -S test.i -o xxx.s`
##### 3.汇编
-c 选项将编译生成的test.s文件生成二进制目标代码. 下面的命令将在当前文件夹自动生成test.o的二进制目标代码文件:`g++ -c test.s `  
如果要指定输出文件名，则需 -o 指定，比如生成xxx.o的二进制目标代码文件:`g++ -c test.s -o xxx.o`
##### 4.链接
链接阶段是将相关的目标文件链接起来，形成一个整体，生成可执行文件无选项链接;下面的命令会把二进制目标文件test.o所需的相关文件链接成一个整体，并在当前文件夹自动生成一个名为a.out的可执行文件:`g++ test.o`
如果要执行这个可执行文件，需要输入命令:`./a.out`
当然也可以指定生成的可执行文件的文件名:`g++ test.o -o test.exe`

##### 5.单个源文件直接生成可执行文件
当然g++/gcc也可以直接把源代码直接生成可执行文件. 下面的命令将test.cpp直接在当前文件夹生成a.out可执行文件，若要指定文件名，可使用 -o 选项
```bash
g++ test.cpp
g++ test.cpp -o test.exe
```
#####  6.多个源文件直接生成可执行文件
也可以将多个源代码编译链接成一个可执行文件. 下面的命令将test.cpp直接在当前文件夹生成a.out可执行文件，若要指定文件名，可使用 -o 选项
```bash
g++ test1.cpp test2.cpp 
g++ test1.cpp test2.cpp -o test.exe
```
- 使用C++11标准编译
如果要使用C++11版本特性，则需要使用 `-std=c++11` 选项

```bash
g++ -std=c++11 test.cpp -o test.exe
```