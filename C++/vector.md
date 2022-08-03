# STL(容器)之vector

## vector读写文件

**补充知识：**

C语言中的缓冲区机制：(区别分割符与回车符)

一般来说，输入的都会进入缓冲区，从缓冲区取走需要对应的标准才可以，从缓冲区取走的过程中未被处理的内容会被清空。对于每次输入后回车符的分析：scanf读取回车符会留在缓冲区，gets会取走回车符，但不会包含在读取的字符中，puts输出会自动添加回车换行符。

行缓冲：在输入和输出中遇到换行符时，执行真正的I/O操作。这时，我们输入的字符先存放在缓冲区，等按下回车键换行时才进行实际的I/O操作。比如对键盘输入数据。输入的回车符留在缓冲区。

全缓冲：按照设定的读取标准(参数)读取，当填满标准I/O缓存后(文件读取完毕)才进行实际I/O操作。比如对磁盘文件的读写。

换行符：\n (转义字符实现换行)

​                 cout<<endl()   endl控制符的作用是将光标移动到输出设备中下一行开头处，并且清空缓冲区）

​                **cout<<flush** ; // 将显存的内容立即输出到显示器上进行显示，即清空缓存区

### vector的相关知识

#### vector的初始化

vector<T> v1 :vector保存对象为T，初始化构造一个空的vector

vector<T> v2(v1):v2是v1的一个副本

vector<T> v3(n,i):v3包含n个值为i的元素

vector<T> v4(n):v4含有值初始化的n个副本

#### vector常见的操作方法

v.empty()  判断vector是否为空

v.push_back(t)  在v的末尾增加一个值为t的元素

v[n]返回位置为n的元素

v1=v2 把v1的元素替换为v2中元素的副本

v1==v2 判断v1和v2是否相等  (其它操作符同理) 

```cpp
//从文件中读取动态的数据
//vectorInt.h
#include <common.h>
  typedef vector<int> VI; //一维向量
  typedef vector<VI> VII;//二维向量
//类的封装,避免函数声明,一个类里面的函数不存在先后关系
class VectorInt{
    private:
      VII vii;//定义共用的成员变量 体现面向对象编程 保护私有变量 类的对象共用一个变量
      VI vi;
    public:
    inputVectorFromFile_v1(string filename){
    int e;
    while(ifs>>e){//不读取换行符，按照该输入标准连续读取整数
    vi.push_back(e);//在vector最后添加一个元素
    }
    traverseVector(vi);//读取打印的结果一连串
}
    void inputVectorFromFile_v2(string filename){
        int e;
        string dataline;
        ifstream ifs(fileName.c_str()){//定义文件输入流(类比cin输入)，c需要string转char*    
        for(;getline(ifs,dataline);;){//读一行数据
         istringstream iss(dataLine); //将读取的string封装为输入流对象
            //保存按行读取的内容，并输入到对象中(识别分隔符并自动转换类型)
            vi.clear();//清空一维数组的内容
        for(;iss>>e;){
            vi.push_back(e);
        }
            vvi.push_back(vi);
    }
    }//文件中的数据按行保存在二维向量中
    void traverseVector_v1(){
        //方法一：下标法,size()返回vector数量的多少
        cout<<"[";
        for(int i=0;i<vi.size();i++){
            cout<<vi[i];
            i!=vi.size-1?cout<<",":cout<<"]";
            
        }
        cout<<"]";
        cout<<endl; 
}
     void traverseVector_v2(){//遍历二维向量
          for(int i=0;i<vii.size();i++){
              traverseVector_v1(vii[i]);//传入一维向量
              cout<<endl;
      }         
}
    void vectorSort(){
         int i,j;
         for(i=0;i<vii.size()-1;i++){
             for(j=0;j<vii.size()-1-i;j++){
              if(vii[j].size()>vii[j+1].size){
                  vii[j].swap(vii[j+1]);
              }   
             }
         }
     } //二维向量的bubble sort(按照每一行的长度排序) 使用swap交换两个向量的值        
    voi outputVectorFromFile_v2(string filename){
         ofstream ofs(filename.c_str());//定义文件输出流
         for(int i=0;i<vii.size();i++){
             for(int j=0;j<vii[i].size();j++){
                 ofs<<vii[i][j];
                 ofs<<" ";
             }
             ofs<<endl;
         }
     }//输出至文件中
}
```

