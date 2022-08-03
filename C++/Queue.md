## 基本操作

使用公共的头文件：#include <quene>

STL队列应用：(先进先出)

- back() 返回最后一个元素
- empty() 如果队列为空则返回真
- front() 返回第一个元素
- pop() 删除第一个元素
- push() 在末尾添加一个元素
- size() 返回队列中元素的个数

## 顺序队列(quene)

一般，只有实现迭代器才是STL，quene,list不是STL

```cpp
//QueneSTL.h
#pragma once
#include "Comm.h"
void visit(int e){
   cout<<e<<" ";
}
typedef quene<int> QI;
class QueneSTL{      
    private:
      quene<int> iQ;
    public:
    //0 1 1   0 1 2 1   0 1 3 3 1   0 1 4
    //[1 1  0] [1 1   0 1] [1 1  0 1 2] [1 1  0 1 2 1]
    void printYanghui(int n){//使用队列打印杨辉三角
        int first,last,coe=0;//设置队首，队尾，系数
        int cur,next;//当前行号，下一行行数
        QI qi;
        qi.push(1);qi.push(1);
        for(cur=1;cur<=n;cur++){//有n行
            qi.push(0);//0作为该行起始值(人为添加)
            for(next=1;next<=cur+2;next++){//每一行的个数(+2人为添加的0)
            first=qi.front();
            qi.pop();//处理初始队首的操作(剔除人为添加的0)
            last=first+coe;
            qi.push(last);
            coe=first;
           if(next != cur+2)
            cout<<coe<<" ";
            }
             cout<<endl;
        }
    }
     testQuene(){
     for(int i=0;i<5;i++){
     iQ.push(rand()%100);
  }
         showQuene(iQ);
}
    showQuene(quene<int> q){//不能使用下标法及迭代器的方法
     int e;
        while(!q.empty()){//使用判空的方法,结果会造成队列的清空
            e=q.front();
            visit(e);cout<<",";
            q.pop();
        }
        cout<<endl;
    }
};
```

```cpp
//Comm.h
#include <stdlib.h>
#include <iostream>
#include <fstream>
#include <sstream>
#include <vector>
#include <queue>
using namespace std;
```

```cpp
#include "QueneSTL.h"
int main(){
   QueueSTL q_st1;
   q_str1.testQuene();
   q_str1.printYanghui(3);
   return 0;
}
```

## 双端队列(deque)

```cpp
typedef deque<int> DQI
class QueueSTL{
  public:
  void visit(int e){
   cout<<e<<" ";
}
    testDeque(){
      DQI dqi;
      for(int i=0;i<5;i++){
          dqi.push_back(20+i);//队尾进入元素
          dqi.push_front(2*i);//队首进入元素
      }//双向进入元素
       showDQI(dqi);//仅进行读操作，故可以实现(队列与vector可以进行传参操作)
    }
    void showDQI(DQI dq){
        DQI::iterator it;
        cout<<"[";
        for(it=dq.begin();it!=dq.end();++it){
            visit(*it);cout<<",";
            (it!=dq.end()-1) ? cout <<",":cout<<"]";
        }
        cout<<endl;
    }//迭代器访问
    /*下标法遍历
    cout<<"[";
    for(int i=0;i<dq.size();i++){
     visit(dq.at(i)); //at返回的是该位置上元素的引用
     i!=dq.size()-1 ?cout<<",":cout<<"]";
    }
    cout<<endl;
    */
};
```

```cpp
//for_each 在实现迭代器的基础上可以使用 注意引入新的头文件
#include <algorithm>
void showDQ(DQI dq){
    for_each(dq.begin(),dq.end(),visit);//遍历的范围是[begin,end) 取出的结果(即*it)直接传递给另一个函数，不必带上实参
}
```

## 优先队列(Priority Quene)

priority_quene<Type,Container,Functional>

Type:数据类型

Container:容器类型(必须是用数组实现的容器)

Functional:比较方式。

需要使用自定义数据类型时传入这三个参数

使用基本数据类型时，只需要传入数据类型，默认是大根堆。

> 优先队列添加了内部的排序属性，让优先级高的排列在队列的前面，优先出队(本身是堆实现的)

```cpp
#include <Person.h>
#include <PersonCmp.h>
void showPriorQuene(priority_quene<int> q){
    int e;
    cout<<"[";
    while(!q.empty()){
        e=q.top();//优先队列top函数相当于front函数
        visit(e);
        cout<<",";
        q.pop();
    }
    cout<<"]";
    cout<<endl;
}
void testPriorityQuene(){
   priority_quene<int> pqi;
    //等同于priority_quene<int,vector<int>,less<int> > pqi,缺省构造;
    //priority_quene<int,vector<int>,greater<int> > pqi2;//显式定义为小根堆。从小到大
    //如果自定义比较方式，需要传参保持一致
    for(int i=0;i<5;i++){
        pqi.push(rand()%100);//默认大根堆，从大到小
    }
    showPriorQuene(pqi);
}
void test2(){
    Person pn[3]={Person(),Person("Tom1",24),Person("Tom2",17)};
    priority_quene<Person> pqObj;//使用对象类型，容器的类型呼应数据类型，缺省构造
    priority_quene<Person,vector<Person>,greater<Person> > pqObj2;//显式构造
    priority_quene<Person,vector<Person>,PersonCmp > pqObj3;//包装类
    for(int i=0;i<3;i++){
        pqObj.push(pn[i]);
        pqObj2.push(pn[i]);
        pqObj3.push(pn[i]);
    }
    while(!pqObj.empty()){
        Person e=pqObj.top();
        cout<<e<<",";
        pqObj.pop();
    }
      while(!pqObj2.empty()){
        Person e=pqObj2.top();
        cout<<e<<",";
        pqObj2.pop();
    }
      while(!pqObj3.empty()){
        Person e=pqObj3.top();
        cout<<e<<",";
        pqObj3.pop();
    }
}
```

```cpp
//使用特殊的数据类型：以对象person为例
 bool operator<(const Person p1,const Person p2){
    return p1.age<p2.age;
}//重载小于运算符，默认的方式less排序
 bool operator>(const Person p1,const Person p2){
    return p1.age>p2.age;
}//重载大于运算符，采用自定义的方式greater排序
```

```cpp
//包装类,采用自定义的排序方式
//PersonCmp.h
#pragma once //等价于条件编译(仅编译一次)
#include "Person.h"
class PersonCmp{
    public:
      bool operator()>(const Person & p1,const Person & p2){
          return p1.age>p2.age;
      }
};
//与在类里面直接进行运算符的重载的区别在于，只有在调用这个封装类的时候，重载之后的运算符才会起作用。
```

