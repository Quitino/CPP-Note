---
title: 语句
nav_order: 7
---

### ch05.语句
1. 语句基础
2. 分支语句
3. 循环语句
4. 语句的综合应用——达夫设备

### 一.语句基础

#### 1.1.语句的常见类别
- 表达式语句:表达式后加分号,对表达式求值后丢弃,可能产生副作用
- 空语句:仅包含一个分号的语句,可能与循环一起工作
- 复合语句(语句体):由大括号组成,无需在结尾加分号,形成独立的域(语句域)
  ```c
  int main(){
    int x = 2;
    {
      int x = 3;
      x = x - 1;
      std::cout<< x <<'\n';
    }//内部的x到这就消亡了
  }
  ```
#### 1.2.顺序语句与非顺序语句

- 顺序语句
  - 从语义上按照先后顺序执行
  - 实际的执行顺序可能产生变化(编译器优化、硬件乱序执行)
  - 与硬件流水线紧密结合,执行效率较高
- 非顺序语句
  - 在执行过程中引入跳转,从而产生复杂的变化
  - 分支预测错误可能导致执行性能降低语句基础(续)

#### 1.3.最基本的非顺序语句: goto
- 通过标签指定跳转到的位置
- 具有若干限制
  - 不能跨函数跳转
  - 向前跳转时不能越过对象初始化语句
  - 向后跳转可能会导致对象销毁与重新初始化
  ```c
  intMain(){
    int x = 3;
    if(x) goto lable;
    x = x + 1;
  lable:
    return 0;
  }
  //不安全,逻辑混乱
  ---
- goto 本质上对应了汇编语言中的跳转指令
  - 缺乏结构性的含义
  - 容易造成逻辑混乱
  - 除特殊情况外,应避免使用


### 二.分支语句
#### 2.1.分支语句 — if
- [语法](https://zh.cppreference.com/w/cpp/language/if)
- 使用语句块表示复杂的分支逻辑
- 从 if 到 if-else
  - 实现多重分支
  - else 会与最近的 if 匹配
  - 使用大括号改变匹配规则
- if V.S. constexpr if—— 运行期与编译期分支
  ```c
  constexpr int grade = 80;//编译期确定的常量
  if constexpr(grade > 60){//编译期可以确定的常量表达式,优化提速
  
  }
  ```
- 带初始化语句的 if: >= C++17
  ```c
  int main(){
    int x = 3;
    if(int y = x * 3; y > 100){
  
    }else{
  
    }
  
  }
  ```
#### 2.2.分支语句 — switch
- [语法](https://zh.cppreference.com/w/cpp/language/switch)
- 条件部分应当能够隐式转换为整形或枚举类型,可以包含初始化的语句
- case/default 标签
  - case 后面跟常量表达式 , 用于匹配 switch 中的条件,匹配时执行后续的代码
  - 可以使用 break 跳出当前的 switch 执行
  - default 用于定义缺省情况下的逻辑
  - 在 case/default 中定义对象要加大括号
  ```c
  //常量表达式:编译期可以确定值的表达式
  int main(){
    int x = 3;
    switch(cin>>x; x + 1){
      case 2 + 1:{
        //在 case/default 中定义对象要加大括号
        int y = 3;
        cout<< "Hello" <<endl; 
        break;
      } 
      case 4: 
        cout<< "world" <<endl; 
        break;
      default: 
        break;
    }
  }
  ```
- \[[fallthrough]] 属性:不加break,往下执行的行为
- 与 if 相比的优劣
  - 分支描述能力较弱
  - 在一些情况下能引入更好的优化

### 三.循环语句
#### 3.1.循环语句 — while
- [语法](https://zh.cppreference.com/w/cpp/language/while)
- 处理逻辑:

  - 1. 判断条件是否满足,如果不满足则跳出循环
  - 2. 如果条件满足则执行循环体
  - 3. 执行完循环体后转向步骤 1
- 注意:在 while 的条件部分不包含额外的初始化内容
#### 3.2.循环语句 — do-while
- [语法](https://zh.cppreference.com/w/cpp/language/do)
  - **注意结尾处要有分号,表示一条语句的结束**
- 处理逻辑:
  - 1. 执行循环体
  - 2. 断条件是否满足,如果不满足则跳出循环
  - 3. 如果条件满足则转向步骤 1
#### 3.3.循环语句 — for
- [语法](https://zh.cppreference.com/w/cpp/language/for)
- 处理逻辑
  - 1. 初始化语句会被首先执行
  - 2. 条件部分会被执行,执行结果如果为 false ,则终止循环
  - 3. 否则执行循环体
  - 4. 迭代表达式会被求值,之后转向 2
- 在初始化语句中声明多个名字
  ```c
  #include <iostream>
  #include <vector>
  
  int main()
  {
      // 典型的以单语句作为循环体的循环
      for (int i = 0; i < 10; ++i)
          std::cout << i << ' ';
      std::cout << '\n';
  
      // 初始化语句可声明多个名字，
      // 只要它们能用拥有相同的声明说明符序列
      // for(int i = 0,double p = 0.5; i<9; i+=2){//非法int,double不同类型
      // int x = 3, int y = 5;//合法;
      // int x = 3; float y = 5.0;//非法,同样的道理
      for (int i = 0, *p = &i; i < 9; i += 2) {
          std::cout << i << ':' << *p << ' ';
      }
      std::cout << '\n';
  
      // （循环）条件可为声明
      char cstr[] = "Hello";
      for (int n = 0; char c = cstr[n]; ++n)
          std::cout << c;
      std::cout << '\n';
  
      // 初始化语句可使用 auto 类型说明符
      std::vector<int> v = {3, 1, 4, 1, 5, 9};
      for (auto iter = v.begin(); iter != v.end(); ++iter) {
          std::cout << *iter << ' ';
      }
      std::cout << '\n';
  
    // 初始化语句可为表达式
      int n = 0;
      for (std::cout << "循环开始\n";
          std::cout << "循环测试\n";
          std::cout << "迭代 " << ++n << '\n')
          if(n > 1)
              break;
      std::cout << '\n';
  }
  ```
- 初始化语句、条件、迭代表达式可以为空
  ```c
  for( ; ; ){//死循环
    ;//搞死
  
  }