```cpp
//main.cpp
#include <common.h>
#include <vectorInt.h>
int main(int argc,char** argv){//此写法代表取得参数 argc是参数个数 argv去获取参数
   VectorInt vi;
   string filename;
   cin>>filename;
   vi.inputVectorFromFile_v2(filename);
   vi.traverseVector_v2();
   vi.vectorSort();
   vi.traverseVector_v2();
   vi.outputVectorFromFile_v2(filename);
}
```

```cpp
//common.h
#include <stdlib.h>
#include <iostream>
#include <fstream>//文件流
#include <sstream>//字符串输入输出流
#include <vector>
using namespace std;
```

```cpp
//迭代器的方法遍历
void traverseVector(const VI vi){
    VI::iterator it;//迭代器指针
    cout<<"[";
    for(it=vi.begin();it!=vi.end();it++){
        cout<<*it;
        it!=vi.end()-1?cout<<",":cout<<"]";
    }
    cout<<endl;
}
```

## vector读写文件对象

对于规范的数据，自定义类实现面向对象编程操作

```cpp
//Person.h
class Person{
    private:
      string name;
      int age;
      double high;
    public:
    void show(){
        cout<<"[name:"<<name<<",age:"<<age;
        cout<<",high"<<high<<"]"<<endl;
    }
    Person(const Person& person){
        cout<<"copy constructor called!\n";
        this->name=person.name;
        this->age=person.age;
        this->high=person.high;
    }
    Person(string name,int age,double hjgh){
        this->name=name;
        this->age=age;
        this->high=high;
    }   
    friend istream& operator>>(istream& in,Person &obj);
    friend ostream& operator>>(ostream& out,Person &obj);
    Person(){
        name="Tom";age=20;
    }
    Person(String n,int a){
        name=n;age=a;
    }//h
};
istream& operator>>(istream& in,Person &obj){//操作符重载(输入流)
     in>>obj.name>>obj.age>>obj.high;
    return in;
}
ostream& operator<<(ostream& out,Person &obj){//操作符重载(输出流)
     out<<obj.name<<obj.age<<obj.high;
    return out;
}
```

```cpp
//VectorObj.h
#include "Person.h"
#include "Comm.h"
class Vectorobj{//封装一个类
    typedef vector<Person> VP;
    typedef vector< <vector<Person> > VVP
    private:
       VP v_obj;//将对象封装到vector
       VVP vv_obj;//注意空格 避免出现>>(输入或者位移运算的歧义)
    public:
   void inputVectorFromFile(string fileName){
    Person obj;
    ifstream ifs(fileName.c_str());
    string dataLine;
    for(;getline(ifs,dataLine);){
        istringstream iss(dataLine);//流对象封装
        v_obj.clear();
        for(;iss>>obj;){//每一行取到的标准数据输入至对象
            v_obj.push_back(obj);
        }
        vv_obj.push_back(v_obj);//文件对象保存在vv_obj中
    } 
}
     void traverseVecObj_1()const{
       VP::iterator first=v_obj.begin(),last=v_obj.end();//定义迭代器指针
          for(;first!=last;++first)
              cout<<*first;//end指向最后一个元素的下一个位置，是一个空的位置
      }//访问一维向量
    //下标法：for(int i=0;i<vobj.size();i++){
    //cout<<vobj[i];vobj[i]是person类的对象 需要对输出运算符进行重载
    //}
    void traverseVecObj_2()const{
        VVP::iterator it=vv_obj.begin;
        for(;it!=vv_obj.end();it++){
            traverseVecObj_v1(*it);
            cout<<endl;
        }
    }//访问二维向量
    //for(int i=0;i<vv_obj.size();i++){
    // traverseVectorObj(vv_obj[i]); 
    void sortVectorObj_2{
        int i,j,k;
        for(i=0;i<vv_obj.size()-1;i++){
            k=i;//假定k是最小元素的下标
            for(j=i+1;j<<vv_obj.size();j++){
                if(vv_obj[k][0].age>vv_obj[j][0].age)//比较对象的元素大小(这里改p类的成员变量为public 暂时实现访问)
                    k=j;
            }
            if(k!=i){
            vv_obj[i].swap(vv_obj[k]);//系统提供的交换一维向量
        }
    }
    }
    void outputVectorFromFile(string filename){
        ofstream ofs(fileName.c_str());
        for(int i=0;i<vv_obj.size();i++){
            for(int j=0;j<vv_obj[i].size();j++){
            ofs<< vv_obj[i][j];
        }
            ofs<<endl;
    }//下标法输出至文件
}；
```



