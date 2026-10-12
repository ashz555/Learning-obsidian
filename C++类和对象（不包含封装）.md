# 继承
## 基本语法：class 子类：继承方式 父类
可以减少重复代码
子类也叫派生类
父类也叫基类
## 继承方式：公共继承，保护继承，私有继承
```cpp
class Base{
		public: int a;
		protected : int b;
		private: int c;
}；
class Son :.....Base
{
};
```
如果是公共继承,则public：int a ；protected ：int b ；int c不可访问
如果是保护继承,则protected: int a , int b; int c不可访问
如果是私有继承,则private int a, int b; int c不可访问
注意：父类的私有成员被隐藏了，但会一直继承下去
## 继承中构造和析构顺序
在创建子类对象时，会先调用父类的构造函数，再调用子类的构造函数。
析构时，按先进后出原则，先调用子类的析构函数，再调用父类的析构函数
## （静态）同名成员处理
直接调用的是子类成员
通过子类对象(s)访问父类成员:s.Base::a
如果子类中出现和父类同名的成员函数，子类的同名函数会隐藏父类中所有同名函数，如果要访问父类的函数，同样要加作用域
另外：静态成员可直接通过类名访问
eg：Son::func(), Base::func(),Son::Base::func() //第一个双引号指通过类名访问，第二个指在Base作用域下的func函数
## 多继承(实际不实用)
语法:Class 子类:继承方式 父类1， 继承方式 父类2...
当父类中出现同名成员时，需要加作用域区分
##  菱形继承
```cpp
class animal {
public:
	int m_a;
};
class Sheep : public animal {};
class Tuo : public animal {};
class SheepTuo : public Sheep, public Tuo {};
int main()
{
	SheepTuo st;
	return 0;
}
```
继承了2份，导致资源浪费，同时st.m_a不明确。
要解决的话,要用虚继承
```cpp
class animal {     //虚基类
public:
	int m_a;
};
class Sheep : virtual public animal {};
class Tuo : virtual public animal {};
class SheepTuo : public Sheep, public Tuo {};
int main()
{
	SheepTuo st;
	return 0;
}
```
# 多态
## 基本概念
```cpp
class animal {
public:
	void speak() {
		cout << "Animal speaks" << endl;
	}
};
class Cat :public animal {
public:
	void speak(){
		cout << "Cat speaks" << endl;
	}
};
void test(animal& animal) {
	animal.speak();
}
int main()
{
	Cat cat;
	test(cat);
}
```

这段代码，我们想让animal的各种子类发出自己的声音，但现在输出结果是Animal speaks，不是我们想要的。
为了解决这个问题，引入虚函数。
```cpp
class animal {
public:
	virtual void speak() {
		cout << "Animal speaks" << endl;
	}
};
class Cat :public animal {
public:
	void speak(){    //此处子类中virtual可写可不写
		cout << "Cat speaks" << endl;
	}
};
void test(animal& animal) {    //动态多态一定要引用或指针
	animal.speak();
}
int main()
{
	Cat cat;
	test(cat);
}
```

## 原理剖析

#暂时不理解
## 纯虚函数和抽象类
```cpp
class AbstractCalculator {
public:
	virtual int getResult() {   //可改为纯虚函数：virtual int getResult()=0;
		return 0;
	}
	int a, b;
};
class addCalcualtor : public AbstractCalculator {
	virtual int getResult() {
		return a + b;
	}
};
int test01() {
	AbstractCalculator *abc = new addCalcualtor;
	abc->a = 10;
	abc->b = 10;
	return abc->getResult();
}
int main()
{
	cout<<test01();
}
```
在多态中，通常父类中虚函数的实现是毫无意义的，因为我们一般调用的是子类中的函数，因此我们可以把父类的虚函数写成纯虚函数，这个类就变成了抽象类。
注意：抽象类无法实例化对象，子类也必须重写纯虚函数，否则子类也会变成抽象类
## 虚析构和纯虚析构
多态使用时，如果子类中有属性开辟到堆区，父类指针无法调用子类的析构函数，为了解决这个问题，可以把父类的析构函数写为虚析构或纯虚析构
写法：virtual ~类名() {}
       virtual ~类名() =0;
和虚函数不同的是，虚析构还会调用父类的析构函数，虚函数不会调用父类的成员函数。
又有一个问题是，当我们写出一个纯虚析构时，如果父类也有属性开辟到堆区，析构函数里却没有功能实现，因此我们需要在类外写出它的功能实现。
在类外：```
```cpp
Base::~Base(){
cout<<"Base的纯虚析构函数"<<endl;
};
```

# 文件操作
## 写文件
程序运行时产生的是临时数据，有时我们需要记录在文件中，就需要文件操作
```cpp
#include <iostream>
#include<string>
#include<fstream>  //包含头文件
using namespace std;
int main()
{
	ofstream ofs;   //创建流对象
	ofs.open("test.txt", ios::out); //文件名+打开方式
	ofs << "Hello World!" << endl;
	ofs.close();  //关闭文件
}

```
## 读文件
共有四种方法,下面给出三种,会用一个就行
```cpp
#include <iostream>
#include<string>
#include<fstream>
using namespace std;
int main()
{
	ifstream ifs;
	ifs.open("test.txt", ios::in);
	string buf;
	while (getline(ifs, buf))
	{
		cout << buf << endl;
	}
}

```
```cpp
int main()
{
	ifstream ifs;
	ifs.open("test.txt", ios::in);
	char buf[1024] = { 0 };
	while(ifs >>buf)
	{
		cout << buf << endl;
	}
}

```
```cpp
int main()
{
	ifstream ifs;
	ifs.open("test.txt", ios::in);
	char buf[1024] = { 0 };
	while(ifs.getline(buf,sizeof(buf)))
	{
		cout << buf << endl;
	}
}

```
## 二进制文件
```cpp
#include <iostream>
#include<string>
#include<fstream>
using namespace std;
class Person{
public:
	char name[20];
	int age;
};
int main()
{
	Person p = { "张三",20 };
	ofstream ofs("person.txt", ios::out | ios::binary);
	ofs.write((const char*) &p, sizeof(Person));	
	ofs.close();
	ifstream ifs("person.txt", ios::in | ios::binary);
	if(!ifs.is_open())
	{
		cout << "文件打开失败" << endl;
		return -1; //main函数异常退出
	}
	ifs.read((char*)&p, sizeof(Person));
	cout << "姓名：" << p.name << endl;
	cout << "年龄：" << p.age << endl;
}
```

