---
title: Basic of Program Design Ⅳ
password: ''
date: 2024-11-18 00:17:37
tags:  
  - Learning-Notes
  - Tsinghua
categories: "Project"
---
> ***In the future, don't blame everyone around you for your problems,just choose the person you hate the most, and then blame her for everything.***
***When you write an if without else, be sure to spend a few extra seconds thinking about whether this else can really be without.***
***我一个 homeless 居然有 homework. God bless you.***

### 回溯好难，用复杂数列类比？
### 筛选查找
- #include <iostream> 注释掉之后仍然会对下面的有影响 ambiguous 
- https://blog.csdn.net/qq_41898367/article/details/119925734
- [C++]七大经典排序算法详解（代码实现+解析）
- https://blog.csdn.net/cpb____/article/details/114917782
- [计算机专业不得不看的15种排序算法，7分钟动画演示](https://www.bilibili.com/video/BV1LG411U7g4/?spm_id_from=333.337.search-card.all.click&vd_source=ca90579a22163e655f3ea46dc72e6f24)
#### 线性查找 linear search
```cpp
int linearSearch(const int arr[], int n, int target)
{
    for (int i = 0; i < n; i++)
    {
        if (arr[i] == target)
        {
            return i;
        }
    }
    return -1;
}
```
#### 二分查找 binary search
```cpp
int search(int nums[], int size, int target) //nums是数组，size是数组的大小，target是需要查找的值
{
    int left = 0;
    int right = size - 1;	// 定义了target在左闭右闭的区间内，[left, right]
    while (left <= right) {	//当left == right时，区间[left, right]仍然有效
        int middle = left + ((right - left) / 2);//等同于 (left + right) / 2，防止溢出
        if (nums[middle] > target) {
            right = middle - 1;	//target在左区间，所以[left, middle - 1]
        } else if (nums[middle] < target) {
            left = middle + 1;	//target在右区间，所以[middle + 1, right]
        } else {	//既不在左边，也不在右边，那就是找到答案了
            return middle;
        }
    }
    //没有找到目标值
    return -1;
}
```
#### 二分查找 another method
```cpp
int main() {
	int cards[13] = { 101, 112, 113, 206, 207, 208, 303, 304, 309, 311, 402, 405, 410 };
	int id = -1, low = 0, high = 12;
	while (low <= high) {
		int middle = (low + high) / 2;
		if (cards[middle] == 112) {
			id = middle;
			break;
		}
		else if (cards[middle] > 112)
			high = middle - 1;
		else
			low = middle + 1;
	}
	cout << id + 1 << endl;
	return 0;
}
```
#### 选择排序 Selection-sort
```cpp
void SelectionSort(int arr[],int len)
{
	for(int i = 0; i < len; i++)
	{
		int min = i;
		for(j = i + 1; j < len; j++)
		{
			if(a[j] < a[min])
			min = j;
		}
		int temp = a[min];
		a[min] = a[i];
		a[i] = temp;
	}
}
```
#### 插入排序 Insertion-Sort
```cpp
void insertSort(int a[], int n)
{
   for(int i = 1; i < n; i++) //第一个元素作为基准元素，从第二个元素开始把其插到正确的位置
   {
	  if(a[i] < a[i-1]) //如果第i个元素比前面的元素小
	  {
	      int j = i-1;     //需要判断第i个元素与前面的多个元素的大小，换成j继续判断
          int x = a[i]; //将第i个元素复制为哨兵
	      while(j >= 0 && x < a[j]) //找哨兵的正确位置，比哨兵大的元素依次后移
	      {
             a[j+1] = a[j]; 
	         j--;
	      }
	      a[j+1] = x;  //把哨兵插入到正确的位置
	  }   
   }
}
```
#### 归并排序 Merge-Sort
```cpp
/* 初始版本，升序排序 */
/* 时间复杂度：O(nlbn) 将n个待排序记录归并次数为lbn，一趟归并O(n)
   空间复杂度：O(n) 递归栈最大深度为[lbn] + 1 ，而辅助数组大小为n
   稳定：无论最好还是最坏情况时间复杂度都是O(nlbn)
*/
 
void Merge(int arr[], int n)
{
    int temp[n]; // 用一个额外的数组来进行排序
    int b = 0; // 额外数组的起始位置
    int mid = n / 2; // mid将数组从中间划分，前后两半都有序
    int first = 0, second = mid; // 两个有序序列的起始位置
 	//以下操作类似于将两个数组合并为一个有序数组
    while (first < mid && second < n)
    {
        if (arr[first] <= arr[second]) // 比较两个序列
        	//这步操作相当于把第一个数组的值放到用来排序的数组，接着两个指针后移对下一个值进行操作
            temp[b++] = arr[first++];
        else
            temp[b++] = arr[second++];
    }
 
    while(first < mid)  // 将剩余子序列复制到辅助序列中
            temp[b++] = arr[first++];
    while(second < n)
            temp[b++] = arr[second++];
    for (int i = 0; i < n; ++i) // 辅助序列复制到原序列
        arr[i] = temp[i];
}
 
void MergeSort(int arr[], int n)
{
    if (n <= 1) // 递归出口
        return;
    if (n > 1)
    {
        MergeSort(arr, n / 2); // 对前半部分进行归并排序
        MergeSort(arr + n / 2, n - n / 2); // 对后半部分进行归并排序
        Merge(arr, n); // 归并两部分
    }
}
```
#### 快速排序 Quick-Sort
```cpp
void Quicksort(int arr[], int low, int high) {
	if (low < high) {
		//双指针，一个指向数组起始，一个指向数组末尾
		int i = low;
		int j = high;
		//将数组的第一个元素作为key寻找它的位置
		//key找到它的位置后，以它为分界线，左右两个数组分治
		int key = arr[i];
		while (i < j) {
			//两个指针不相遇，且指针指向的值大于key时，不断左移
			while (i < j && arr[j] >= key)
				j--;
			if (i < j) arr[i] = arr[j];
			//两个指针不相遇，且指针指向的值小于key时，不断右移
			while (i < j && arr[i] <= key)
				i++;
			if (i < j) arr[j] = arr[i];
		}
		//将key放在适合的位置
		arr[i] = key;
		//分治
		Quicksort(arr, low, i - 1);
		Quicksort(arr, i + 1, high);
	}
}
```
### 例题分析
#### 第五周“矩阵填充”
```cpp
#include<iostream>
#include <iomanip>
using namespace std;

const int N = 5;
void FillMatrix(int matrix[N][N], int size, int num, int offset) {	
	if (size == 0)
		return;
	if (size == 1) {
		matrix[offset][offset] = num;
		return;
	}
	for (int i = 0; i < size - 1; i++) {
		matrix[offset + i][offset] = num + i;
		matrix[offset + size - 1][offset + i] = num + (size - 1) + i;
		matrix[offset + size - 1 - i][offset + size - 1] = num + 2 * (size - 1) + i;
		matrix[offset][offset + size - 1 - i] = num + 3 * (size - 1) + i;
	}
	FillMatrix(matrix, size - 2, num + 4 * (size - 1), offset + 1);
}
int main() {
	int matrix[N][N] = { 0 }; // 初始化矩阵为0
	FillMatrix(matrix, N, 0, 0); // 从整个矩阵开始填充，从数字0开始
	// 打印矩阵
	for (int i = 0; i < N; ++i) {
		for (int j = 0; j < N; ++j) {
			std::cout << std::setw(3) << matrix[i][j]; // 使用 std::setw 来格式化输出，使数字对齐
		}
		cout << endl;
	}
	return 0;
}
```
#### 第五周“青蛙过河”
```cpp
#include<iostream>
using namespace std;

int Jump(int s, int y) {
	int k = 0;
	if (s == 0)
		k = y + 1;
	else
		k = 2 * Jump(s - 1, y);
	return k;
}
int main() {
	cout << Jump(1, 10) << endl;
}
```
#### 第五周“分书”的C++参考程序
```cpp
#include <iostream> // cout
using namespace std;
int Num; /// 方案数
int take[5]; /// 5本书分别分给谁（用户编号）
bool assigned[5]; /// 5本书是否已分配
int like[5][5] = { {0,0,1,1,0},
{1,1,0,0,1},
{0,1,1,0,1},
{0,0,0,1,0},
{0,1,0,0,1} };
void Try(int id)
{
	/// 递归中止条件：所有读者均已分配合适书籍
	if (id == 5)
	{
		/// 方案数加1
		Num++;
		/// 输出方案细节
		cout << "第" << Num << "个方案（按ABCDE次序）: ";
		for (int i = 0; i < 5; i++)
			cout << take[i] << ' ';
		cout << endl;
		return;
	}
	/// 逐一为每本书找到合适的读者
	for (int book = 0; book <= 4; book++)
	{
		/// 是否满足分书条件
		if ((like[id][book] == 1) && !assigned[book])
		{
			/// 记录当前这本书的分配情况
			take[id] = book;
			assigned[book] = true;
			/// 为下一位读者分配合适书籍
			Try(id + 1);
			/// 将书退还（回溯），尝试另一种方案
			assigned[book] = false;
		} // __if__
	} // __for__
}
void Assign(int id)
{
	/// 逐一为每本书找到合适的读者
	for (int book = 0; book <= 4; book++)
	{
		/// 是否满足分书条件
		if ((like[id][book] == 1) && !assigned[book])
		{
			/// 记录当前这本书的分配情况
			take[id] = book;
			assigned[book] = true;
			/// 是否已给所有人分了书？如否，则继续给下一人分书；若是，则输出分书方案
			if (id < 4)
				Assign(id + 1);
			else
			{
				/// 方案数加1
				Num++;
				/// 输出方案细节
				cout << "第" << Num << "个方案（按ABCDE次序）: ";
				for (int i = 0; i < 5; i++)
					cout << take[i] + 1 << ' ';//###这里注意加一
				cout << endl;
			}
			/// 将书退还（回溯），尝试另一种方案
			assigned[book] = false;
		} // __if__
	} // __for__
}
int main()
{
	Num = 0; // 分书方案数初始值
	for (int book = 0; book < 5; book++)
		assigned[book] = false;
	// 从第0个人（A）开始分书
	Try(0);
	return 0;
}
```
#### 第五周“八皇后”的C++参考程序