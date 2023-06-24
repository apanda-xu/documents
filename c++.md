# 1 基础知识 
## 1.1 std::unique_lock，用于管理互斥量（std::mutex）的RAII类，提供了更多的灵活性和功能，用法如下：
    构造函数：std::unique_lock<std::mutex> lock(mtx); // 创建std::unique_lock对象，并立即锁定互斥量mtx。等同于mtx.lock()。

## 1.2 睡眠
```c++
#include<thread>
#include<chrono>

std::this_thread::sleep_for(std::chrono::seconds(1));           // 秒
std::this_thread::sleep_for(std::chrono::milliseconds(100));    // 毫秒
std::this_thread::sleep_for(std::chrono::microseconds(500));    // 微秒
```
## 1.3 getopt函数

## 1.4 ".h"头文件的作用
    在C++中，.h 文件通常用于头文件（Header File），它们具有以下作用：

    1. 声明函数和类：头文件包含函数和类的声明，允许其他源文件在编译时知道这些函数和类的存在和接口。通过包含头文件，可以在不暴露实现细节的情况下使用函数和类。

    2. 定义常量和宏：头文件可以包含常量定义、宏定义以及其他预处理指令。这些定义可以在多个源文件中共享，提供了一种方便的方式来定义常量和宏，并确保它们在整个程序中的一致性。

    3. 包含其他头文件：头文件可以包含其他头文件，以便将多个相关的声明和定义组织在一起。通过使用头文件，可以提高代码的模块化性和可维护性。

    4. 提供模板和内联函数定义：对于模板类和内联函数，它们的定义通常包含在头文件中。这是因为模板和内联函数的实现需要在编译时可见，以便进行实例化或内联展开。

    5. 导入外部库的声明：如果使用外部库（例如第三方库或标准库），头文件通常包含该库的声明和接口定义，以便在程序中使用这些库的功能。

    请注意，头文件只包含声明和定义，而不应该包含实际的实现代码。实现代码应该放在源文件（.cpp 文件）中，并通过包含相应的头文件来使用声明和定义。
    使用头文件可以提高代码的可读性、重用性和维护性。它们有助于将程序分割为逻辑模块，并提供清晰的接口定义。同时，使用头文件可以避免在多个源文件中重复编写相同的声明和定义。

## 1.5 静态局部变量初始化的线程安全性
    在 C++11 标准之前，对于静态局部变量的初始化是不线程安全的。多个线程同时访问该静态局部变量的初始化过程可能导致竞态条件和未定义行为。
    
    C++11 标准引入了线程安全的局部静态变量初始化。根据 C++11 标准规定，静态局部变量的初始化在多线程环境下是线程安全的。编译器会生成初始化代码，以确保只有一个线程可以执行变量的初始化，并且这个过程是线程安全的。因此，如果你使用的是 C++11 或更高版本的编译器，可以使用静态局部变量来实现线程安全的懒汉模式，而无需显式地使用互斥锁或双重检查锁定。

## 1.6 饿汉模式 （程序启动时创建并初始化对象）
    饿汉模式（Eager Initialization）是一种单例模式的实现方式，它在程序启动时就立即创建并初始化单例对象。

    在饿汉模式中，单例对象在类加载时就被创建，无论是否使用该对象。它的特点是在类加载时就创建了对象实例，因此它是线程安全的。
### 以下是饿汉模式的示例代码
```c++
class Singleton {
private:
    static Singleton instance;
    // 私有构造函数
    Singleton() {}

public:
    // 全局访问点，用于获取单例对象
    static Singleton& getInstance() {
        return instance;
    }

    // 其他成员函数
    void doSomething() {
        // 执行操作
    }
};

// 静态成员初始化
Singleton Singleton::instance;
```

    在上述示例中，instance 对象在类加载时就被创建，并在声明时进行了初始化。因此，无论何时需要访问单例对象，都可以通过 getInstance 方法获取到该对象。

    饿汉模式的优点是实现简单，且在多线程环境下不需要考虑线程安全问题。缺点是无论是否使用该对象，都会在程序启动时进行创建和初始化，可能会增加程序的启动时间和内存消耗。

    选择饿汉模式还是懒汉模式取决于具体的需求和场景。如果对象的创建和初始化过程较为简单，并且在程序运行期间一定会被使用到，那么饿汉模式是一个不错的选择。但如果对象的创建和初始化过程较为复杂，或者可能在程序运行期间并不一定被使用到，那么懒汉模式可能更合适。
## 1.7 懒汉模式（需要时创建并初始化对象）
    懒汉模式（Lazy Initialization）是一种单例模式的实现方式，它在需要时才创建并初始化单例对象。

    在懒汉模式中，单例对象的初始化是延迟到第一次访问时进行的，即在需要时才创建对象实例。它的特点是在第一次访问时才进行对象的实例化，因此有可能存在线程安全问题。在多线程环境下，需要额外的同步机制来保证只有一个实例被创建。
### 1. 使用加锁方式实现线程安全懒汉模式：
```c++
#include <mutex>

class Singleton {
private:
    static Singleton* instance;
    static std::mutex mtx;

    // 私有构造函数
    Singleton() {}

public:
    // 全局访问点，用于获取单例对象
    static Singleton* getInstance() {
        std::lock_guard<std::mutex> lock(mtx);  // 加锁

        if (instance == nullptr) {
            instance = new Singleton();
        }
        return instance;
    }

    // 其他成员函数
    void doSomething() {
        // 执行操作
    }
};

Singleton* Singleton::instance = nullptr;
std::mutex Singleton::mtx;
```
    在上述示例中，通过使用 std::mutex 实现了加锁的方式来保证线程安全。在 getInstance 方法中，先加锁，然后检查实例是否为 nullptr，如果是，则创建实例。这样能够确保在多线程环境下只有一个实例被创建。
### 2. 使用双重检查锁定方式实现线程安全懒汉模式：
```c++
#include <mutex>

class Singleton {
private:
    static Singleton* instance;
    static std::mutex mtx;

    // 私有构造函数
    Singleton() {}

public:
    // 全局访问点，用于获取单例对象
    static Singleton* getInstance() {
        if (instance == nullptr) {
            std::lock_guard<std::mutex> lock(mtx);  // 加锁
            if (instance == nullptr) {
                instance = new Singleton();
            }
        }
        return instance;
    }

    // 其他成员函数
    void doSomething() {
        // 执行操作
    }
};

Singleton* Singleton::instance = nullptr;
std::mutex Singleton::mtx;
```
    在上述示例中，通过双重检查锁定的方式实现线程安全的懒汉模式。在 getInstance 方法中，首先检查实例是否为 nullptr，如果是，则加锁并再次检查，以确保只有一个线程创建实例。这样可以减少加锁的频率，提高性能。

# 2 功能模块
## 2.1 线程池
## 2.2 多路复用