---
title: Basic of Program Design Ⅴ
password: ''
date: 2024-11-21 12:26:06
tags:  
  - Learning-Notes
  - Tsinghua
categories: "Project"
---
> ***Knowledge isn't free. You have to pay attention.***

- [动态规划的简单套路（C++描述）](https://www.bilibili.com/video/BV14P4y117r9/?spm_id_from=333.337.search-card.all.click&vd_source=ca90579a22163e655f3ea46dc72e6f24)
- [10分钟彻底搞懂“动态规划”算法](https://www.bilibili.com/video/BV1AB4y1w7eT/?spm_id_from=333.337.search-card.all.click&vd_source=ca90579a22163e655f3ea46dc72e6f24)
- https://blog.csdn.net/NaturalNumber/article/details/113139718
- https://oi-wiki.org/dp/ 3位数就要用动态规划了算不完，根本算不完。。。
**典型的递归算法，有大量的重复计算！保存计算结果。。。**
**动态规划，理解了，但是写不出来啊啊。。。**

> **对于每一行输入，输出斐波那契数列第n项的值f(n)。一个cin一个cout**
> **共n行，每一行为分解质因数得到的等式，因子从小到大进行排列。用数组记录**
### 兔子数列 
```cpp
#include <iostream>
#include <vector>
using namespace std;

int fibonacci(int n) {
    if (n < 0) {
        throw invalid_argument("n必须是非负整数");
    } else if (n == 0) {
        return 0;
    } else if (n == 1) {
        return 1;
    }

    // 创建一个数组来存储已计算的斐波那契数
    vector<int> fib(n + 1);
    fib[0] = 0;
    fib[1] = 1;

    // 填充数组
    for (int i = 2; i <= n; ++i) {
        fib[i] = fib[i - 1] + fib[i - 2];
    }

    return fib[n];
}

int main() {
    int n = 10; // 可以修改 n 的值来计算不同的斐波那契数
    cout << "斐波那契数列的第 " << n << " 项是: " << fibonacci(n) << endl;
    return 0;
}
```
#### 第六周“兔子数列”的C++参考程序1
```cpp
// 兔子数列，按小兔子、大兔子分别递推
#include <iostream>
using namespace std;
int main() {
	int n;
	cin >> n;
	int small[12], big[12];        // 定义数组
	small[0] = 1;                  // 设定初值
	big[0] = 0;
	for (int i = 1; i < n; i++) {  // 递推
		small[i] = big[i - 1];
		big[i] = small[i - 1] + big[i - 1];
	}// 输出总数
	cout << "第" << n << "个月的兔子总数是" << small[n - 1] + big[n - 1] << endl;
	return 0;
}
```
#### 第六周“兔子数列”的C++参考程序2
```cpp
// 兔子数列，按兔子总数递推
#include <iostream>
using namespace std;
int main() {
	int n;
	cin >> n;
	int total[12];// 定义数组
	total[0] = 1; // 设定初值
	total[1] = 1;
	for (int i = 2; i < n; i++) {// 递推
		total[i] = total[i - 2] + total[i - 1];
	}// 输出总数
	cout << "第" << n << "个月的兔子总数是" << total[n - 1] << endl;
	return 0;
}
```
#### 第六周“兔子数列”的C++参考程序3
```cpp
// 兔子数列，按小兔子、大兔子分别递推，不用数组
#include <iostream>
using namespace std;
int main() {
	int n;
	cin >> n;
	int dang_yue;
	int qian_yue = 1;             // 设定初值
	int shang_yue = 1;            // 设定初值
	for (int i = 2; i < n; i++) { // 递推
		dang_yue = qian_yue + shang_yue;
		qian_yue = shang_yue;
		shang_yue = dang_yue;
	}// 输出总数
	cout << "第" << n << "个月的兔子总数是" << dang_yue << endl;
	return 0;
}
```
### 分鱼问题
#### 第六周“分鱼”的C++参考程序1
```cpp
// 分鱼问题，从A开始递推，未优化
#include <iostream>
using namespace std;
int main() {
	int num[5];// 定义数组
	for (num[0] = 1; ; num[0]++) {// 枚举num[0]
		if (num[0] % 5 != 1)
			continue;
		num[1] = (num[0] - 1) / 5 * 4;
		if (num[1] % 5 != 1)
			continue;
		num[2] = (num[1] - 1) / 5 * 4;
		if (num[2] % 5 != 1)
			continue;
		num[3] = (num[2] - 1) / 5 * 4;
		if (num[3] % 5 != 1)
			continue;
		num[4] = (num[3] - 1) / 5 * 4;
		if (num[4] % 5 != 1)
			continue;
		break;
	}// 输出答案
	for (int i = 0; i < 5; i++)
		cout << num[i] << ' ';
	return 0;
}
```
#### 第六周“分鱼”的C++参考程序2
```cpp
// 分鱼问题，从A开始递推，使用for循环简化中间计算
#include <iostream>
using namespace std;
int main() {
	int num[5];// 定义数组
	for (num[0] = 1; ; num[0]++) {	// 枚举num[0]
		if (num[0] % 5 != 1)
			continue;
		int i = 0;
		for (; i < 4; ++i) {
			num[i + 1] = (num[i] - 1) / 5 * 4;
			if (num[i + 1] % 5 != 1)
				break;
		}
		if (i >= 4) // 已找到答案
			break;
	}	// 输出答案
	for (int i = 0; i < 5; i++)
		cout << num[i] << ' ';
	return 0;
}
```
#### 第六周“分鱼”的C++参考程序3
```cpp
// 分鱼问题，从A开始递推，使用for循环简化中间计算，优化枚举
#include <iostream>
using namespace std;
int main() {
	int num[5];　 
	for (num[0] = 16; ; num[0] += 5) {
		int i = 0;
		for (; i < 4; ++i) {
			num[i + 1] = num[i] / 5 * 4; // 整数除法，可以不减1就直接除以5
			if (num[i + 1] % 5 != 1)
				break;
		}
		if (i >= 4) // 已找到答案
			break;
	}
	for (int i = 0; i < 5; ++i)
		cout << num[i] << ' ';
	return 0;
}
```
#### 第六周“分鱼”的C++参考程序4
```cpp
// 分鱼问题，从E开始递推
#include <iostream>
using namespace std;
int main()
{
	// 定义数组
	int num[5];
	// 从6开始枚举num[4]
	for (num[4] = 6; ; num[4] += 5)
	{
		if (num[4] % 4 != 0)
			continue;
		num[3] = num[4] / 4 * 5 + 1;
		if (num[3] % 4 != 0)
			continue;
		num[2] = num[3] / 4 * 5 + 1;
		if (num[2] % 4 != 0)
			continue;
		num[1] = num[2] / 4 * 5 + 1;
		if (num[1] % 4 != 0)
			continue;
		num[0] = num[1] / 4 * 5 + 1;
		break;
	}
	// 输出答案
	for (int i = 0; i < 5; ++i)
		cout << num[i] << ' ';
	return 0;
}
```
#### 第六周“分鱼”的C++参考程序5
```cpp
// 分鱼问题，从E开始递推，使用for循环简化中间计算
#include <iostream>
using namespace std;
int main()
{
	// 定义数组
	int num[5];
	// 从6开始枚举num[4]
	for (num[4] = 6; ; num[4] += 5)
	{
		int i = 4;
		for (; i >= 1; --i)
		{
			if (num[i] % 4 != 0)
				break;
			num[i - 1] = num[i] / 4 * 5 + 1;
		}
		if (i == 0) // 已找到答案
			break;
	}
	// 输出答案
	for (int i = 0; i < 5; ++i)
		cout << num[i] << ' ';
	return 0;
}
```
#### 第六周“分鱼”的C++参考程序6
```cpp
// 分鱼问题，从E开始递推，使用for循环简化中间计算，优化枚举
#include <iostream>
using namespace std;
int main()
{
	int num[5];
	for (num[4] = 16; ; num[4] += 20)
	{
		int i = 3;
		for (; i >= 1; --i)
		{
			num[i] = num[i + 1] / 4 * 5 + 1;
			if (num[i] % 4 != 0)
				break;
		}
		if (i == 0) // 已找到答案
			break;
	}
	num[0] = num[1] / 4 * 5 + 1;
	for (int i = 0; i < 5; ++i)
		cout << num[i] << ' ';
	return 0;
}
```
### 橱窗插花
#### 第六周“橱窗插花”的C++参考程序1
```cpp
// 橱窗插花问题，按二进制枚举
#include <iostream>
using namespace std;
const int V = 5;
const int F = 3;
void ToBinary(int num, int binary[V]) {// 把num表示为二进制
	for (int i = 0; i < V; i++) {
		binary[i] = num & 1; // 取最低位
		num = num >> 1; // 右移1位
	}
}// 计算V位二进制中1的个数
int Count1(int binary[V]) {
	int count = 0;
	for (int i = 0; i < V; i++)
		count += binary[i];
	return count;
}
int main() {
	int beauty[V][F] = { {7, 5, -21}, {23, 21, 5}, {-5, -4, -4}, {-24, 10, -20}, {16, 23, 20} };// 定义美感数组
	int best_beauty = 0, best_put = 0;// 定义最大美感得分和、相应方案
	for (int i = 0; i < (1 << V); i++) {// 枚举0~2^V-1
		int binary[V] = { 0 };       // 计算具体插花方案
		ToBinary(i, binary);
		if (Count1(binary) != F) // 不是合法方案
			continue;
		int sum_beauty = 0;// 计算美感得分和
		for (int vase = 0, flower = 0; vase < V; vase++) {
			if (binary[vase] == 1) {// 插了花
				sum_beauty += beauty[vase][flower];
				flower++;
			}
		}// 维护最大美感得分和、相应方案
		if (sum_beauty > best_beauty) {
			best_beauty = sum_beauty;
			best_put = i;
		}
	}// 输出答案
	cout << "最大美感得分和：" << best_beauty << endl;
	cout << "插花方法：";
	int best_binary[V] = { 0 };
	ToBinary(best_put, best_binary);
	for (int vase = 0, flower = 1; vase < V; vase++) {
		if (best_binary[vase] == 1) {
			cout << flower;
			flower++;
		}
		else
			cout << 0;
	}
	return 0;
}
```
#### 第六周“橱窗插花”的C++参考程序2
```cpp
// 橱窗插花问题，按二进制枚举，记录部分美感和
#include <iostream>
using namespace std;
const int V = 5;
const int F = 3;
// 把num表示为二进制
void ToBinary(int num, int binary[V])
{
	for (int i = 0; i < V; i++)
	{
		binary[i] = num & 1; // 取最低位
		num = num >> 1; // 右移1位
	}
}
// 计算V位二进制中1的个数和最高位1的位置
int Count1(int binary[V], int& high)
{
	int count = 0;
	high = -1;
	for (int i = 0; i < V; i++)
		if (binary[i] == 1)
		{
			high = i;
			count++;
		}
	return count;
}
int main()
{
	// 定义美感数组
	int beauty[V][F] = { {7, 5, -21}, {23, 21, 5}, {-5, -4, -4}, {-24, 10, -20}, {16, 23, 20} };
	// 定义部分美感和数组，设定初值
	int partial_sum[1 << V] = { 0 };
	// 定义最大美感得分和、相应方案
	int best_beauty = 0, best_put = 0;
	// 枚举1~2^V-1
	for (int i = 1; i < (1 << V); i++)
	{
		// 计算部分插花方案
		int binary[V] = { 0 };
		ToBinary(i, binary);
		int high;
		int flowers = Count1(binary, high);
		if (flowers > F) // 不是合法的部分方案
			continue;
		// 递推部分美感和
		partial_sum[i] = beauty[high][flowers - 1] + partial_sum[i - (1 << high)];
		// 维护最大美感得分和、相应方案
		if (flowers == F && partial_sum[i] > best_beauty)
		{
			best_beauty = partial_sum[i];
			best_put = i;
		}
	}
	// 输出答案
	cout << "最大美感得分和：" << best_beauty << endl;
	cout << "插花方法：";
	int best_binary[V] = { 0 };
	ToBinary(best_put, best_binary);
	for (int vase = 0, flower = 1; vase < V; vase++)
		if (best_binary[vase] == 1)
		{
			cout << flower;
			flower++;
		}
		else
			cout << 0;
	return 0;
}
```
#### 第六周“橱窗插花”的C++参考程序3
```cpp
// 橱窗插花问题，动态规划
// 记录每个状态的最优决策，最优策略需回溯
#include <iostream>
using namespace std;
const int V = 5;
const int F = 3;
int main()
{
	// 定义美感数组
	int beauty[V][F] = { {7, 5, -21}, {23, 21, 5}, {-5, -4, -4}, {-24, 10, -20}, {16, 23, 20} };
	// 定义最优部分美感和数组，设定递推初值
	int best_partial[V + 1][F + 1] = { {0} };
	// 定义记录是否插花的数组
	bool put[V + 1][F + 1] = { {false} };
	// 按m个花瓶插n朵花递推
	for (int m = 1; m <= V; m++)
		for (int n = 1; n <= m && n <= F; n++)
		{
			// 新花瓶插花
			best_partial[m][n] = best_partial[m - 1][n - 1] + beauty[m - 1][n - 1];
			put[m][n] = true;
			if (n < m && best_partial[m][n] < best_partial[m - 1][n]) // 新花瓶不插花更优
			{
				best_partial[m][n] = best_partial[m - 1][n];
				put[m][n] = false;
			}
		}
	// 输出答案
	cout << "最大美感得分和：" << best_partial[V][F] << endl;
	cout << "插花方法：";
	for (int m = V, n = F; m >= 1; m--)
		if (put[m][n])
		{
			cout << n;
			n--;
		}
		else
		{
			cout << '0';
		}
	return 0;
}
```
#### 第六周“橱窗插花”的C++参考程序4
```cpp
// 橱窗插花问题，动态规划，
// 记录当前子问题的最优策略，无需回溯
#include <iostream>
using namespace std;
const int V = 5;
const int F = 3; 
// 把num表示为二进制##############################
void ToBinary(int num, int binary[V])
{
	for (int i = 0; i < V; i++)
	{
		binary[i] = num & 1; // 取最低位
		num = num >> 1; // 右移1位
	}
}
int main()
{
	// 定义美感数组
	int beauty[V][F] = { {7, 5, -21}, {23, 21, 5}, {-5, -4, -4}, {-24, 10, -20}, {16, 23, 20} };
	// 定义最优部分美感和数组，设定递推初值
	int best_partial[V + 1][F + 1] = { {0} };
	// 定义记录是否插花的数组
	int best_put[V + 1][F + 1] = { {0} };
	// 按m个花瓶插n朵花递推
	for (int m = 1; m <= V; m++)
		for (int n = 1; n <= m && n <= F; n++)
		{
			// 新花瓶插花
			best_partial[m][n] = best_partial[m - 1][n - 1] + beauty[m - 1][n - 1];
			best_put[m][n] = best_put[m - 1][n - 1] + (1 << (m - 1));
			if (n < m && best_partial[m][n] < best_partial[m - 1][n]) // 新花瓶不插花更优
			{
				best_partial[m][n] = best_partial[m - 1][n];
				best_put[m][n] = best_put[m - 1][n];
			}
		}
	// 输出答案
	cout << "最大美感得分和：" << best_partial[V][F] << endl;
	cout << "插花方法：";
	int best_binary[V] = { 0 };
	ToBinary(best_put[V][F], best_binary);
	for (int vase = 0, flower = 1; vase < V; vase++)
		if (best_binary[vase] == 1)
		{
			cout << flower;
			flower++;
		}
		else
			cout << 0;
	return 0;
}
```
### 最长公共子序列
```cpp
// 最长公共子序列，动态规划
#include <iostream>
#include <cstring>
using namespace std;
const int M = 100;
const int N = 100;
int main()
{
	char A[] = "abcbdacb";
	char B[] = "bdcab";
	int lcs[M][N]; // M、N足够大
	int decision[M][N];
	enum
	{
		I_J, // 1 + lcs[i+1][j+1]
		I_1, // lcs[i+1][j]
		J_1 // lcs[i][j+1]
	};
	int m = strlen(A);
	int n = strlen(B);
	// 设定递推初值
	for (int j = 0; j < n + 1; j++)
		lcs[m][j] = 0; // lcs[m][?]=0
	for (int i = 0; i < m + 1; i++)
		lcs[i][n] = 0; // lcs[?][n]=0
	// 递推
	for (int i = m - 1; i >= 0; i--)
		for (int j = n - 1; j >= 0; j--)
			if (A[i] == B[j])
			{
				lcs[i][j] = 1 + lcs[i + 1][j + 1];
				decision[i][j] = I_J;
			}
			else if (lcs[i][j + 1] < lcs[i + 1][j])
			{
				lcs[i][j] = lcs[i + 1][j];
				decision[i][j] = I_1;
			}
			else
			{
				lcs[i][j] = lcs[i][j + 1];
				decision[i][j] = J_1;
			}
	// 输出
	for (int i = 0, j = 0; i < m && j < n;)
		switch (decision[i][j])
		{
		case I_J:
			cout << A[i];
			i++;
			j++;
			break;
		case I_1:
			i++;
			break;
		case J_1:
			j++;
			break;
		}
	return 0;
}
```