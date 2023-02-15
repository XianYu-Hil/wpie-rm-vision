# C++ 语言特性进阶

## 运算符重载

### 引入

```cpp
class Point2D {
public:
    double x{};
    double y{};
};

void main()
{
    Point2D a{},b{};
    if(a>b){}
}
```

对于我们自定义的类，它的数据和含义都是由程序员设计的，编译器显然没办法预先知道该如何去处理与它有关的运算符。

包括但不限于 +、-、*、/、>、<、== 等一切你能想到的。

因此，我们需要通过一种方式，来定义与我们的类相关的运算符逻辑。

### 类内定义

通常用这种方式定义的操作符的相关操作数都是这个类。

省略一个操作数：`this`。

```cpp
class Point2D {
public:
    double x{};
    double y{};

    bool operator>(const Point2D &p) const {
        return sqrt(pow(this->x, 2) + pow(this->y, 2)) > sqrt(pow(p.x, 2) + pow(p.y, 2));
    }
};
```

```cpp
class Point2D {
public:
    double x{};
    double y{};

    bool operator>(const Point2D &p) const {
        return sqrt(pow(this->x, 2) + pow(this->y, 2)) > sqrt(pow(p.x, 2) + pow(p.y, 2));
    }

    bool operator==(const Point2D &p) const {
        return this->x == p.x && this->y == p.y;
    }

    Point2D operator++() {
        return Point2D{this->x++, this->y++};
    }

    Point2D operator+(const Point2D &p) const {
        return Point2D{this->x + p.x, this->y + p.y};
    }
};
```

### 类外定义

通常用这种方式定义的操作符的相关操作数不是同一个类。

```cpp
class Point2D {
public:
    double x{};
    double y{};
};

class Point3D {
public:
    double x{};
    double y{};
    double z{};
};

Point3D operator+(const Point3D &p_3d, const Point2D &p_2d) {
    return Point3D{p_3d.x + p_2d.x, p_3d.y + p_2d.y, p_3d.z};
}

bool operator>(const Point3D &p_3d, const Point2D &p_2d){
    return sqrt(pow(p_3d.x, 2) + pow(p_3d.x, 2) + pow(p_3d.z, 2)) > sqrt(pow(p_2d.x, 2) + pow(p_2d.y, 2));
}
```

## 模板

### 概念

模板是泛型编程的基础，泛型编程即以一种独立于任何特定类型的方式编写代码。

### 引入

起初，我们只需要返回int类型的最大值，没有人在意这个需求......

```cpp
int Max(int a, int b) {
    return a > b ? a : b;
}
```

直到后来......

```cpp
class Point2D {
public:
    double x{};
    double y{};

    bool operator>(const Point2D &p) const {
        return sqrt(pow(this->x, 2) + pow(this->y, 2)) > sqrt(pow(p.x, 2) + pow(p.y, 2));
    }
};

int Max(int a, int b) {
    return a > b ? a : b;
}

float Max(float a, float b) {
    return a > b ? a : b;
}

double Max(double a, double b) {
    return a > b ? a : b;
}

char Max(char a, char b) {
    return a > b ? a : b;
}

Point2D Max(Point2D a, Point2D b) {
    return a > b ? a : b;
}

int main() {
    int i = Max(1, 2);
    float f = Max(1.0f, 2.0f);
    double d = Max(1.0, 2.0);
    char c = Max((char) 1, (char) 2);
    Point2D p = Max({1, 2}, {2, 1});
}
```

???

**Tips**：当你同一段类似的代码需要在多个地方复制粘贴的时候，该考虑重构啦！

模板，横空出世。

```cpp
template<typename T>
T Max(T a, T b) {
    return a > b ? a : b;
}
```

### 声明
```cpp
template<模板列表>
主体(函数、类、结构体)
```

```cpp
template<typename T, typename R,typename Q>
Q Func(T a,R b){
    T value;
    R temp;
    Q sth;
    // ...
}
```

