---
title: Object Oriented Program Ⅰ
password: ''
date: 2024-11-22 14:35:06
tags: 
  - Learning-Notes
  - Tsinghua
categories: "Project"
---
> ***Beggars cannot be choosers.***
***A bird in the hand is worth two in the bush.***
***A man may lead a horse to the water, but he cannot make it drink.---Heywood***
```bash
ls ex1.*
./ex1.out
cat ex1.cpp
g++ -c ex1.cpp
g++ -o ex1.out ex1.o
// Mac的指令
```
### 先导知识－－源程序拆分
#### 使用头文件分离声明与定义
```cpp
// //func.h
// int ADD(int a, int b);
//func.h    //防止重复包含导致的编译错误
#ifndef FUNC_H
#define FUNC_H
int ADD(int a, int b);
#endif
//func.cpp
#include "func.h"
int ADD(int a, int b)
{	return a + b;   }
//main.cpp
#include<iostream>
#include<cstdio>
#include "fun.h"
int main(int argc, char** argv) {
	if (argc != 3) {
		std::cout << "Usage: " << argv[0] << " op1 op2" << std::endl;
		return 1;
	}
	int a, b;
	a = atoi(argv[1]);
	b = atoi(argv[2]);
	std::cout << ADD(a, b) << std::endl;
	return 0;
}
```
#### 多个源文件的编译链接
> ***Thanks the new world, thanks the best time. We donot need to do it.***
```cpp
- 使用控制台窗口的命令行
c1 -GX ex51.cpp ex52.cpp -o ex5.exe
- 使用集成开发环境IDE
添加文件到工程
- 使用MAKE工具
# Xu Mingxing @ 20070525
# C++ Course for THU2006
#
all: main.exe test.exe

main.exe: main.cpp student.cpp
    g++ -o main.exe main.cpp student.cpp

test.exe: student.cpp student_test.cpp
    g++ -o test.exe student_test.cpp student.cpp

clean: 
    del *.obj *.exe

```