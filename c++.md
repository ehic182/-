# VisualStudio

## 一.编译器基本原理

1.解决方案配置：控制文件如何被编译

   解决方案平台：编译代码的目标平台 x86 32位运行平台 x64 64位运行平台

2.项目中每个cpp文件都会被编译，但头文件不会被编译（因为头文件的内容在预处理的时候包含进来了：以#开头的的东西被称为预处理命令），头文件的代码被复制粘贴到 cpp文件里并在cpp文件被编译时一起编译了

3.cpp文件编译被编译成.obj文件（object file 目标文件），多个.obj通过链接合并成一个可执行的.exe程序

4.c开头的错误编码表示在编译阶段出错，l开头表示在链接阶段出错

5.c++头文件省去多次函数声明

## 二.关键字

### 1.头文件防止多重定义

当多个函数要调用同一个头文件时，为避免函数重定义可以 #ifndef 头文件名  //检查是否有该头文件被定义

​												        #define 头文件名//如果没有则进行定义

​													

​													函数声明..........

​												        #endif//否则不再进行定义

### 2.输入输出

头文件 #include <iostrome>

   输出 std::cout<<hello world<< std::endl

   输入std::cin>>........(变量)

### 3.静态static

静态变量:static int s_variable 该变量只能在创建的.cpp文件里访问，其他地方不会访问

作用：可以防止命名相同变量时编译出错

类里的静态变量只声明不定义，定义要在类外定义

~~~c++
struct Entity{
    static int variable;//此处只是声明我有这个变量
};

int Entity:: variable//这才是定义
    
int main(){
Enitity e;//声明实例
    e.variable=1
}

~~~



静态变量static在类里创建的所有实例共享一个数据

.访问普通对象成员（成员函数，成员变量）如：cin.get

::访问类/命名空间的静态成员

#### 3.1静态中的单例

私有化实例，公开访问

~~~ c++
#include<iostream>

class PrivateExample {
private:
	PrivateExample() {};//实例私有化，禁止外部创建实例
public:                  //&取返回值地址，防止返回值是拷贝值
	static PrivateExample& Get() {//get()是函数名，外界只有通过这个函数访问唯一一个实例
		static PrivateExample message;
		return message;
	}//本身创建唯一一个实例

	int test() {
		std::cout << "hello" << std::endl;
		return 0;
	}
};

int main() {
	PrivateExample::Get().test();//拿到实例调用函数
}
~~~

## 三.类

c++里的类和c里的结构体很像，都表示一个实例所具有的特点：

1.函数的调用是主要区别，c里不能放函数，但c++可以，函数的调用也用.来实现，前提是有实例

2.类有构造函数，它是你每构造一个对象都会调用的函数，它可以初始化变量，避免后面每次创建新的实例都要重新初始化

定义：没有返回类型，且必须与类名相同

3.类里的静态变量归属类本身，不属于某一个对象不能读写类里的普通成员变量x,y

4.类里默认私有private，若要公共化要加前缀public

~~~ c++
class PrivateExample {
public:
	float x, y;
	PrivateExample(float X, float Y) {
		x = X;
		y = Y;
	}//构造函数，初始化变量
      static int print(PrivateExample message) {//参数是具体的成员变量，这样函数才能知道是哪个实例里的变量
		std::cout <<message.x << "," <<message.y << std::endl;
		return 0;
	}//静态函数
};

int main() {
	PrivateExample A(10, 9);
	A.print(A);
}
~~~

作用：用来初始化内存；

上面的是构造体内初始化

~~~c++
class Player{
public:
		std::string name;
    	int x,y,z;
    	
    	Player(int ax,int ay,int az):x(ax),y(ay),z(az){};//把括号里的值赋给括号外面的变量
}


~~~

### 3.1 析构函数

作用域：在程序结束时使用

写法：

~~~c++
~类名（）{//在构造函数的前面加上一个~
    
}
~~~

## 四.继承

1.子类完全继承父类里的变量和函数

## 三十.纯虚函数（接口）