```cpp
template<typename T>
class Array{
public:
    T Get(int index){
        T value = data[index];
        return value;
    }
    T Insert(T value){
        data[0] = value;
    }
    T data[];
};
```

### 调用

#### 显式调用

在函数名/类名后加上方括号，直接指定模板列表的具体类型

```
int main()
{
    Func<int,float,double>(1,2.0f);
    Array<int> array{};
}
```

最通用最标准的调用方法，但是好像还……差点意思？

#### 隐式调用

如果模板列表中的模板类型全部都用于了函数参数，那么就可以让编译器根据我们传入的参数类型自动推断模板类型。

```cpp
template<typename T>
T Max(T a, T b) {
    return a > b ? a : b;
}

template<typename T>
class Array{
public:
    Array(T t){
        data[0] = t;
    }
    T data[10];
};

int main()
{
    auto i = Max(1,2);
    Array array{'c'};
}
```

**注意**：模板参数确定类型时要求全部都要完成推断，也就是说如果有任何一个模板参数没办法通过函数参数列表推断出类型，就老老实实显式调用吧。

### 编译期求值

```cpp
template<typename T>
class Array{
private:
    int length_{};
public:
    Array(int length){
        length_ = length;
    }
    T data[length_];
};
```

```cpp
template<typename T,int LENGTH>
class Array{
private:
    int length_ = LENGTH;
public:
    Array()= default;
    T data[LENGTH];
};

int main()
{
    Array<double,10> array{};
}
```

类的生命周期：(编译期)模板列表实例化 -> (运行期) 成员默认初始化 -> 构造函数 -> ......

模板列表确定值的过程发生在编译期，在运行期之前。因此可以实现一些无法在运行期确定值的功能。

## 类型推断

模板、auto、decltype()

### auto

### decltype

### C++化验结果出来了，全是auto，没有一点类型

语言的发展趋势必然是越来越智能的，由编译器来代替我们大脑的部分思考过程，让我们可以更加专注在算法上。

以下例子仅在C++20达到完全体

```cpp
template<typename T, typename R, typename Q>
Q Add(T a, R b) {
    return a + b;
}
```

```cpp
auto Add(auto a, auto b) {
    return a + b;
}
```

## 多线程

### 引入

某咸鱼：获取图片 -> 识别 -> 解算 -> 预测 -> 通信 -> 获取图片 -> ......

---

某咸鱼：获取图片 -> 获取图片 -> ......

mkbk：识别 -> 识别 -> ......

月月：解算 -> 预测 -> 解算 -> 预测 -> ......

高哥：通信 -> 通信 -> ......

---

```cpp
int main() {
    cout << "process" <<endl;
    this_thread::sleep_for(std::chrono::seconds(2));
    cout << "detect" << endl;
    this_thread::sleep_for(std::chrono::seconds(2));
    cout << "serial" << endl;
    this_thread::sleep_for(std::chrono::seconds(2));
    cout << "Bye" << endl;
}
```

```cpp
void process(){
    while(true){
        cout << "process" <<endl;
        this_thread::sleep_for(std::chrono::seconds(2));
    }
}
void detect(){
    while(true){
        cout << "detect" << endl;
        this_thread::sleep_for(std::chrono::seconds(2));
    }
}
void serial(){
    while(true){
        cout << "serial" << endl;
        this_thread::sleep_for(std::chrono::seconds(2));
    }
}
int main() {
    thread process_t{process};
    thread detect_t{detect};
    thread serial_t{serial};

    process_t.join();
    detect_t.join();
    serial_t.join();
}
```

### 概念

#### 线程
线程是操作系统能够进行运算调度的最小单位，它被包含在进程之中，进程包含一个或者多个线程。进程可以理解为完成一件事的完整解决方案，而线程可以理解为这个解决方案中的的一个步骤，可能这个解决方案就这只有一个步骤，也可能这个解决方案有多个步骤.

#### 多线程
多线程就是把执行一件事情的完整步骤拆分为多个子步骤，然后使得这多个步骤同时执行.

