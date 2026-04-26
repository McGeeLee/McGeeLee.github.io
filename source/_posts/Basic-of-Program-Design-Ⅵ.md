---
title: Basic of Program Design Ⅵ
password: ''
date: 2024-11-21 13:26:06
tags:  
  - Learning-Notes
  - Tsinghua
categories: "Project"
---
### 文本数据处理
#### 第七周“统计记录总数”的参考程序
```cpp
#include <iostream>
#include <fstream>           // 包含文件操作的头文件
using namespace std;
int main() {
	ifstream fin("log.txt"); // ifstream表示input file stream
	int count = 0;           // 记录数，初始值为0
	while (!fin.eof()) {     // eof表示end of file
		int year, month, day, hour, minute, second;
		char tmp, id[20], operation[10];// 获取文本文件中的一行
		fin >> year >> tmp >> month >> tmp >> day; // 2015/4/21
		fin >> hour >> tmp >> minute >> tmp >> second; // 11:16:16
		fin >> id;        // 40dbae14f777cdd
		fin >> operation; // LOGIN
		count++;          // 读到一行，记录数加1
	} fin.close();        // 关闭文件
	cout << count << endl;// 输出记录数
	return 0;
}
```
#### 第七周“统计活跃用户”的参考程序
```cpp
#include <iostream>
#include <fstream> // 包含文件操作的头文件
#include <cstring> // ADDED in 2.0, 字符串操作头文件
using namespace std;
int main() {
	ifstream fin("log.txt"); // ifstream表示input file stream
	int count = 0; // 记录数，初始值为0
	char ids[4500][20]; // ADDED in 2.0, 记录所有的编号
	while (!fin.eof()) {    // eof表示end of file
		int year, month, day, hour, minute, second;
		char tmp, id[20], operation[10];
		// 获取文本文件中的一行
		fin >> year >> tmp >> month >> tmp >> day; // 2015/4/21
		fin >> hour >> tmp >> minute >> tmp >> second; // 11:16:16
		fin >> id; // 40dbae14f777cdd
		fin >> operation; // LOGIN
		strcpy(ids[count], id); // ADDED in 2.0, 将本行的编号拷贝到编号数组中
		count++; // 读到一行，记录数加1
	} fin.close(); // 关闭文件
	int user_count = 0; // ADDED in 2.0, 用户数
	for (int i = 0; i < count; i++) {
		// ADDED in 2.0, 线性查找, 在第i项之前找与i相同的项
		int found = -1;
		for (int j = 0; j < i; j++)
			if (strcmp(ids[i], ids[j]) == 0) {
				found = j;
				break;
			}
		// ADDED in 2.0, 如果没找到，说明是新用户，用户记数加1
		if (found == -1)
			user_count++;
	}
	cout << user_count << endl; // ADDED in 2.0, 输出用户数
	return 0;
}
```
#### 第七周“统计活跃用户（省空间）”参考程序
```cpp
#include <iostream>
#include <fstream> // 包含文件操作的头文件
#include <cstring> // 字符串操作头文件
using namespace std;
int main() {
	ifstream fin("log.txt"); // ifstream表示input file stream
	int user_count = 0; // ADDED in 2.1, 用户数
	char ids[600][20]; // ADDED in 2.1, 记录所有的编号
	while (!fin.eof()) {// eof表示end of file
		int year, month, day, hour, minute, second;
		char tmp, id[20], operation[10];
		// 获取文本文件中的一行
		fin >> year >> tmp >> month >> tmp >> day; // 2015/4/21
		fin >> hour >> tmp >> minute >> tmp >> second; // 11:16:16
		fin >> id; // 40dbae14f777cdd
		fin >> operation; // LOGIN
		// ADDED in 2.1, 线性查找, 在活跃用户数组找与当前项一样的编号
		int found = -1;
		for (int i = 0; i < user_count; i++)
			if (strcmp(id, ids[i]) == 0) {
				found = i;
				break;
			}
		// ADDED in 2.1, 如果没找到，说明是新用户，用户记数加1
		if (found == -1) {
			strcpy(ids[user_count], id);
			user_count++;
		}
	}
	fin.close(); // 关闭文件
	cout << user_count << endl; // 输出用户数
	return 0;
}
```
#### 第七周“统计在线时长”的参考程序
```cpp
#include <iostream>
#include <fstream> // 包含文件操作的头文件
#include <cstring> // 字符串操作头文件
using namespace std;
// ADDED in 3.0, 时间类型的结构定义
struct Time_t {
	int year, month, day;
	int hour, minute, second;
};
// ADDED in 3.0, 计算时长的函数声明
int TimeDifference(Time_t, Time_t);
int main() {
	ifstream fin("log.txt"); // ifstream表示input file stream
	int user_count = 0; // 用户数，初始值为0
	char ids[600][20]; // 记录所有的编号
	bool online[600]; // ADDED in 3.0, 在线状态
	Time_t last_on[600]; // ADDED in 3.0, 上次登录时间
	int secs[600]; // ADDED in 3.0, 累计在线秒数
	while (!fin.eof()) {// eof表示end of file
		Time_t t;
		char tmp, id[20], operation[10];
		// ADDED in 3.0, 获取文本文件中的一行（结构版本）
		fin >> t.year >> tmp >> t.month >> tmp >> t.day; // 2015/4/21
		fin >> t.hour >> tmp >> t.minute >> tmp >> t.second;// 11:16:16
		fin >> id; // 40dbae14f777cdd
		fin >> operation; // LOGIN
		// 线性查找
		int found = -1;
		for (int i = 0; i < user_count; i++)
			if (strcmp(id, ids[i]) == 0) {
				found = i;
				break;
			}
		// 如果没找到，说明是新用户，记录
		if (found == -1) {
			strcpy(ids[user_count], id);
			// ADDED in 3.0, 记录新用户的登陆状态
			if (strcmp(operation, "LOGIN") == 0) {
				online[user_count] = true;
				last_on[user_count] = t;
			}
			else
				online[user_count] = false;
			secs[user_count] = 0;
			user_count++;
		}
		// ADDED in 3.0, 否则，修改登录状态，计算登录时长
		else {
			if (strcmp(operation, "LOGIN") == 0) {
				if (!online[found]) {
					online[found] = true;
					last_on[found] = t;
				}
			}
			else {
				if (online[found]) {
					online[found] = false;
					// 调用函数，计算两个时刻之差
					secs[found] += TimeDifference(last_on[found], t);
				}
			}
		}
	}
	fin.close(); // 关闭文件
	// ADDED in 3.0, 输出每个用户的在线时长
	for (int i = 0; i < user_count; i++)
		cout << ids[i] << " " << secs[i] << endl;
	return 0;
}
// ADDED in 3.0, 计算时间差，已知只会出现2015年内的时间
int TimeDifference(Time_t s, Time_t t) {
	int days[12] = { 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 };
	int day_count = t.day - s.day;
	for (int i = s.month; i < t.month; i++)
		day_count += days[i - 1];
	int result = day_count * 60 * 60 * 24;
	result += (t.hour - s.hour) * 60 * 60;
	result += (t.minute - s.minute) * 60;
	result += (t.second - s.second);
	return result;
}
```
#### 第七周“统计在线时长（写文件）”参考程序
```cpp
#include <iostream>
#include <fstream> // 包含文件操作的头文件
#include <cstring> // 字符串操作头文件
using namespace std;
// ADDED in 3.0, 时间类型的结构定义
struct Time_t {
	int year, month, day;
	int hour, minute, second;
};
// ADDED in 3.0, 计算时长的函数声明
int TimeDifference(Time_t, Time_t);
int main() {
	ifstream fin("log.txt"); // ifstream表示input file stream
	int user_count = 0; // 用户数，初始值为0
	char ids[600][20]; // 记录所有的编号
	bool online[600]; // ADDED in 3.0, 在线状态
	Time_t last_on[600]; // ADDED in 3.0, 上次登录时间
	int secs[600]; // ADDED in 3.0, 累计在线秒数
	while (!fin.eof()) {// eof表示end of file
		Time_t t;
		char tmp, id[20], operation[10];
		// ADDED in 3.0, 获取文本文件中的一行（结构版本）
		fin >> t.year >> tmp >> t.month >> tmp >> t.day; // 2015/4/21
		fin >> t.hour >> tmp >> t.minute >> tmp >> t.second;// 11:16:16
		fin >> id; // 40dbae14f777cdd
		fin >> operation; // LOGIN
		// 线性查找
		int found = -1;
		for (int i = 0; i < user_count; i++)
			if (strcmp(id, ids[i]) == 0) {
				found = i;
				break;
			}
		// 如果没找到，说明是新用户，记录
		if (found == -1) {
			strcpy(ids[user_count], id);
			// ADDED in 3.0, 记录新用户的登陆状态
			if (strcmp(operation, "LOGIN") == 0) {
				online[user_count] = true;
				last_on[user_count] = t;
			}
			else
				online[user_count] = false;
			secs[user_count] = 0;
			user_count++;
		}
		// ADDED in 3.0, 否则，修改登录状态，计算登录时长
		else {
			if (strcmp(operation, "LOGIN") == 0) {
				if (!online[found]) {
					online[found] = true;
					last_on[found] = t;
				}
			}
			else {
				if (online[found]) {
					online[found] = false;
					// 调用函数，计算两个时刻之差
					secs[found] += TimeDifference(last_on[found], t);
				}
			}
		}
	}
	fin.close(); // 关闭文件
	// ADDED in 3.0, 输出每个用户的在线时长
	for (int i = 0; i < user_count; i++)
		cout << ids[i] << " " << secs[i] << endl;
	return 0;
}
// ADDED in 3.0, 计算时间差，已知只会出现2015年内的时间
int TimeDifference(Time_t s, Time_t t) {
	int days[12] = { 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31 };
	int day_count = t.day - s.day;
	for (int i = s.month; i < t.month; i++)
		day_count += days[i - 1];
	int result = day_count * 60 * 60 * 24;
	result += (t.hour - s.hour) * 60 * 60;
	result += (t.minute - s.minute) * 60;
	result += (t.second - s.second);
	return result;
}
```

