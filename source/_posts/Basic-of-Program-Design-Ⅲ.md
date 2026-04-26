---
title: Basic of Program Design Ⅲ
password: ''
date: 2024-11-16 21:07:07
tags: 
  - Learning-Notes
  - Tsinghua
categories: "Project"
---
>Jabonese agcent is vely, vely hard to undershdand.
Indeian ekusento ishi belly belly haudo tsu andasudando！
>"What is your student ID number?"
"sex, sex, sex, Oh! Free sex tonight"（66603629）
```cpp
#include<iostream>
using namespace std;
bool isPrime(int n)
{
	for (int i = 2; i * i <= n; i++)
		if (n % i == 0)
			return false;
	return true;
}
int main() {
	int n;
	int num[101];
	cin >> n;
	for (int i = 1; i <= n; i++) {
		cin >> num[i];
	}
	for (int i = 1; i <= n; i++) {
		cout << num[i] << "=";
		for (int f = 2; f <= num[i]; f++) {
			if (isPrime(f) && (num[i] % f == 0) && (num[i] / f != 1)) {
				num[i] = num[i] / f;
				cout << f << "*";
				f = 1;
			}
			else if (isPrime(f) && (num[i] % f == 0) && (num[i] / f == 1)) {
				num[i] = num[i] / f;
				cout << f << endl;
				f = 1;
			}
		}
	}
}
```
```cpp
#include<iostream>
using namespace std;
bool isPrime(int n)
{
	for (int i = 2; i * i <= n; i++)
		if (n % i == 0)
			return false;
	return true;
}
int main() {
	int n, num;
	cin >> n;
	for (int i = 1; i <= n; i++) {
		cin >> num;
		cout << num << "=";
		for (int f = 2; f <= num; f++) {
			if (isPrime(f) && (num % f == 0) && (num / f != 1)) {
				num = num / f;
				cout << f << "*";
				f = 1;
			}
			else if (isPrime(f) && (num % f == 0) && (num / f == 1)) {
				num = num / f;
				cout << f ;
				f = 1;
			}
		}
	}
}
```
### Josephus Problem
```cpp
#include<iostream>
using namespace std;
int main()
{//在全局或静态作用域中声明的数组通常不受栈内存大小的限制
 //因为它们存储在数据段中，而不是栈上。但是，它们仍然受到可用内存和编译器/链接器限制的影响。
	bool a[101]={0}; //please don't use "a[101]=0;"
	int n,m,i,f=0,t=0,s=0;
	cin>>n>>m;
	do
	{
		++t;     //逐个枚举圈中的所有位置
		if(t>n)
			t=1; //数组模拟环状，最后一个与第一个相连
        if(!a[t])
            s++; //第t个位置上有人则报数
        if(s==m) //当前报的数是m
        {
            s=0; //计数器清零
            cout<<t<<' ';//输出被杀人编号
            a[t]=1;//此处人已死，设置为空
            f++;   //死亡人数+1
        }
    }while(f!=n);  //直到所有人都被杀死为止
    return 0;
}

#include <iostream>
#include <iomanip>
#include <cmath>
using namespace std;
int main()
{
    bool a[10010],i,t,k,m;
    cin>>t>>m;
    for(i=1;i<=t;i++)
    {
        a[i]=1;
    }
    k=0;
    while(t>0)    {
        for(i=1;i<=t;i++)
        {
            if(a[i]==1)
            {
                k++;
                if(k==m)
                {
                    k=0;
                    a[i]=0;
                    cout<<setw(3)<<i;
                    t--;
                    if(t==0)
                    {
                        break;
                    }
                }
            }
        }
    }
    return 0;
}

#include <iostream>
using namespace std;
int main()
{
    int n, f = 0, m;
    cin >> n >>m;
    for (int i = 1; i <= n; i++) f = (f + m) % i;
    cout << f + 1 << endl;
}
```
### 第三周逻辑推理
>位与 AND 	& 	位异或 XOR 	^ 	位或 OR 	| 
>杂项运算符 Condition ? X : Y	
https://www.runoob.com/cplusplus/cpp-operators.html
#### 1.“谁是凶手”
>巴斯维克命案抓住了六个嫌疑犯，他们的口供如下：
 A：我不是罪犯
 B：A、C中有一个是罪犯 
 C：A和B说了假话
 D：C和F说了假话
 E：其他五个人中，**只有**A和D说了真话
 F：我是罪犯