#### C++中的多线程

C++ 官方提供了完整的且非常便于使用的多线程库。位于头文件 `thread` 中。

简单情况下，C++ 多线程使用多个函数实现各自功能，然后将不同函数生成不同线程，并同时执行这些线程（不同线程可能存在一定程度的执行先后顺序，但总体上可以看做同时执行）。

### 创建线程

```cpp
void Func1(){
    while(true);
}

void Func2(int i){
    while(true);
}

int main() {
    thread func1_t{Func1};
    thread func2_t{Func2,10};

    func1_t.join();
    func2_t.join();
}
```
#### thread.join()

当前线程暂停，直至所有子线程执行结束。

你把工作委托给mkbk和月月进行，接着你直接开摆，直到他们完成你委托的工作，你才会继续你的工作。

### 互斥量

#### 引入

```cpp
vector<int> v{1, 2, 3, 4, 5, 6, 7, 8, 9, 0};

void Add() {
    while (true)
        v.push_back(10);
}

void Sub() {
    while (true)
        v.pop_back();
}

int main() {
    thread add_t1{Add};
    thread sub_t1{Sub};
    thread add_t2{Add};
    thread sub_t2{Sub};
    thread add_t3{Add};
    thread sub_t3{Sub};

    add_t1.join();
    sub_t1.join();
    add_t2.join();
    sub_t2.join();
    add_t3.join();
    sub_t3.join();
}
```

你正在愉快地调着唯一一辆C车，突然一个人过来，二话不说就直接开始调你的C车，并且还修改了你的参数。

当多个线程**同时**对同一个变量进行操作时，就会导致程序崩溃。

一行语句编译为汇编时，往往会拆分为多个**原子操作**，看似是一行代码，在执行时其实也是有步骤的。为了确保操作的确定性，对原子操作的插入访问是非法的。

```
p++;
---
q = p;
q++;
p = q;
```

正确的解决方式应该是：此时规定不管是谁，在调C车之前都要向队长申请许可证(lock)，用完再向队长归还许可证(unlock)。许可证总共只有一个，没有许可证的人就等着在调C车的队友用完后才能申请许可证(阻塞，线程1 lock 互斥量后其他线程就无法 lock, 只能等线程1 unlock 后，其他线程才能 lock)。那么，这个许可证就是互斥量。互斥量保证了调C车这一过程不被打断。

#### 使用

```cpp
mutex m;
int a = 0;

void Add()
{
    while(true){
        m.lock();
        cout << "Adding" << endl;
        cout << "before:" << a << endl;
        cout << "now:" << ++a << endl;
        m.unlock();
    }

}

void Sub()
{
    while(true){
        m.lock();
        cout << "Subing" << endl;
        cout << "before:" << a << endl;
        cout << "now:" << --a << endl;
        m.unlock();
    }

}
int main()
{
    thread proc1(Add);
    thread proc2(Sub);
    proc1.join();
    proc2.join();
    return 0;
}
```

为了确保性能，互斥量的lock范围需要尽可能地小，当对同一个变量的操作结束后需尽快unlock，以避免不必要的线程阻塞。

互斥量lock后必须进行unlock，如果忘记unlock，将会导致其它线程阻塞。

#### 智能锁 unique_lock

为了解决忘记unlock或者无法unlock的问题(线程异常中止)，我们可以使用unique_lock。它在实例化时会拥有一个锁，并自动将其上锁，当离开它的作用域后，将会自动解锁。

```cpp
mutex m;
int a = 0;

void Add()
{
    while(true){
        unique_lock<mutex> locker{m};
        cout << "Adding" << endl;
        cout << "before:" << a << endl;
        cout << "now:" << ++a << endl;

    }

}

void Sub()
{
    while(true){
        unique_lock<mutex> locker{m};
        cout << "Subing" << endl;
        cout << "before:" << a << endl;
        cout << "now:" << --a << endl;
    }

}
int main()
{
    thread proc1(Add);
    thread proc2(Sub);
    proc1.join();
    proc2.join();
    return 0;
}
```

