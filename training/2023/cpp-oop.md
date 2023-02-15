# C++ 面向对象程序设计进阶

## 补充

https://www.runoob.com/cplusplus/cpp-tutorial.html

## 类

### 引入
结构体：对同一事物的属性的封装。

```cpp
struct Student {
    string name{};
    int age{};
    Gender gender{};
};

void Greet(const Student &student) {
    cout << "Hello! My name is " << student.name << ". I am " << student.age << " years old." << endl;
    cout << "Nice to meet you!!!" << endl;
}
```
进一步地，我们可以将这一事物相关的函数也封装到结构体内。
```cpp
struct Student {
    string name{};
    int age{};
    Gender gender{};

    void Greet() {
        cout << "Hello! My name is " << name << ". I am " << age << " years old." << endl;
        cout << "Nice to meet you!!!" << endl;
    }
};
```
现在，他不仅具有了一个Person的所有属性，还可以干一个Person应该能干的事情，实现了对现实中的“人”真正的完全的**抽象**！

起个新名字吧。**类**

```cpp
class Student {
public:
    string name{};
    int age{};
    Gender gender{};

    void Greet() {
        cout << "Hello! My name is " << name << ". I am " << age << " years old." << endl;
        cout << "Nice to meet you!!!" << endl;
    }
};
```

### 类的定义
```cpp
class 类名{
    // 成员变量
    // 成员函数
};
```
简单来说，类是属性和方法的集合，属性是类中的数据，方法就是调用这些数据进行操作的函数。

**三大特性：封装、继承、多态**

### 类的两种定义方式

#### 内联定义
注意，成员函数如果放在类内定义，编译器会将其当作**内联函数**进行处理。
```cpp
class Student {
public:
    string name{};
    int age{};
    Gender gender{};

    void Greet() {
        cout << "Hello! My name is " << name << ". I am " << age << " years old." << endl;
        cout << "Nice to meet you!!!" << endl;
    }
};
```

#### 拆分定义
**最常用**
```cpp
// student.h
class Student {
public:
    string name{};
    int age{};
    Gender gender{};

    void Greet();
};

// student.cpp
#include "student.h"

void Student::Greet(){
    cout << "Hello! My name is " << name << ". I am " << age << " years old." << endl;
        cout << "Nice to meet you!!!" << endl;
}
```

### 访问限定符
- 用于修饰成员，限定成员的访问范围。
- public
  - 公有
  - 可以在类外部被直接访问。
- private
  - 私有
  - 只能在类的内部被访问。
- protected
  - 保护
  - 只能在类的内部或者派生类中被访问。

```cpp
class Student {
public:
    void Greet(){
        cout << "Hello! My name is " << name << ". I am " << age << " years old." << endl;
        cout << "Nice to meet you!!!" << endl;
    }
    void SetInfo(string name,int age,Gender gender){
        this->name_ = name;
        this->age_ = age;
        this->gender_ = gender;
    }
private:
    string name_{};
    int age_{};
    Gender gender_{};
};
```
#### 注意事项
- 访问限定符的作用域是从该访问限定符出现的位置到下一个访问限定符为止。
- class的默认访问权限是private，struct的默认访问权限是public。
- 访问限定符限定的是外部访问，保护私有的不希望被直接访问的成员。

### 封装

封装的本质是一种管理手段，将属性（数据）和方法（接口）有机结合起来，再隐藏他们，只对外公开接口（函数）来和对象交互。不想给人看的，使用private/protected进行修饰，而公开一些共有函数提供对成员的合理访问。

```cpp
struct Student {
    string name{};
    int age{};
    Gender gender{};
};

int main() {
    Student studentMkbk{};
    studentMkbk.name = "MKBK";
    studentMkbk.gender = MALE;
    studentMkbk.age = 21;
    
    Student studentKy{};
    studentKy.name = "KY";
    studentKy.gender = FEMALE;
    studentKy.age = -1;             // ????????
}
```

这里也能说明，为什么C语言不适合写大型项目，因为它不支持面向对象，管理能力较弱，不是函数就是数据，且默认访问是public的，安全性较差。

```cpp
class Student {
public:
    void SetInfo(const string& name_,const int age_,const Gender gender_){
        assert(age_ > 0);
        
        name = name_;
        age = age_;
        gender = gender_;
    }
    void ChangeName(const string& name_){
        name = name_;
    }
    void GrowUp(){
        age++;
    }
    string GetName(){return name;}
    int GetAge(){return age;}
    Gender GetGender(){return gender;}
private:
    string name{};
    int age{};
    Gender gender{};
};

int main() {
    Student studentMkbk{};
    studentMkbk.SetInfo("MKBK",21,MALE);
    studentMkbk.GrowUp();
}
```
### 类的实例化
类可以理解为一个房屋的图纸，这个图纸规定了一些基本信息，比如说这个房子朝向是什么、有几扇窗户、房子材料由什么构成、几室几厅等……但是图纸终归是图纸，纸上谈兵永远不会成为现实，要把这个房子现实化，就需要根据这个图纸的规定，修建出相应的房子。

所以说根据图纸（类）修建房子（对象）的过程，称为类的实例化。就像前面的Student一样，我们的类只是规定了具有name、age、gender这些属性，拥有Greet方法，这样的Student还只是一个概念。而只有将这个概念，通过赋予具体的数据，才能让他成为一个真正存在Person——即占有实际空间。