- for 的更多示例
#### 3.4.循环语句—基于范围的 for 循环
- [语法](https://zh.cppreference.com/w/cpp/language/)range-for
- 本质:语法糖,编译器会转换为 for 循环的调用方式
- 转换形式的衍化: C++11 / C++17 / C++20
- **使用常量左值引用读元素;**
- 使用 “万能引用(universal reference)”修改元素:auto &&
  ```c
  std::vector<int> arr{1,2,3,4,5};
  for(int v : arr)
    cout<< v <<'\n';
  ---
  /*使用常量左值引用读元素*/
  std::vector<string> arr {'h', 'c', 'l'};
  for(const std::string& v: arr)//常引用: 避免数组退化为指针的问题
    std::cout<< v <<std::endl;
  ---
  //使用 “万能引用(universal reference)”修改元素:auto &&
  std::vector<int> arr{1,2,3,4,5};
  for(auto&& v : arr)
    cout<< v <<'\n';
  ```
#### 3.5.循环语句 — break / continue
- 含义(转自 cpp reference )
  - break: 导致外围的 for 、范围 for 、 while 或 do-while 循环或 switch 语句终止
  - continue: 用于跳过整个 for 、 while 或 do-while 循环体的剩余部分。
- 注意这二者均不能用于多重嵌套循环,多重嵌套循环的跳转可考虑 goto 语句

### 四.语句的综合应用——达夫设备

- 使用循环展开提升系统性能
- 处理无法整除的情形
  - 额外增加一个循环语句
  - 将 switch 与循环结合 —— 达夫设备

  ```c
  #include<iostream>
  #include<vector>
  
  int main(){
    constexpr size_t buffer_count = 10001;
    std::vector<size_t> buffer(buffer_count);
    for(size_t i = 0; i < buffer_count; ++i>){
      buffer[i] = i;
    }
    //求最大值
    size_t max_value = buffer[0];
    for(size_t i = 0; i < buffer_count; ++i){
      max_value = (max_value > buffer[i]) ? max_value : buffer[i];
    }
    ctd::cout<< max_value << std::endl;
  
    //改进,减少循环判断,提速
    //问题,如果循环不是8的倍数,会越界
    for(size_t i = 0; i < buffer_count; i += 8){
      max_value = (max_value > buffer[i]) ? max_value : buffer[i];
      max_value = (max_value > buffer[i + 1]) ? max_value : buffer[i + 1];
      max_value = (max_value > buffer[i + 2]) ? max_value : buffer[i + 2];
      max_value = (max_value > buffer[i + 3]) ? max_value : buffer[i + 3];
      max_value = (max_value > buffer[i + 4]) ? max_value : buffer[i + 4];
      max_value = (max_value > buffer[i + 5]) ? max_value : buffer[i + 5];
      max_value = (max_value > buffer[i + 6]) ? max_value : buffer[i + 6];
      max_value = (max_value > buffer[i + 7]) ? max_value : buffer[i + 7];
    }
    ctd::cout<< max_value << std::endl;
    //改进,减少循环判断,提速
    //能整除的
    for(size_t i = 0; i + 8 < buffer_count; i += 8){
      max_value = (max_value > buffer[i]) ? max_value : buffer[i];
      max_value = (max_value > buffer[i + 1]) ? max_value : buffer[i + 1];
      max_value = (max_value > buffer[i + 2]) ? max_value : buffer[i + 2];
      max_value = (max_value > buffer[i + 3]) ? max_value : buffer[i + 3];
      max_value = (max_value > buffer[i + 4]) ? max_value : buffer[i + 4];
      max_value = (max_value > buffer[i + 5]) ? max_value : buffer[i + 5];
      max_value = (max_value > buffer[i + 6]) ? max_value : buffer[i + 6];
      max_value = (max_value > buffer[i + 7]) ? max_value : buffer[i + 7];
    }
    //不能整除的
    for(size_t i = buffer_count/8 * 8; i < buffer_count; ++i){
      max_value = (max_value > buffer[i]) ? max_value : buffer[i];
    }
    ctd::cout<< max_value << std::endl;
  
    //改进
    auto ptr = buffer.begin();
    for(size_t i = 0; i + 8 < buffer_count; i += 8){
      max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;
      max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;
      max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;
      max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;
      max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;
      max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;
      max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;
      max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;
    }
  
    switch(buffer_count%8){//希望他fallthrough
      case 7:max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;[[fallthrough]];
      case 6:max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;[[fallthrough]];
      case 5:max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;[[fallthrough]];
      case 4:max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;[[fallthrough]];
      case 3:max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;[[fallthrough]];
      case 2:max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;[[fallthrough]];
      case 1:max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;
    }
  
    //改进
    switch(buffer_count%8){//希望他fallthrough
      case 0:max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;[[fallthrough]];
      case 7:max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;[[fallthrough]];
      case 6:max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;[[fallthrough]];
      case 5:max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;[[fallthrough]];
      case 4:max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;[[fallthrough]];
      case 3:max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;[[fallthrough]];
      case 2:max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;[[fallthrough]];
      case 1:max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;
    }
  
    for(size_t i = 0; i < (buffer_count - 1)/8; ++i){
      max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;
      max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;
      max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;
      max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;
      max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;
      max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;
      max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;
      max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;
    }
  
    //改进//达夫设备
    size_t = 0;
    switch(buffer_count%8)
      for(; i < (buffer_count + 7) / 8; ++i){
        [[fallthrough]];
        case 0:max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;[[fallthrough]];
        case 7:max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;[[fallthrough]];//希望他fallthrough
        case 6:max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;[[fallthrough]];
        case 5:max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;[[fallthrough]];
        case 4:max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;[[fallthrough]];
        case 3:max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;[[fallthrough]];
        case 2:max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;[[fallthrough]];
        case 1:max_value = (max_value > *ptr) ? max_value : *ptr; ++ptr;
      }
  
  }
  ```

