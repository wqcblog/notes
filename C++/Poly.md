# 抽象基类

```cpp
//abstract basic class抽象相关类的共性
//Worker.h 定义抽象基类 包含至少一个纯虚函数的类，该类不能实例化对象，但是可以实例化对象指针，纯虚函数在派生类中必须要实现(约束派生类必须实现某些纯虚函数，相当于接口(interpace)，否则派生类不能实例化)
#pragma once
#include<bits/stdc++.h>
abstract class Worker{
    private:
       string name;
       double salary;
       int rank;
    public:
       Worker():name("Tom"),pay(0),rank(0){}
       virtual void work(){
           cout<<"Worker is working!"<<endl;
       };//virtual function虚函数,需要在这里实现
       virtual double income() =0;//a pure virtual function 纯虚函数     
};//构造函数不可以作为虚函数，但是析构函数可以作为虚函数
   void testpoly_pointer(Worker *worker){
        worker->work();
   }
   void testpoly_ref(Worker & worker){
       worker.work();
   }
```

```cpp
//Teacher.h
#include "Worker.h"
class Teacher:virtual public Worker{
    public:
      void work();//派生出的不同类实现的结果不同
      double income();//这个函数必须实现
};
void Teacher::work(){
    cout<<"Teach online in this term"<<endl
}
double Teacher::income(){
    return 0
}
void testTeacher{
    Teacher teacher;
    teacher.work();
    teacher.income();
}
```

```cpp
//Admin.h
#include "Worker.h"
class Admin:virtual public Worker{
    public:
      void work();//派生出的不同类实现的结果不同
      double income();//这个函数必须实现
};
void Admin::work(){
    cout<<"Admin online in this term"<<endl
}
double Admin::income(){
    return 0
}
void testTeacher{
    Admin admin;
    admin.work();
    admin.income();
}
```

```cpp
#include "Teacher.h"
#include "Admin.h"
int main(){
    testWorker();
    Teacher tea;
    Admin admin;
    testpoly_pointer(&tea);
    testpoly_pointer(&admin);//体现多态：根据传入的子类对象确定调用的子类函数
    //方式1：指针的形参是基类指针 实参是不同子类的指针，父类的指针指向子类的对象
    testpoly_ref(&tea); //方式2：采用引用的形式实现
    testpoly_ref(&admin);
    //注意调用的函数必须为虚函数才可实现多态，否则调用的为基类的方法(不是多态)
}
```

> 赋值兼容规则：在公有派生的情况下，一个派生类的对象可用于基类对象适用的地方:(假定类derived由类base派生)，以下三种说明：

- （1）派生类的对象可以赋值给基类的对象    

```cpp
derived d; base b; b=d;
```

-  (2)派生类的对象可以初始化基类的引用

```
derived d; base& b=d;
```

- (3)派生类的对象的地址可以赋给指向基类的指针。

```
derived d; base*pb=&d;
```

