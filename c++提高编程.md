# 模板
## 函数模板
```
写法：
template<typename T> //typename可换成class
函数声明或定义 

使用函数模板：
1.自动类型推导:swap（a,b)    //a,b数据类型要一样
2.显示指定类型:swap<int>(a,b)

注意：模板必须确定出T的数据类型
反例：
template<class T>
void func(){}
当我们调用func()时就会报错
```
同时T只能代表一个类型
```cpp
void swap(T &a, T &b)
{
	T temp = a;
	a = b;
	b = temp;
}
int main()
{
	int a;
	char c;
	swap<int, char>(a, c);
}
```
这个程序是错误的
## 类模板


# STL（标准模板库）
STL从广义上分为：容器，算法，迭代器
STL大体分为六大组件：容器，算法，迭代器，仿函数，适配器（配接器），空间配置器
## vector容器
可以把vector视为数组模板，迭代器视为指针
```cpp
#include<vector>
#include<algorithm>
void test()
{
	vector<int>v;
	v.pushback(1); //输入
	v.pushback(2);
	v.pushback(3);
	for(vector<int>::iterator it=v.begin();!v.end();it++) //遍历方法
	{
		cout<<*it<<endl;
	}
	void print(int val)
	{
	cout<<val<<endl;
	}
	for_each(v.begin(),v.end(),print); //用遍历算法
}

```
## 案例
```cpp
#include <iostream>
#include<vector>
using namespace std;
void test() {
	vector<vector<int>>v;
	vector<int>v1;
	vector<int>v2;
	vector<int>v3;
	for (int i = 0; i < 3; i++) {
		v1.push_back(i);
		v2.push_back(i + 3);
		v3.push_back(i + 6);
	}
		v.push_back(v1);
		v.push_back(v2);
		v.push_back(v3);
		for (vector<vector<int>>::iterator it = v.begin(); it != v.end(); it++) { //*it is a pointer to a vector<int>
		for (vector<int>::iterator vit = (*it).begin(); vit != (*it).end(); vit++)         {
			cout << *vit;
		}
		cout << endl;
	}
}
int main() {
	test();
	return 0;
}
```