### 非文本数据处理 Hash算法 二进制文件
#### 第八周1
```cpp
#include <iostream>
#include <fstream>
#include <cstring>
using namespace std;
struct Time_t {
	int year, month, day;
	int hour, minute, second;
};
struct Node {
	Time_t tm;
	char id[20];
	char op[10];
	Node* next;
};
int Hash(char* id) {
	int res = 0;
	for (int i = 0; id[i]; i++) {
		res = (res + id[i] * 7) % 256;
	}
	return res;
}
void Insert(Node* hash_tab[], Node elem) {
	int idx = Hash(elem.id);
	Node* data = new Node;
	*data = elem;
	data->next = hash_tab[idx];
	hash_tab[idx] = data;
}
void Print(Node* list) {
	Node* now = list;
	while (now != NULL) {
		cout << now->id << " " << now->op << endl;
		now = now->next;
	}
}
void Output(Node* hash_tab[]) {
	for (int i = 0; i < 256; i++) {
		if (hash_tab[i] == NULL) continue;
		cout << i << ": ";
		Print(hash_tab[i]);
		cout << endl << endl;
	}
}
void Delete(Node* list) {
	while (list) {
		Node* tmp = list;
		list = list->next;
		delete tmp;
	}
}
void Release(Node* hash_tab[]) {
	for (int i = 0; i < 256; i++) {
		if (hash_tab[i] == NULL) {
			continue;
		}
		Delete(hash_tab[i]);
		hash_tab[i] = NULL;
	}
}
int main() {
	Node* list_tab[256] = { NULL }; //初始化: 每个元素均为空指针, 不指向任何变量
	ifstream fin("log.txt");
	while (!fin.eof()) {
		char tmp;
		Node data;
		fin >> data.tm.year >> tmp >> data.tm.month >> tmp >> data.tm.day;
		fin >> data.tm.hour >> tmp >> data.tm.minute >> tmp >> data.tm.second;
		fin >> data.id;
		fin >> data.op;
		Insert(list_tab, data);
	}
	fin.close();
	Output(list_tab);
	Release(list_tab);
	return 0;
}
```
#### 第八周2
```cpp
#include <iostream>
#include <fstream>
#include <cstring>
using namespace std;
struct Time_t {
	int year, month, day;
	int hour, minute, second;
};
struct Log_info {
	Time_t tm;
	char id[20];
	char op[10];
};
struct Node {
	Log_info log;
	Node* next;
};
int Hash(char* id) {
	int res = 0;
	for (int i = 0; id[i]; i++) {
		res = (res + id[i] * 7) % 256;
	}
	return res;
}
void Insert(Node* hash_tab[], Node elem) {
	int idx = Hash(elem.log.id);
	Node* data = new Node;
	*data = elem;
	data->next = hash_tab[idx];
	hash_tab[idx] = data;
}
void Print(Node* list) {
	Node* now = list;
	while (now != NULL) {
		cout << now->log.id << " " << now->log.op << endl;
		now = now->next;
	}
}
void Output(Node* hash_tab[]) {
	for (int i = 0; i < 256; i++) {
		if (hash_tab[i] == NULL) continue;
		cout << i << ": ";
		Print(hash_tab[i]);
		cout << endl << endl;
	}
}
void Delete(Node* list) {
	while (list) {
		Node* tmp = list;
		list = list->next;
		delete tmp;
	}
}
void Release(Node* hash_tab[]) {
	for (int i = 0; i < 256; i++) {
		if (hash_tab[i] == NULL) {
			continue;
		}
		Delete(hash_tab[i]);
		hash_tab[i] = NULL;
	}
}
void SaveHashTab(Node* hash_tab[], const char* filename) {
	ofstream fout(filename, ios::binary);
	for (int i = 0; i < 256; i++) {
		Node* p = hash_tab[i];
		while (p) {
			fout.write((char*)&(p->log), sizeof(p->log));
			p = p->next;
		}
	}
	fout.close();
}
void LoadHashTab(Node* hash_tab[], const char* filename) {
	ifstream fin(filename, ios::binary);
	while (fin) {
		Node data;
		fin.read((char*)&(data.log), sizeof(data.log));
		if (fin.eof()) {
			break;
		}
		Insert(hash_tab, data);
	}
	fin.close();
}
int main() {
	Node* list_tab[256] = { NULL };
	ifstream fin("log.txt");
	while (!fin.eof()) {
		char tmp;
		Node data;
		fin >> data.log.tm.year >> tmp >> data.log.tm.month >> tmp >> data.log.tm.day;
		fin >> data.log.tm.hour >> tmp >> data.log.tm.minute >> tmp >> data.log.tm.second;
		fin >> data.log.id;
		fin >> data.log.op;
		Insert(list_tab, data);
	}
	fin.close();
	Output(list_tab);
	SaveHashTab(list_tab, "list.tab");
	Release(list_tab);
	LoadHashTab(list_tab, "list.tab");
	Output(list_tab);
	Release(list_tab);
	return 0;
}
```