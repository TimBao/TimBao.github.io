---
layout: post
title: "More Effective C++读书笔记12"
date: 2013-01-11 11:17:00 
comments: true
categories: [C++技术]
tags: [C++技术]
description: "More Effective C++读书笔记12"
keywords: C++技术
---


 
  Item 12：理解“抛出一个异常”与“传递一个参数”或“调用一个虚函数”间的差异
 
 
  
   你调用函数时，程序的控制权最终还会返回到函数的调用处，但是当你抛出一个异常时，控制权永远不会回到抛出异常的地方。
   
    把一个对象传递给函数或一个对象调用虚拟函数与把一个对象做为异常抛出，这之间有三个主要区别。
    
     第一、异常对象在传递时总被进行拷贝；当通过传值方式捕获时，异常对象被拷贝了两次。对象做为参数传递给函数时不一定需要被拷贝。第二、对象做为异常被抛出与做为参数传递给函数相比，前者类型转换比后者要少（前者只有两种转换形式）。
     
      
       最后一点，catch 子句进行异常类型匹配的顺序是它们在源代码中出现的顺序，第一个类型匹配成功的 catch 将被用来执行。当一个对象调用一个虚拟函数时，被选择的函数位于与对象类型匹配最佳的类里，即使该类不是在源代码的最前头。
      
      #include <iostream>;
using namespace std;

class Base
{
public:
    Base() { cout << "constructor" << endl; }
    ~Base()  { cout << "destructor" << endl; }

    Base(const Base& m) { cout << "copy constructor" << endl; pInt = m.pInt; }
    void Message() {cout << "Base::Message" <<endl;}
private:
    int pInt;
};

class SubClass: public Base
{
public:
    SubClass() { cout << "constructor_sub" << endl; }
    ~SubClass(){ cout << "destructor_sub" << endl; }
    SubClass(const SubClass& ) { cout << "copy constructor_sub" << endl; }

    void Message() {cout << "SubClass::Message" <<endl;}
private:

};

int main() {
    int iTemp = 0;
    try
    {
        throw iTemp;
    }
    catch (double d)
    {
        cout << "double" << endl;
    }
    catch (int i)
    {
        cout << "int" << endl;
    }

    int* piTemp = NULL;
    try
    {
        throw piTemp;
    }
    catch (void* e)
    {
        cout << "void*" << endl;
    }
    catch (int* i)
    {
        cout << "int* " << endl;
    }

    SubClass n;
    try
    {
        throw n;
    }
    catch (Base& e)
    {
        e.Message();
        //throw;
        //throw e;
    }
    catch (SubClass& ex)
    {
        e.Message();
    }

    Base m;
    //Base* m = new Base（）；
    try
    {
        throw m;
    }
    catch (Base e)
    {
        cout << "Base" << endl;
    }
//    catch (Base& e)
//    {
//        cout << "Base&" << endl;
//    }
//    catch (const Base& e)
//    {
//        cout << "const Base&" << endl;
//    }
//    catch (Base* e)
//    {
//        e->;Message();
//        delete e;
//    }
}
      
       第一部分throw iTemp;输出：cout << "int" << endl; 对应上面的第二种情况，异常传递的类型如果是基础类型的话不能进行隐式转换。
       
        第二部分throw piTemp;输出：cout << "void*" << endl;对应上面的第二种情况，说明如果是异常抛出指针的话，catch参数可以被隐式转换为void*指针。
        
         第三部分throw n;输出：
         
          constructor
          
           constructor_sub
           
            constructor
            
             copy constructor_sub
             
              Base::Message
              
               destructor_sub
               
                destructor
                
                 对应上面的第二种情况，说明在异常捕获中，子类异常可以通过基类参数捕获到。
                 
                  对应上面的第三种情况，说明捕获的顺序是代码中出现的顺序。
                  
                   把注释掉的//throw; //throw e;两句分别打开，运行结果确是一样的，跟书中描述不尽相同。理论上讲throw应该抛出的时subclass类型的异常。
                   
                    
                     最后一部分主要是验证异常参数传值，传引用和传指针的区别。通过程序结果可以看出，不论是传值还是传引用，都会进行拷贝构造，不同之处为传值会进行两次拷贝构造。不过在传指针时，确没有进行拷贝构造函数的调用。和函数参数穿指针一样。但是需要注意的是，异常处理有可能会跟异常抛出位置不是同一个作用域，那样抛指针异常的时候，有可能会造成异常泄露，所以我在最后显示调用delete e；释放资源。
                     
                      
                      
                     
                    
                   
                  
                 
                
               
              
             
            
           
          
         
        
       
      
     
    
   
  
 