>他们中只有一半说了真话，凶手只有一个。
***本题答案不唯一，请编程找出所有可能的凶手。***
```cpp
#include <iostream>
using namespace std;
int main() // I did it.
{
	char bad_man; // 定义变量
	for (bad_man = 'A'; bad_man <= 'F'; bad_man++)
	{
		int count = 0; // 说真话的人数
		if (bad_man != 'A')
			count++;// A说了真话
		if (bad_man == 'A' || bad_man == 'C') // 注意优先级
			count++;// B说了真话
		if (1 == 0)   // C must be wrong
			count++;// C说了真话
		if (bad_man != 'F')
			count++;// D说了真话
		if ((bad_man != 'A') && (bad_man != 'C') && (bad_man != 'F'))//'只有'的逻辑好麻烦
			count++;// E说了真话
		if ((bad_man == 'F'))
			count++;// F说了真话

		if (count == 3) // 有3个人说了真话
		{
			cout << bad_man << endl; // 输出做好事的人			
		}
	}
		return 0;
}
```
```cpp
#include<stdio.h>
int main()
{
	int i,j,sum;
	for (i=0;i<6;i++)//假设第i个是凶手
	{
	    sum = 0;
	    int lie[6] = {0,0,0,0,0,0};
	    lie[0] = (i==0);//A是否说谎
	    lie[1] = (i!=0 && i!=2);//B是否说谎
	    lie[2] = !(lie[0] && lie[1]);//有一个说真话则C在说谎
	    lie[5] = (i!=5);//F是否说谎
	    lie[3] = !(lie[2] && lie[5]);
	    lie[4] = !(((lie[0]||lie[3])==0) && ((lie[1]&&lie[2]&&lie[5])==1));
	    for (j=0;j<6;j++)
	        sum+=lie[j];
	    if (sum==3)//说谎人数为3人
	        printf("%c\n",i+65);
	}
	return 0;
}
```
#### 2.“谁做的好事”
```cpp
#include <iostream>
using namespace std;
int main()
{
	char good_man; // 定义变量，表示“做好事的人”
	for (good_man = 'A'; good_man <= 'D'; good_man++)
	{
		int count = 0; // 说真话的人数
		if (good_man != 'A') // A说了真话
			count++;
		if (good_man == 'C') // B说了真话
			count++;
		if (good_man == 'D') // C说了真话
			count++;
		if (good_man != 'D') // D说了真话
			count++;
		if (count == 3) // 有3个人说了真话
		{
			cout << good_man << endl; // 输出做好事的人
			break;
		}
	}
	return 0;
}
```
#### 3.“谁是嫌疑犯”
```cpp
#include <iostream>
using namespace std;
int main()
{
	for (int i = 0; i < (1 << 6); i++)  // 1 << 6   2^6
	{
		int A = (i >> 5) & 1;           // 5 + 1 = 6
		int B = (i >> 4) & 1;           
		int C = (i >> 3) & 1;           // ### i 依然是10进制
		int D = (i >> 2) & 1;           
		int E = (i >> 1) & 1;           // 位运算自动转换为二进制
		int F = i & 1;
		bool b1 = (A == 1) || (B == 1);
		bool b2 = ((A == 1) && (E == 1)) ||
			((A == 1) && (F == 1)) || ((E == 1) && (F == 1));
		bool b3 = !((A == 1) && (D == 1));
		bool b4 = ((B == 1) && (C == 1)) || ((B == 0) && (C == 0));
		bool b5 = ((C == 1) && (D == 0)) || ((C == 0) && (D == 1));
		bool b6 = ((D == 0) && (E == 0)) || (D == 1);
		if (b1 && b2 && b3 && b4 && b5 && b6)
		{
			cout << A << B << C << D << E << F << endl;
			break;
		}
	}
	return 0;
}
```
### 第四周筛选查找
```cpp
/// 分治思想与递归 @ 八皇后问题
#include <iostream>
using namespace std;
const int Normalize = 9; // 用来统一数组下标
int Num; // 方案数
int q[9]; // 8个皇后所占用的行号
bool S[9]; // S[1]~S[8]，当前行是否安全
bool L[17]; // L[2]~L[16]，(i-j)对角线是否安全
bool R[17]; // R[2]~R[16]，(i+j)对角线是否安全
void Try(int col)
{
	/// 递归中止条件：所有列均已放上皇后了
	if (col == 9)
	{
		Num++;
		cout << "方案" << Num << "：";
		for (int k = 1; k <= 8; k++)
			cout << q[k] << " ";
		cout << endl;
		return;
	}
	/// 依次尝试当前列的 8 行位置
	for (int row = 1; row <= 8; row++)
	{
		/// 判断拟放置皇后的位置是否安全
		if (S[row] && R[col + row] && L[col - row + Normalize])
		{
			/// 记录位置信息（行号）
			q[col] = row;
			/// 修改三个方向的安全性标记
			S[row] = false;
			L[col - row + Normalize] = false;
			R[col + row] = false;
			/// 递归尝试放下一列
			Try(col + 1);
			/// 回溯：恢复三个方向原有安全性
			S[row] = true;
			L[col - row + Normalize] = true;
			R[col + row] = true;
		}
	}
}
void Place(int col)
{
	/// 依次尝试当前列的 8 行位置
	for (int row = 1; row <= 8; row++)
	{
		/// 判断拟放置皇后的位置是否安全
		if (S[row] && R[col + row] && L[col - row + Normalize])
		{
			/// 记录位置信息（行号）
			q[col] = row;
			/// 修改三个方向的安全性标记
			S[row] = false;
			L[col - row + Normalize] = false;
			R[col + row] = false;
			/// 是否是最后一列，若否则递归放下一列，若是则输出方案细节
			if (col < 8)
				Place(col + 1);
			else
			{
				Num++;
				cout << "方案" << Num << "：";
				for (int k = 1; k <= 8; k++)
					cout << q[k] << " ";
				cout << endl;
			}
			/// 恢复三个方向原有安全性（此过程被称为“回溯”）
			S[row] = true;
			L[col - row + Normalize] = true;
			R[col + row] = true;
		}
	}
}
int main()
{
	Num = 0;
	for (int i = 0; i < 9; i++)
		S[i] = true;
	for (int i = 0; i < 17; i++)
	{
		L[i] = true;
		R[i] = true;
	}
	Try(1); /// 从第1列开始放皇后
	return 0;
}
```

eg.1
```