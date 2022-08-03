# 泛型编程

```cpp
//Node.h
template <typename T>
class Node{
    public:
      T data;
      Node * next;
    //构造函数
      Node():next(NULL){}
      Node(T e):data(e),next(NULL){}
      void show(){cout<<data;}
}
void testNode(){
    Node<int> i_node(2020);
    i_node.show();
    cout<<endl;
    Node<string> s_node("Hello");
    s_node.show();
    cout<<endl;
    Node<string> *ps_node;
    ps_node=new Node<string>("hello,CTGU");
    ps_node->show();
    cout<<endl;
    delete ps_node;
}
```

```cpp
//链表模板类头文件
//LinkList.h
#include "Node.h"
template <typename T>
class LinkList{
    Node<T> *head;
    Node<T> *tail;
    int length;
    public:
         LinkList();
         LinkList(int);
         void show();
};
  void testLinkList(){
      LinkList<int> list;
      list.show;
  }
```

```cpp
//链表类成员函数的实现文件
//LinkList.cpp
#include "LinkList.h"
template <typename T>
LinkList<T>::LinkList(){
    head=new Node<T>[n];
    tail=new Node<T>[n];
    length=0;
}
template <typename T>
LinkList<T>::LinkList(int n){
    head=new Node<T>;
    tail=new Node<T>;
    length=n;
}
template <typename T>
void LinkList<T>::show(){
    cout<<length;
}
```

```cpp
#include "Node.h"
int main(){
   void (*fn_arr[])()={testNode,testLinkList};
    fn_arr[0]();//即testNode
    fn_arr[0](1);
    return 0;
}
```