一个图纸不可能只造出一个房子，而是可以造出千千万万个房子，这些诸多房子的本源只有一个，就是那张图纸。而这些房子尽管都来自于同一张图纸，但是根据使用者的不同，比如房子内部有不同的装饰，环境卫生不一样，这些房子之间还是会存在区别的。

一个类可以实例化出众多对象，这些对象由于内部存储的数据不同，相互之间存在差别。

```cpp
Student studentMkbk{};
studentMkbk.SetInfo("MKBK",21,MALE);
studentMkbk.GrowUp();
```

程序员在写代码的过程中，其实就是从众多对象中总结出共同点，从对象**抽象**出类，这些是写代码非常看中的能力。

### 浅谈面向对象

需求：我想去洗个澡

面向过程：

```
我打开浴室门()
我进入浴室()
我打开水龙头()
我洗澡()
我擦干身体()
```

面向对象：
万物皆对象，你是对象，浴室是对象，水龙头是对象，毛巾也是对象
```
浴室.打开()
我.前往(浴室)
水龙头.打开()
我.洗澡()
毛巾.吸水()
```

- 面向过程->雕版印刷
- 面向对象->活字印刷

## 构造函数与析构函数

### 引入

#### 构造函数

根据图纸建造完房子后，会先打扫房里内部，把房子清理干净，放上一些自带的家具，然后才会住进人。

类实例化完成一个对象后，就会马上调用构造函数，对对象的属性进行初始化，并进行一些准备工作。

#### 析构函数

当房子准备被拆除时，会先请出房子内住的人，然后清空房子内部，最后才会真正拆除。

在准备释放一个对象之前，最后会执行对象的析构函数，对对象的工作进行收尾工作，释放占用的资源。

### 构造函数

编译器自动生成一个不带参数的默认构造函数，来对对象进行初始化。但默认构造函数的工作并不一定能满足我们的需求，因此我们也可以重写默认构造函数，或者自己写带参构造函数。

```cpp
class Student {
public:
    Student(){
        name = "NoName";
        age = 0;
        gender = MALE;

        cout << "Hello~ I don't have a name. This is my first time here.";
    }

    Student(const string& name_, const int age_,const Gender gender_){
        name = name_;
        age = age_;
        gender = gender_;

        cout << "Hello~ I am "<< name<<". This is my first time here.";
    }
private:
    string name{};
    int age{};
    Gender gender{};
};

int main() {
    Student student1{};
    Student student2{"Hil",20,MALE};
}
```

### 析构函数
只能有一个，不能含有参数。由系统自动调用。

```cpp
class Student {
public:
    ~Student(){
        cout << "Good bye!";
    }
private:
    string name{};
    int age{};
    Gender gender{};
};
```

## 继承和多态

### 继承

#### 引入

- Food
  - Fruit
    - Apple
    - Banana
    - Pear
  - Vegetable
    - Turnip(萝卜)
    - Tomato
    - Potato
  - Meat
    - Beef
    - Pork

```cpp
class Food {
public:
    int Quality{};
    int Volume{};
};

class Fruit : public Food {
public:
    int Sweetness{};
    int MoistureContent{};
};

class Apple : public Fruit {
public:
    int Color{};
};

class Banana : public Fruit {
public:
    int Aging{};
};

class Vegetable : public Food {
public:
    int Type{};
};

class Meat : public Food {
public:
    int Smell{};
};

int main() {
    Apple apple{};
    apple.Color = 0;
    apple.Sweetness = 100;
    apple.Quality = 50;
}
```

#### 用途

使用现有类的所有功能，并在无需重新编写原来的类的情况下对这些功能进行扩展。

通过继承创建的新类称为 “子类” 或“派生类”。

被继承的类称为 “基类”、“父类” 或“超类”。

继承的过程，就是从一般到特殊的过程。

在考虑使用继承时，有一点需要注意，那就是两个类之间的关系应该是 “属于” 关系。例如，Employee 是一个人，Manager 也是一个人，因此这两个类都可以继承 Person 类。但是 Leg 类却不能继承 Person 类，因为腿并不是一个人。

### 多态

子类对父类同名函数的覆写。调用父类成员函数时，会根据调用函数的对象的类型来执行不同的函数。

```cpp
class Food {
public:
    virtual void Eat(){
        cout << "Eating...";
    }
};

class Fruit : public Food {
public:
    void Eat(){
        cout << "Eating... Sweet Fruit!";
    }
};

class Apple : public Fruit {
public:
    void Eat(){
        cout << "Eating... Sweet Fruit! Apples today are delicious!";
    }
};

class Banana : public Fruit {
public:
};

void EatSomething(Food* food){
    food->Eat();
}

int main() {
    Apple apple{};
    Banana banana{};

    EatSomething(&apple);
    cout << endl;
    EatSomething(&banana);
}
```

## this 指针

每一个对象都能通过 this 指针来访问自己的地址。

```cpp
class Person{
    void SetName(string name){
        this->name = name;
    }
private:
    string name{};
};
```

## 静态成员

### 引入

```cpp
class Person{
public:
    static int Count;
    static void War(){
        Person::Count = 0;
    }
    Person(){
        Count++;
    }
};

int Person::Count = 0;

int main() {
    Person p1{};
    Person p2{};
    Person p3{};
    cout << Person::Count<< endl;

    Person::War();
    cout << Person::Count<< endl;
}
```

### 概念
static声明为静态的，称为静态成员。 

不管这个类创建了多少个对象，静态成员只有一个拷贝，这个拷贝被所有属于这个类的对象共享。

静态成员无法访问非静态成员。

静态成员属于类而不属于对象。