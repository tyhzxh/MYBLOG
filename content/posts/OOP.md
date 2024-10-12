+++
title = 'OOP'
date = 2024-03-25T18:48:36+08:00
draft = true
+++

今天复习一下OOP（全是代码，之后再改成文章）

构造，析构，继承，多态，友元，重载

## 构造函数和析构函数

```c++

#include <iostream>

using namespace std;

class Animal
{
    public:
        Animal()
        {
            cout << "Animal构造函数" << endl;
        }
        ~Animal()
        {
            cout << "Animal析构函数" << endl;
        }
};

class Cat : public Animal
{
    public:
        Cat()
        {
            cout << "Cat构造函数" << endl;
        }
        ~Cat()
        {
            cout << "Cat析构函数" << endl;
        }
};

int main(){
    Animal *a = new Cat();
    delete a;
    return 0;
}
```

## 继承

```c++

#include <iostream>
using namespace std;

class Animal{
    public:
        Animal(){
            cout << "Animal构造函数" << endl;   
        }
        ~Animal(){
            cout << "Animal析构函数" << endl;
        }
};

class Cat : public Animal{
    public:
        Cat(){
            cout << "Cat构造函数" << endl;
        }
        ~Cat(){
            cout << "Cat析构函数" << endl;
        }
};

class dog : public Animal{
    public:
        dog(){
            cout << "dog构造函数" << endl;
        }
        ~dog(){
            cout << "dog析构函数" << endl;
        }
}

class test : public Cat, public dog{//继承
    public:
        test(){
            cout << "test构造函数" << endl;
        }
        ~test(){
            cout << "test析构函数" << endl;
        }
}
//共有继承

```