为什么有这个定义：在父类里存在虚函数，这个虚函数时没有意义的，我们希望的是在子类里实现这个函数的具体功能，所以创建了这样				   一个没有主体的虚函数，叫纯虚函数也叫接口

~~~ c++
#include<iostream>
#include<string>
class Printable 
{
public:
	virtual std::string GetClassName() = 0;//纯虚函数,子类必须重写
};//总的父类

class Entity :public Printable
{
public:
	virtual std::string Try() = 0;//只要类里有一个纯虚数，这个类就变成抽象类（抽象类不能创建实体对象，只能用指针或引用）
	virtual std::string GetName() { return "Entity"; }//虚数
	std::string GetClassName()override{return "Entity";}
};

class Player:public Entity
{
private:
	std::string m_Name;
public:                             //name赋值m_Name
	Player(const std::string& name) :m_Name(name){}//构造函数，初始化基本量
	std::string Try()override { std::cout << "123" << std::endl; return 0; }
	std::string GetName()override { return m_Name; }//重写虚函数
	std::string GetClassName()override { return "Player"; }//重写纯虚函数
};							//override重写

class Son:public Player {
private:
	std::string name;
public:
	Son(std::string n_Name):Player(n_Name),name(n_Name){}//此处若为空，无参构造，但若父类为有参构造此处会报错
	std::string GetClassName()override { return "Player2"; }
};

void PrintName(Entity& entity) {//抽象类无法创建实例，必须要用引用或指针
	std::cout << entity.GetName() << std::endl;

}

void Print(Printable* entity) {
	std::cout << entity->GetClassName()<< std::endl;
}
int main() {
	Son* x = new Son("hf");
	Player* p = new Player("埃弗拉");
	std::cout << p->GetName()<< std::endl;
	Print(p);
	Print(x);
}
~~~

## 三一.可见性

概念：类的成员或方法有多可见，即谁能调用，谁能使用

~~~ c++
#include<iostream>
#include<string>
class Entity 
{
protected://只能被当前类或子类看到
	int z;
private://只能被当前类看到
	int x, y;
	void print() { std::cout << "123" << std::endl; }
public://谁都可以看到
	Entity(int ox,int oy):x(ox),y(oy){
		z = 1;
		std::cout << z << std::endl;
		print();
	}
};

class Player :public Entity
{
private:
	int a, b;
public:
	Player():Entity(a,b) {
		z = 2;
		std::cout << z << std::endl;
	}
};
int main() {
	Entity* a = new Entity(1, 2);
}
~~~

## 三二.数组

1.在栈上和在堆上创建的数组是不一样的，更推荐在栈上，访问速度更快(new出来是在堆上，直接创建是在栈上，堆上你拿到的是地址你还要访问地址才能知道数组值，栈上你输个下标直接就能知道值) 

![QQ20260921-193225](E:\照片\QQ20260921-193225.png)

2.std::array创建数组：它有边界查询，大小明确

~~~ c++
#include<iostream>
#include<string>
#include<array>
int main() {
	std::array<int, 20>another;
	for (int i = 0; i < another.size(); i++) {
		another[i] = i;
		std::cout << another[i] << std::endl;
	}
}
~~~

## 三三.字符串

""双引号用于字符串，''用于字符

~~~ c++
#include<iostream>

int main() {
	const char* name = "nsajd";
	char name2[5] = { 'h', 'e', 'l', 'l','o'};
	std::string name3 = "cherno";//+"hello"不能直接加
	std::cout << name2<< std::endl;
}
~~~

~~~ c++
#include<iostream>
					//std::string string
void PrintMessage(const std::string& string) {
	//string += "he"; 上面和这里都改后，会降低速度，本质改变的是拷贝值拷贝费时
	std::cout << string << std::endl;
}

int main() {
	const char* name = "cherno";
	std::string name2 = std::string("cherno") + "hello";
//std::string name2 = "cherno" + "hello";这里会退化成const char*类型的指针相加
	bool contains = name2.find("no") != std::string::npos;
}
~~~

## 三四.字符串字面量