### 条件变量

#### 引入

某咸鱼：初始化相机 -> 获取图像 -> 获取图像 -> 获取图像 -> ......
mkbk：等待相机初始化 -> 识别 -> 识别 -> 识别 -> ......

相机线程在获取图像之前，需要先完成一次相机的初始化，才能够获取图像。而识别线程，也就需要在识别前先等待相机线程完成相机初始化后，才能够开始工作。

```cpp
bool isFinishInit = false;

void process() {
    cout << "Initing Camera......" << endl;
    this_thread::sleep_for(chrono::seconds(3));
    
    cout << "Start process" << endl;
    isFinishInit = true;

    while (true) {
        cout << "Process a picture!" << endl;
    }
}

void detect() {
    while(!isFinishInit);
    
    cout << "Start detect" << endl;
    while(true){
        cout << "Detect a picture!" << endl;
    }
}

int main() {
    thread proc1(process);
    thread proc2(detect);
    proc1.join();
    proc2.join();
    return 0;
}
```

通过while卡死的方式尽管可行，但对变量的大量访问，会导致CPU资源的浪费。

在等待你的队友完成前期准备工作的时候，你不断地询问“好了没？”“好了没？”“好了没？”“好了没？”“好了没？”“好了没？”“好了没？”“好了没？”“好了没？”“好了没？”“好了没？”，显然并不合适。我们可以等待队友完成前期准备工作后让他通知我们，我们才开始工作。使用事件通知机制来代替反复的轮询。

#### 使用
```cpp
bool isFinishInit = false;
mutex m{};
condition_variable condition{};

void process() {
    cout << "Initing Camera......" << endl;
    this_thread::sleep_for(chrono::seconds(3));

    cout << "Start process" << endl;

    unique_lock<mutex> locker{m};
    isFinishInit = true;
    condition.notify_all();
    locker.unlock();

    while (true) {
        cout << "Process a picture!" << endl;
    }
}

void detect() {
    unique_lock<mutex> locker{m};
    if (!isFinishInit) {
        condition.wait(locker);
    }

    cout << "Start detect" << endl;
    while (true) {
        cout << "Detect a picture!" << endl;
    }
}

int main() {
    thread proc1(process);
    thread proc2(detect);
    proc1.join();
    proc2.join();
    return 0;
}
```

```cpp
notify_one(); //通知第一个进入等待的
notify_all(); //通知所有进入等待的
```

## Lambda 表达式

### 引入

```cpp
void Traverse(const vector<int> &array, function<int(int)> func) {
    for (const auto &v: array) {
        func(v);
    }
}

int Print(int i) {
    cout << i << endl;
    return i;
}

int main() {
    vector<int> array = {1, 2, 3, 4, 5, 6, 7, 8, 9, 0};
    Traverse(array, Print);

    return 0;
}
```

```cpp
void Traverse(const vector<int> &array, function<int(int)> func) {
    for (const auto &v: array) {
        func(v);
    }
}

int main() {
    vector<int> array = {1, 2, 3, 4, 5, 6, 7, 8, 9, 0};
    Traverse(array,
             [](int i) {
                 cout << i << endl;
                 return i;
             }
    );

    return 0;
}
```

### 优势

- 声明式编程风格：就地匿名定义目标函数或函数对象，不需要额外写一个命名函数或者函数对象。以更直接的方式去写程序，好的可读性和可维护性。
- 简洁：不需要额外再写一个函数或者函数对象，避免了代码膨胀和功能分散，让开发者更加集中精力在手边的问题，同时也获取了更高的生产率。
- 在需要的时间和地点实现功能闭包，使程序更灵活。

### 使用

```cpp
[ capture ] ( params ) { body; };
```

```cpp
int main() {
    auto f = []{
        cout << "Hello" << endl;
    };
    
    f();

    return 0;
}
```

### 表达式捕获

- [=]按值捕获
- [&]按引用捕获
- [name]捕获特定变量