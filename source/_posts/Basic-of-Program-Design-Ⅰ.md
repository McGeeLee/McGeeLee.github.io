---
title: Basic of Program Design Ⅰ
date: 2024-11-08 02:48:53
tags: 
  - Learning-Notes
  - Tsinghua
categories: "Project"
---
> ***It is time for me to make a computer-learning notebook, so here is the website. I will write some about learning or something else here.I hope everyone, you and me could have a better future. God bless you.***

## start coding
动态规划之后还有期末考试... 

> 学会使用人工智能吧 少年
1 ≤ L < U ≤ 10^9 
这个代码加上一个运行超过60ms就输出-1的模块


#### 第九周“自动售卖”参考程序
// 自动售卖，考虑非法输入
```cpp
#include <iostream>
#include <iomanip>
#include <cstdlib>
#include <cmath>
#include <cstring>
using namespace std;
void ShowMenu()
{
	cout << "****************************************" << endl; // 30个*
	cout << "* 欢迎使用自动售卖机，请输入您的选择。 *" << endl;
	cout << "* 1. 下订单 *" << endl;
	cout << "* 2. 退出自动售卖 *" << endl;
	cout << "****************************************" << endl;
}
int GetInteger()
{
	char buf[100] = { 0 };
	while (strlen(buf) == 0) // 输入直接回车
		cin.getline(buf, 100);
	return atoi(buf);
}
double GetDouble()
{
	char buf[100] = { 0 };
	while (strlen(buf) == 0) // 输入直接回车
		cin.getline(buf, 100);
	return atof(buf);
}
void DealOrder();
int main()
{
	while (1)
	{
		ShowMenu();
		cout << "您的选择是：";
		int input = GetInteger();
		switch (input)
		{
		case 1:
			DealOrder();
			break;
		case 2:
			return 0;
		}
	}
}
void ShowSubMenu()
{
	cout << "++++++++++++++++++++++++++++++++++++++++" << endl;
	cout << "+ 请选择您要的操作。 +" << endl;
	cout << "+ 1. 购买苹果(3.5元/公斤) +" << endl;
	cout << "+ 2. 购买香蕉(4.2元/公斤) +" << endl;
	cout << "+ 3. 结账 +" << endl;
	cout << "+ 4. 放弃购买 +" << endl;
	cout << "++++++++++++++++++++++++++++++++++++++++" << endl;
}
void DealOrder()
{
	double apple_price = 3.5;
	double apple_weight = 0;
	double banana_price = 4.2;
	double banana_weight = 0;
	double sum = 0;
	while (1)
	{
		ShowSubMenu();
		cout << "已购买水果总价：" << fixed << setprecision(2) << setw(8) << sum << "元" << endl;
		cout << "您的选择是：";
		int input = GetInteger();
		double weight = 1;
		switch (input)
		{
		case 1:
			cout << "请输入称重(公斤)：";
			weight = GetDouble();
			sum += apple_price * weight;
			apple_weight += weight;
			break;
		case 2:
			cout << "请输入称重(公斤)：";
			weight = GetDouble();
			sum += banana_price * weight;
			banana_weight += weight;
			break;
		case 3:
			if (sum > 0) // 确实买了水果
			{
				cout << "您一共购买了";
				if (fabs(apple_weight) > 1E-6)
					cout << apple_weight << "公斤苹果，";
				if (fabs(banana_weight) > 1E-6)
					cout << banana_weight << "公斤香蕉，";
				cout << "总价是" << sum << "元" << endl;
				system("pause");
			}
		case 4:
			system("cls");
			return;
		}
	}
}
第九周“自动售卖”读水果配置文件的参考程序
// 自动售卖，从文件读入水果和价格
#include <iostream>
#include <iomanip>
#include <cstdlib>
#include <cmath>
#include <cstring>
#include <fstream>
using namespace std;
struct Fruit_t
{
	char name[20];
	double price;
};
void ShowMenu()
{
	cout << "****************************************" << endl;
	cout << "* 欢迎使用自动售卖机，请输入您的选择。 *" << endl;
	cout << "* 1. 下订单 *" << endl;
	cout << "* 2. 退出自动售卖 *" << endl;
	cout << "****************************************" << endl;
}
int GetInteger()
{
	char buf[100] = { 0 };
	while (strlen(buf) == 0) // 输入直接回车
		cin.getline(buf, 100);
	return atoi(buf);
}
double GetDouble()
{
	char buf[100] = { 0 };
	while (strlen(buf) == 0) // 输入直接回车
		cin.getline(buf, 100);
	return atof(buf);
}
// 从配置文件读入所有水果名称和单价
bool GetFruits(Fruit_t*& fruits, int& fruit_number);
void DealOrder(Fruit_t* fruits, int fruit_number);
int main()
{
	Fruit_t* fruits = 0;
	int fruit_number;
	if (!GetFruits(fruits, fruit_number))
	{
		cout << "水果配置文件错误！" << endl;
		return 0;
	}
	while (1)
	{
		ShowMenu();
		cout << "您的选择是：";
		int input = GetInteger();
		switch (input)
		{
		case 1:
			DealOrder(fruits, fruit_number);
			break;
		case 2:
			delete[]fruits;
			return 0;
		}
	}
}
void ShowSubMenu(Fruit_t* fruits, int fruit_number)
{
	cout << "++++++++++++++++++++++++++++++++++++++++" << endl;
	cout << "+ 请选择您要的操作。 +" << endl;
	for (int i = 0; i < fruit_number; i++)
	{
		cout << "+" << setw(2) << i + 1 << ". 购买";
		cout << fruits[i].name << "(" << fixed << setw(5) << setprecision(2) << fruits[i].price << "元/公斤)";
		int len = strlen(fruits[i].name);
		for (int j = 0; j < 16 - len; j++)
			cout << ' ';
		cout << "+" << endl;
	}
	cout << "+" << setw(2) << fruit_number + 1 << ". 结账 +" << endl;
	cout << "+" << setw(2) << fruit_number + 2 << ". 放弃购买 +" << endl;
	cout << "++++++++++++++++++++++++++++++++++++++++" << endl;
}
void DealOrder(Fruit_t* fruits, int fruit_number)
{
	double* weight = new double[fruit_number];
	for (int i = 0; i < fruit_number; i++)
		weight[i] = 0;
	double sum = 0;
	while (1)
	{
		ShowSubMenu(fruits, fruit_number);
		cout << "已购买水果总价：" << fixed << setprecision(2) << setw(8) << sum << "元" << endl;
		cout << "您的选择是：";
		int input = GetInteger();
		if (input > 0 && input <= fruit_number)
		{
			cout << "请输入称重(公斤)：";
			double w = GetDouble();
			sum += fruits[input - 1].price * w;
			weight[input - 1] += w;
		}
		else if (input == fruit_number + 1)
		{
			if (sum > 1E-6) // 确实买了水果
			{
				cout << "您一共购买了";
				for (int i = 0; i < fruit_number; i++)
					if (fabs(weight[i]) > 1E-6)
						cout << weight[i] << "公斤" << fruits[i].name << "，";
				cout << "总价是" << sum << "元" << endl;
				system("pause");
			}
			break;
		}
		else if (input == fruit_number + 2)
			break;
	}
	delete[]weight;
	system("cls");
}
bool GetFruits(Fruit_t*& fruits, int& fruit_number)
{
	ifstream fin("fruits.txt");
	if (!fin)
		return false;
	fin >> fruit_number;
	fin >> ws;
	fruits = new Fruit_t[fruit_number];
	for (int i = 0; i < fruit_number; i++)
	{
		char buffer[100];
		fin.getline(buffer, 100);
		char* p = strchr(buffer, '=');
		*p = '\0';
		strcpy(fruits[i].name, buffer);
		fruits[i].price = atof(p + 1);
	}
	fin.close();
	return true;
}
第九周“自动售卖”指定界面语言的参考程序
// 自动售卖，从文件读入水果和价格，命令行参数指定语言
#include <iostream>
#include <iomanip>
#include <cstdlib>
#include <cmath>
#include <cstring>
#include <fstream>
using namespace std;
struct Fruit_t
{
	char name[20];
	double price;
};
int GetInteger()
{
	char buf[100] = { 0 };
	while (strlen(buf) == 0) // 输入直接回车
		cin.getline(buf, 100);
	return atoi(buf);
}
double GetDouble()
{
	char buf[100] = { 0 };
	while (strlen(buf) == 0) // 输入直接回车
		cin.getline(buf, 100);
	return atof(buf);
}
// 从配置文件读入所有水果名称和单价
bool GetFruits(Fruit_t*& fruits, int& fruit_number);
void DealOrder(Fruit_t* fruits, int fruit_number);
struct LanguageMessages_t
{
	char menu[5][50];
	char fruit_file_error[50];
	char your_choice[30];
	char submenu[3][50];
	char submenu_left[5];
	char submenu_right[5];
	char submenu_buy[20];
	char submenu_payoff[20];
	char submenu_cancel[20];
	char submenu_price_unit[10];
	char submenu_weight_unit[10];
	char submenu_total_price[30];
	char submenu_input_weight[30];
	char submenu_have_bought[30];
} language;
void ShowMenu()
{
	for (int i = 0; i < 5; i++)
		cout << language.menu[i] << endl;
}
bool LoadLanguage(const char* file)
{
	ifstream fin(file);
	if (!file)
		return false;
	for (int i = 0; i < 5; i++)
		fin.getline(language.menu[i], 50);
	fin.getline(language.fruit_file_error, 50);
	fin.getline(language.your_choice, 30);
	for (int i = 0; i < 3; i++)
		fin.getline(language.submenu[i], 50);
	fin.getline(language.submenu_left, 5);
	fin.getline(language.submenu_right, 5);
	fin.getline(language.submenu_buy, 20);
	fin.getline(language.submenu_payoff, 20);
	fin.getline(language.submenu_cancel, 20);
	fin.getline(language.submenu_price_unit, 10);
	fin.getline(language.submenu_weight_unit, 10);
	fin.getline(language.submenu_total_price, 30);
	fin.getline(language.submenu_input_weight, 30);
	fin.getline(language.submenu_have_bought, 30);
	fin.close();
	return true;
}
int main(int argc, char* argv[])
{
	const char* file_lang = "Chs.txt";
	if (argc >= 2) // 指定语言文件
		file_lang = argv[1];
	if (!LoadLanguage(file_lang))
	{
		cout << "Language file error!" << endl;
		return 0;
	}
	Fruit_t* fruits = 0;
	int fruit_number;
	if (!GetFruits(fruits, fruit_number))
	{
		cout << language.fruit_file_error << endl;
		return 0;
	}
	while (1)
	{
		ShowMenu();
		cout << language.your_choice;
		int input = GetInteger();
		switch (input)
		{
		case 1:
			DealOrder(fruits, fruit_number);
			break;
		case 2:
			delete[]fruits;
			return 0;
		}
	}
}
void ShowSubMenu(Fruit_t* fruits, int fruit_number)
{
	cout << language.submenu[0] << endl;
	cout << language.submenu[1] << endl;
	for (int i = 0; i < fruit_number; i++)
	{
		cout << language.submenu_left << setw(2) << i + 1 << ". " << language.submenu_buy;
		cout << fruits[i].name << "(" << fixed << setw(5) << setprecision(2)
			<< fruits[i].price << language.submenu_price_unit << '/'
			<< language.submenu_weight_unit << ")";
		int len = strlen(language.submenu[0]) - strlen(language.submenu_left) - 4 - strlen(language.submenu_buy)
			- strlen(fruits[i].name) - 6 - strlen(language.submenu_price_unit) - 1
			- strlen(language.submenu_weight_unit) - 1 - strlen(language.submenu_right);
		for (int j = 0; j < len; j++)
			cout << ' ';
		cout << language.submenu_right << endl;
	}
	cout << language.submenu_left << setw(2) << fruit_number + 1 << ". "
		<< language.submenu_payoff;
	int len = strlen(language.submenu[0]) - strlen(language.submenu_left) - 4
		- strlen(language.submenu_payoff) - strlen(language.submenu_right);
	for (int j = 0; j < len; j++)
		cout << ' ';
	cout << language.submenu_right << endl;
	cout << language.submenu_left << setw(2) << fruit_number + 2 << ". "
		<< language.submenu_cancel;
	len = strlen(language.submenu[0]) - strlen(language.submenu_left) - 4
		- strlen(language.submenu_cancel) - strlen(language.submenu_right);
	for (int j = 0; j < len; j++)
		cout << ' ';
	cout << language.submenu_right << endl;
	cout << language.submenu[2] << endl;
}
{
	double* weight = new double[fruit_number];
	for (int i = 0; i < fruit_number; i++)
		weight[i] = 0;
	double sum = 0;
	while (1)
	{
		ShowSubMenu(fruits, fruit_number);
		cout << language.submenu_total_price << fixed << setprecision(2) << setw(8) << sum
			<< language.submenu_price_unit << endl;
		cout << language.your_choice;
		int input = GetInteger();
		if (input > 0 && input <= fruit_number)
		{
			cout << language.submenu_input_weight;
			double w = GetDouble();
			sum += fruits[input - 1].price * w;
			weight[input - 1] += w;
		}
		else if (input == fruit_number + 1)
		{
			if (sum > 1E-6) // 确实买了水果
			{
				cout << language.submenu_have_bought;
				for (int i = 0; i < fruit_number; i++)
					if (fabs(weight[i]) > 1E-6)
						cout << weight[i] << language.submenu_weight_unit << fruits[i].name << ", ";
				cout << language.submenu_total_price << sum << language.submenu_price_unit << endl;
				system("pause");
			}
			break;
		}
		else if (input == fruit_number + 2)
			break;
	}
	delete[]weight;
	system("cls");
}
bool GetFruits(Fruit_t*& fruits, int& fruit_number)
{
	ifstream fin("fruits.txt");
	if (!fin)
		return false;
	fin >> fruit_number;
	fin >> ws;
	fruits = new Fruit_t[fruit_number];
	for (int i = 0; i < fruit_number; i++)
	{
		char buffer[100];
		fin.getline(buffer, 100);
		char* p = strchr(buffer, '=');
		*p = '\0';
		strcpy(fruits[i].name, buffer);
		fruits[i].price = atof(p + 1);
	}
	fin.close();
	return true;
}
```