# **C++：namespace命名空间详解**

**命名空间的主要用途：解决命名冲突。** 

```cpp
#include <iostream>;
using namespace std;

// 定义两个全局变量var01、var02
int var01 = 1; 
int var02 = 2;

// 定义命名空间A
namespace A
{
    int var01 = 10;
    int var02 = 20;
}

定义命名空间B
namespace B
{
    int var01 = 100;
    int var02 = 200;
}

void test01(void)
{
    cout<<"全局变量var01 = "<<var01<<endl; // 打印结果为：全局变量var01 = 1
    cout<<"全局变量var02 = "<<::var02<<endl; // 打印结果为：全局变量var02 = 2

    cout<<"A::var01 = "<<A::var01<<endl; // 打印结果为：A::var01 = 10
    cout<<"B::var02 = "<<B::var02<<endl; // 打印结果为：B::var02 = 200
}

int main()
{
    test01();
    return 0;
}
```

说明：上述程序在全局命名空间(也称为默认命名空间)、A命名空间、B命名空间中分别定义了变量var01、var02，通过运行说明，不同命名空间的变量互不干扰，从而成功避免了变量名冲突问题。

**命名空间下可以存放变量、函数、结构体、类...**

```cpp
#include <iostream>;
using namespace std;

namespace A
{
	int x = 1;
    // 在A命名空间下定义函数func1
	void func1()
	{
		cout<<"namespace A::func1"<<endl;
	}
    // 在A命名空间下声明函数func2
	void func2();
}

int main()
{
	cout<<"A::x = "<<A::x<<endl; // 打印结果：A::x = 1
	A::func1(); // 调用A空间中函数func1，打印结果：namespace A::func1
    A::func2(); // 调用A空间中函数func2，打印结果：namespace A::func2
    return 0;
}

void A::func2()
{
    cout<<"namespace A::func2"<<endl;
}
```

**命名空间须声明在全局作用域下**

**命名空间可以嵌套命名空间，使用A::B::y**

```cpp
namespace A
{
    int var01 = 1;
    // 在命名空间A下再声明命名空间B
	namespace B
	{
		int var02 = 4;
	 }
}

int main()
{
    cout<<"A::var01 = "<<A::var01<<endl;
    cout<<"A::B::var02 = "<<A::B::var02<<endl;
    return 0;
}
```

**命名空间是开放的，可以随时将新成员添加到命名空间下。**

```cpp
#include <iostream>;
using namespace std;

// 定义命名空间A
namespace A
{
    int var01 = 10;
    int var02 = 20;
}

定义命名空间B
namespace B
{
    int var01 = 100;
    int var02 = 200;
}

// 在A命名空间中追加新变量var03.此时A空间中包含三个变量var01、var02、var03
namespace A
{
    int var03 = 30;
}

int main()
{
    cout<<"A::var01 = "<<A::var01<<endl; // 打印结果为：A::var01 = 10
    cout<<"B::var02 = "<<B::var02<<endl; // 打印结果为：B::var02 = 200
    cout<<"A::var03 = "<<A::var03<<endl; // 打印结果为：A::var03 = 30
    return 0;
}
```

**命名空间可以是匿名的，此时该匿名命名空间实际也是作用于默认命名空间中的，只是相当于对该名空间下的标识符添加了static修饰词，使其仅作用于本文件。**

```cpp
// 注：该程序是错误的。匿名命名空间依旧作用于默认命名空间，因此变量var01重复定义了。

#include <iostream>;
using namespace std;

int var01 = 1;
namespace
{
	int var01 = 10;
}

int main()
{
	cout<<"::var01 = "<<var01<<endl;
	cout<<"::var01 = "<<var01<<endl;
}
```

**命名空间可以取别名 namespace new_A = A**

```cpp
#include <iostream>;
using namespace std;
// 定义的命名空间名过长，使用不方便。
namespace ABCDEFGHIJKDFJKDFJIDDJFJDKJDKJDKFJKDJDKFEJDKDJFI
{
	int var01 = 10;
}

int main()
{   
    // 对命名空间进行重命名
    namespace A = ABCDEFGHIJKDFJKDFJIDDJFJDKJDKJDKFJKDJDKFEJDKDJFI;
	cout<<"ABCDEFGHIJKDFJKDFJIDDJFJDKJDKJDKFJKDJDKFEJDKDJFI::var01 = "<<A::var01<<endl;
}
```

