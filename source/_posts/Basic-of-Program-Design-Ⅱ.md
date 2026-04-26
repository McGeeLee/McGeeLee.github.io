---
title: Basic of Program Design Ⅱ
password: ''
date: 2024-11-11 14:34:11
tags: 
  - Learning-Notes
  - Tsinghua
categories: "Project"
---
> ***There are things not measured by grapeor flower or delicate snow.---Jorge Luis Borges***
### Tips: delete the blank line in VS.
```cpp
//   VS code快速删除文本中的空白行
//1. 使用快捷键 Ctrl + H，打开替换框。
//2. 使用快捷键 Alt + R或者点击下图红框图标, 选择使用“正则表达式”。如下图：
//3. 输入正则表达式： ^\s*$，如下图：
//4. 使用快捷键Ctrl + Alt + Enter或者点击下图红框图标，快速替换所有。
```
***the first week is about introduce***
***第二周“关于变量的讨论”示例程序***
```cpp
#include <iostream>
#include <complex>
using namespace std;
```
### Task1 　//No initialization
```cpp
void Task1()
{
int num;
cout << num << endl;
} //it is wrong when count without initialization.
```
### Task2_1 //Different data type
```cpp
void Task2_1()
{
int a = 2;
float b = 2;
cout << "a = " << a << endl;         // a = 2
cout << "a / 4 = " << a / 4 << endl; // a / 4 = 0
cout << "b / 4 = " << b / 4 << endl; // b / 4 = 0.5
}
```
### Task2_2 //Data type calculation
```cpp
void Task2_2()
{
	string str1 = "Tsinghua University, ";
	string str2 = "Department of Computer";
	cout << "str1 = " << str1 << endl;              // str1 = Tsinghua University,
	cout << "str2 = " << str2 << endl;              // str2 = Department of Computer
	cout << "str1 + str2 = " << str1 + str2 << endl;// str2 = Department of Computer
	complex <float> c1(3, 4), c2(4, 5);             // str1 + str2 = Tsinghua University, Department of Computer
	cout << "c1 = " << c1 << endl;                  // c1 = (3,4)
	cout << "c2 = " << c2 << endl;                  // c2 = (4,5)
	cout << "c1 + c2 = " << c1 + c2 << endl;        // c1 + c2 = (7,9)
	cout << "c1 * c2 = " << c1 * c2 << endl;        // c1 * c2 = (-8,31)
}
```
### Task3 　//Memory address
```cpp
void Task3()
{
	int n;
	float f;
	double d = 1.23;
	char c = '*';
	cout << "address of n: " << &n << endl;// address of n: 00000028E5FCF884
	cout << "address of f: " << &f << endl;// address of f: 00000028E5FCF8A4
	cout << "address of d: " << &d << endl;// address of d: 00000028E5FCF8C8
	cout << "address of c: " << &c << endl;// address of c: *�����������������������������������c$�_�
}
```
### Task4 　//Address type
```cpp
void Task4()
{
	int n;
	float f;
	double d = 1.23;
	char c = '*';
	int* pn = &n;
	float* pf = &f;
	double* pd = &d;
	char* pc = &c;
	cout << "address of n: " << &n << endl;// address of n: 00000028E5FCF804
	cout << "address of f: " << &f << endl;// address of f: 00000028E5FCF824
	cout << "address of d: " << &d << endl;// address of d: 00000028E5FCF848
	cout << "address of c: " << &c << endl;// address of c: *��������������������������������������(
	cout << "pn: " << pn << endl;// pn: 00000028E5FCF804
	cout << "pf: " << pf << endl;// pf: 00000028E5FCF824
	cout << "pd: " << pd << endl;// pd: 00000028E5FCF848
	cout << "pc: " << pc << endl;// pc: *��������������������������������������(
}
```
### Task5 　//Read/write memory
```cpp
void Task5()
{
	int n;
	float f;
	double d = 1.23;
	char c = '*';
	int* pn = &n;
	float* pf = &f;
	double* pd = &d;
	char* pc = &c;
	*pn = 999;
	*pf = 888;
	*pd = 777;
	*pc = 'A';
	cout << "n: " << n << ", *pn = " << *pn << endl;// n: 999, *pn = 999
	cout << "f: " << f << ", *pf = " << *pf << endl;// f: 888, *pf = 888
	cout << "d: " << d << ", *pd = " << *pd << endl;// d: 777, *pd = 777
	cout << "c: " << c << ", *pc = " << *pc << endl;// c: A, *pc = A
}
```
### Task6 　//Memory calculation
```cpp
void Task6()
{
	int n1 = 12, n2 = 87;
	char c1 = '9', c2 = 'B';
	cout << "n1: value = " << n1 << ", address = " << &n1 << endl;
	cout << "n2: value = " << n2 << ", address = " << &n2 << endl;
	cout << "c1: value = " << c1 << ", address = " << &c1 << endl;
	cout << "c2: value = " << c2 << ", address = " << &c2 << endl;
	int* pn = &n1;
	char* pc = &c1;
	cout << "pn: " << pn << endl;
	cout << "pn + 1: " << pn + 1 << endl;
	cout << "pn - 1: " << pn - 1 << endl;
	cout << "pc: " << pc << endl;
	cout << "pc + 1: " << pc + 1 << endl;
	cout << "pc - 1: " << pc - 1 << endl;
	*(pn - 1) = 367;
	cout << "n2 = " << n2 << endl;
	cin >> *(pn - 1);
	cout << "n2 = " << n2 << endl;
	*(pn - 1) = 'K';
	cout << "c2 = " << c2 << endl;
	cin >> *(pc - 1);
	cout << "c2 = " << c2 << endl;
}
// n1: value = 12, address = 00000028E5FCF834
// n2: value = 87, address = 00000028E5FCF854
// c1: value = 9, address = 9�������������������������������B�������������������������������������������������������������������������������������������������������������������c$�_�
// c2: value = B, address = B�������������������������������������������������������������������������������������������������������������������c$�_�
// pn: 00000028E5FCF834
// pn + 1: 00000028E5FCF838
// pn - 1: 00000028E5FCF830
// pc: 9�������������������������������B�����������������������������������4���(
// pc + 1: �������������������������������B�����������������������������������4���(
// pc - 1: �9�������������������������������B�����������������������������������4���(
// n2 = 87
```
```cpp
int main()
{
	//task1
	cout << "Task 1" << endl;
	Task1();
	cout << endl;
	//task2.1
	cout << "Task 2.1" << endl;
	Task2_1();
	cout << endl;
	//task2.2
	cout << "Task 2.2" << endl;
	Task2_2();
	cout << endl;
	//task3
	cout << "Task 3" << endl;
	Task3();
	cout << endl;
	//task4
	cout << "Task 4" << endl;
	Task4();
	cout << endl;
	//task5
	cout << "Task 5" << endl;
	Task5();
	cout << endl;
	//task6
	cout << "Task 6" << endl;
	Task6();
	cout << endl;
	system("PAUSE");
	return 0;
}
```
#### “插花游戏”
```cpp
#include <iostream>
using namespace std;
bool isPrime(int n)
{
	for (int i = 2; i * i <= n; i++)
		if (n % i == 0)
			return false;
	return true;
}
int main()
{
	for (int n = 2; n <= 100; n++)
	{
		if (isPrime(n))
			cout << n << endl;
	}
	return 0;
}
```
#### “筛法”
```cpp
#include <iostream>
using namespace std;
const int N = 100;
bool seive[N + 1];
int main() {
	for (int i = 2; i <= N; i++)
		seive[i] = true;
	for (int d = 2; d * d <= N; d++)
		if (seive[d])
			for (int n = d * d; n <= N; n += d)
				seive[n] = false;
	for (int n = 2; n <= N; n++)
		if (seive[n])
			cout << n << endl;
	return 0;
}
```
#### “小朋友数人数”
```cpp
#include <iostream>
using namespace std;
int main()
{
	int r3 = 0, r5 = 0, r7 = 0, seive[200];
	cin >> r3 >> r5 >> r7;
	for (int i = 0; i < 200; i++)
		seive[i] = 0;
	for (int i = r3; i < 200; i = i + 3)
		seive[i]++;
	for (int i = r5; i < 200; i = i + 5)
		seive[i]++;
	for (int i = r7; i < 200; i = i + 7)
		seive[i]++;
	for (int i = 0; i < 200; i++)
		if (seive[i] == 3)
		{
			cout << i << endl;
			break;
		}
	return 0;
}
```
#### “折半查找”
```cpp
#include <iostream>
using namespace std;
int main()
{
	int cards[13] = { 101, 112, 113, 206, 207, 208,
	303, 304, 309, 311, 402, 405, 410 };
	int id = -1, low = 0, high = 12;
	while (low <= high)
	{
		int middle = (low + high) / 2;
		if (cards[middle] == 112)
		{
			id = middle;
			break;
		}
		else if (cards[middle] > 112)
			high = middle - 1;
		else
			low = middle + 1;
	}
	cout << "黑桃Q在第" << id + 1 << "张" << endl;
	return 0;
}
```
#### “排序问题”
```cpp
#include <iostream>
using namespace std;
void InsertionSort(int cards[], int n)
{
	for (int i = 1; i < n; i++)
	{
		int target = cards[i], pos = 0;
		while (target > cards[pos])
			pos++;
		for (int j = i; j > pos; j--)
			cards[j] = cards[j - 1];
		cards[pos] = target;
	}
}
void SelectionSort(int cards[], int n)
{
	for (int i = 0; i < n; i++)
	{
		int min = cards[i], min_id = i;
		for (int j = i + 1; j < n; j++)
			if (cards[j] < min)
			{
				min = cards[j];
				min_id = j;
			}
		cards[min_id] = cards[i];
		cards[i] = min;
	}
}
int main()
{
	int cards[13] = { 101, 113, 303, 206, 405, 208,
	311, 304, 410, 309, 112, 207, 402 };
	for (int i = 0; i < 13; i++)
		cout << cards[i] << " ";
	cout << endl;
	SelectionSort(cards, 13);
	for (int i = 0; i < 13; i++)
		cout << cards[i] << " ";
	cout << endl;
	return 0;
}
```