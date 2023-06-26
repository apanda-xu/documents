- [1 基础知识](#1-基础知识)
  - [1.2 睡眠](#12-睡眠)
  - [1.3 getopt函数](#13-getopt函数)
  - [1.4 ".h"头文件的作用](#14-h头文件的作用)
  - [1.5 静态局部变量初始化的线程安全性](#15-静态局部变量初始化的线程安全性)
  - [1.6 饿汉模式 （程序启动时创建并初始化对象）](#16-饿汉模式-程序启动时创建并初始化对象)
    - [以下是饿汉模式的示例代码](#以下是饿汉模式的示例代码)
  - [1.7 懒汉模式（需要时创建并初始化对象）](#17-懒汉模式需要时创建并初始化对象)
    - [1. 使用加锁方式实现线程安全懒汉模式：](#1-使用加锁方式实现线程安全懒汉模式)
    - [2. 使用双重检查锁定方式实现线程安全懒汉模式：](#2-使用双重检查锁定方式实现线程安全懒汉模式)
  - [1.8 信号量（C）](#18-信号量c)
    - [1. 示例](#1-示例)
    - [2. \<semaphore.h\>用法](#2-semaphoreh用法)
  - [1.9 互斥锁（C）](#19-互斥锁c)
    - [1. 示例](#1-示例-1)
    - [2. \<pthread.h\>用法](#2-pthreadh用法)
  - [1.10 条件变量（C）](#110-条件变量c)
    - [1. 示例](#1-示例-2)
    - [2. \<pthread.h\>用法](#2-pthreadh用法-1)
  - [1.11 \<exception\>用法](#111-exception用法)
    - [1. 介绍](#1-介绍)
    - [2. 示例](#2-示例)
  - [1.11 std::unique\_lock](#111-stdunique_lock)
- [2 功能模块](#2-功能模块)
  - [2.1 线程池](#21-线程池)
  - [2.2 多路复用](#22-多路复用)

# 1 基础知识 
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
    C++11 标准之前，对于静态局部变量的初始化是不线程安全的。多个线程同时访问该静态局部变量的初始化过程可能导致竞态条件和未定义行为。

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

## 1.8 信号量（C）
### 1. 示例
```c
#include<semaphore.h>

// 封装信号量的操作
class sem
{
public:
    // 默认构造函数：初始化信号量为0
    sem()
    {
        if (sem_init(&m_sem, 0, 0) != 0)
        {
            throw std::exception();
        }
    }
    // 有参构造函数：初始化信号量为num
    sem(int num)
    {
        if (sem_init(&m_sem, 0, num) != 0)
        {
            throw std::exception();
        }
    }
    // 析构函数：销毁信号量
    ~sem()
    {
        sem_destroy(&m_sem);
    }
    // 成员函数：减少信号量的值。如果信号量的值大于0，则将其减一，表示资源已被占用，函数返回true；如果信号量值为0，则阻塞等待，直到有其他线程调用post函数，增加信号量的值
    bool wait()
    {
        return sem_wait(&m_sem) == 0;
    }
    // 成员函数：增加信号量的值。表示资源已经释放，其他等待的线程可以继续执行，函数返回true
    bool post()
    {
        return sem_post(&m_sem) == 0;
    }

private:
    sem_t m_sem;
};
```
### 2. <semaphore.h>用法
```c++
// 1. sem_init函数
int sem_init(sem_t *sem, int pshared, unsigned int value);
```
    该函数用于初始化一个信号量，其中：
    sem参数是指向信号量的指针；
    pshared参数指定信号量的类型，取值为0表示信号量是线程内共享的，取值为非0表示信号量是进程间共享的；
    value参数指定信号量的初始值。
    该函数成功返回0，失败返回-1。
```c++
// 2. sem_destroy函数
int sem_destroy(sem_t *sem);
```
    该函数用于销毁一个信号量，其中：
    sem参数是指向信号量的指针。
    该函数成功返回0，失败返回-1。
```c++
// 3. sem_wait函数
int sem_wait(sem_t *sem);
```
    该函数用于获取一个信号量，如果信号量的值大于0，则将其减1并返回0，表示获取成功；如果信号量的值等于0，则线程会被阻塞，直到有其他线程或者进程释放信号量。
    需要注意的是，sem_wait函数是原子操作，即在获取信号量的过程中，不会被其他线程或者进程打断。如果在获取信号量的过程中，出现信号中断或者其他错误，则会返回-1，并设置相应的错误码。
```c++
// 4. sem_post函数
int sem_post(sem_t *sem);
```
    该函数用于释放一个信号量，将其值加1。
    需要注意的是，sem_post函数也是原子操作，即在释放信号量的过程中，不会被其他线程或者进程打断。如果释放信号量失败，则会返回-1，并设置相应的错误码。
    这些函数是使用信号量的基本操作。在使用时，需要先通过sem_init函数初始化一个信号量，然后在需要同步的代码段中使用sem_wait和sem_post函数来获取和释放信号量。最后，使用sem_destroy函数来销毁信号量。

## 1.9 互斥锁（C）
### 1. 示例
```c++
#include<pthread.h>
#include<exception>
class locker
{
public:
    // 构造函数：初始化互斥锁，如果失败，抛出异常
    locker()
    {
        if (pthread_mutex_init(&m_mutex, NULL) != 0)
        {
            throw std::exception();
        }
    }
    // 析构函数：销毁互斥锁
    ~locker()
    {
        pthread_mutex_destroy(&m_mutex);
    }
    // 获取锁
    bool lock()
    {
        return pthread_mutex_lock(&m_mutex) == 0;
    }
    // 释放锁
    bool unlock()
    {
        return pthread_mutex_unlock(&m_mutex) == 0;
    }
    // 获取互斥锁指针的函数，返回一个指向互斥锁的指针
    pthread_mutex_t *get()
    {
        return &m_mutex;
    }

private:
    pthread_mutex_t m_mutex;
};
```
### 2. <pthread.h>用法
```c++
//1. 在使用互斥锁时，需要先定义一个pthread_mutex_t类型的变量，例如：
pthread_mutex_t m_mutex;
```
```c++
// 2. 使用pthread_mutex_init函数对互斥锁进行初始化, 在初始化时，可以通过第二个参数指定互斥锁的属性，通常情况下可以将其设置为NULL，表示使用默认属性：
pthread_mutex_init(&m_mutex, NULL);
```
```c++
// 3. 在使用互斥锁进行同步时，可以使用pthread_mutex_lock函数获取互斥锁，如果获取成功，则可以访问共享资源；如果获取失败，则线程会被阻塞，直到互斥锁被释放：
pthread_mutex_lock(&m_mutex);
```
```c++
// 4. 在访问完共享资源后，需要使用pthread_mutex_unlock函数释放互斥锁，这样，其他线程就可以获取互斥锁并访问共享资源了：
pthread_mutex_unlock(&m_mutex);
```
```c++
// 5. 最后，在程序结束时，需要使用pthread_mutex_destroy函数销毁互斥锁，这样可以释放互斥锁占用的资源：
pthread_mutex_destroy(&m_mutex);
```
## 1.10 条件变量（C）
### 1. 示例
```c++
#include<pthread.h>
class cond
{
public:
    // 构造函数：初始化条件变量
    cond()
    {
        if (pthread_cond_init(&m_cond, NULL) != 0)
        {
            throw std::exception();
        }
    }
    // 析构函数：销毁条件变量
    ~cond()
    {
        pthread_cond_destroy(&m_cond);
    }
    // 成员函数：等待条件变量满足，该函数在等待条件变量期间会释放互斥锁，并在条件变量满足时重新获得互斥锁
    bool wait(pthread_mutex_t *m_mutex)
    {
        int ret = 0;
        //pthread_mutex_lock(&m_mutex);
        ret = pthread_cond_wait(&m_cond, m_mutex);
        //pthread_mutex_unlock(&m_mutex);
        return ret == 0;
    }
    // 成员函数：在指定时间内等待条件变量满足
    bool timewait(pthread_mutex_t *m_mutex, struct timespec t)
    {
        int ret = 0;
        //pthread_mutex_lock(&m_mutex);
        ret = pthread_cond_timedwait(&m_cond, m_mutex, &t);
        //pthread_mutex_unlock(&m_mutex);
        return ret == 0;
    }
    // 成员函数：发送信号唤醒一个正在等待条件变量的线程
    bool signal()
    {
        return pthread_cond_signal(&m_cond) == 0;
    }
    // 成员函数：发送广播唤醒所有正在等待条件变量的线程
    bool broadcast()
    {
        return pthread_cond_broadcast(&m_cond) == 0;
    }

private:
    //static pthread_mutex_t m_mutex;
    pthread_cond_t m_cond;
}
```
### 2. <pthread.h>用法
```c++
// 1. 初始化条件变量对象。
int pthread_cond_init(pthread_cond_t *cond, const pthread_condattr_t *attr)：
```
    参数：
    cond：指向条件变量对象的指针。
    attr：指向条件变量属性对象的指针，一般可以传入NULL。
    返回值：成功返回0，失败返回错误码。
```c++
// 2. 销毁条件变量对象。
int pthread_cond_destroy(pthread_cond_t *cond)：
```
    参数：
    cond：指向条件变量对象的指针。
    返回值：成功返回0，失败返回错误码。
 
```c++
// 3. 等待条件变量满足，并在等待期间自动释放关联的互斥锁。
int pthread_cond_wait(pthread_cond_t *cond, pthread_mutex_t *mutex)：
```
    参数：
    cond：指向条件变量对象的指针。
    mutex：指向互斥锁对象的指针。
    返回值：成功返回0，失败返回错误码。

```c++
// 4. 发送信号，唤醒一个正在等待条件变量的线程。
int pthread_cond_signal(pthread_cond_t *cond)：
```
    参数：
    cond：指向条件变量对象的指针。
    返回值：成功返回0，失败返回错误码。

```c++
// 5. 发送广播，唤醒所有正在等待条件变量的线程。
int pthread_cond_broadcast(pthread_cond_t *cond)：
```
    参数：
    cond：指向条件变量对象的指针。
    返回值：成功返回0，失败返回错误码。
## 1.11 \<exception>用法
### 1. 介绍
    <exception>是一个标准C++库头文件，包含了异常处理相关的类和函数。在C++程序中，异常处理是一种处理错误的机制，当程序出现错误时，可以抛出一个异常，并在适当的地方捕获和处理这个异常。下面是一些常用的异常处理类和函数：

    (1) std::exception：标准异常类。所有标准异常类都继承自这个类，用于表示通用的异常错误。
    (2) std::runtime_error：运行时异常类。继承自std::exception，用于表示运行时错误。
    (3) std::logic_error：逻辑错误异常类。继承自std::exception，用于表示逻辑错误。
    (4) std::bad_alloc：内存分配异常类。继承自std::exception，用于表示内存分配失败。
    (5) try：异常处理语句块。使用try语句块可以标识可能会抛出异常的代码块。
    (6) throw：抛出异常语句。使用throw语句可以抛出一个异常。
    (7) catch：异常捕获语句块。使用catch语句块可以捕获并处理抛出的异常。
    (8) std::terminate：异常终止函数。如果程序中没有捕获到抛出的异常，会调用这个函数终止程序。

    使用异常处理可以让程序更加健壮，避免出现未处理的错误导致程序崩溃。需要注意的是，在使用异常处理时需要特别注意异常的类型和处理方式，避免出现不必要的异常和异常处理不当导致的问题。
### 2. 示例
```c++
#include <iostream>
#include <exception>

int main() {
    try {
        int a, b;
        std::cout << "Enter two numbers: ";
        std::cin >> a >> b;

        if (b == 0) {
            throw std::runtime_error("Divide by zero error");
        }

        std::cout << "Result: " << a / b << std::endl;
    } catch (const std::exception& e) {
        std::cerr << "Error: " << e.what() << std::endl;
    }

    return 0;
}
```


## 1.11 std::unique_lock
    构造函数：std::unique_lock<std::mutex> lock(mtx); // 创建std::unique_lock对象，并立即锁定互斥量mtx。等同于mtx.lock()。

# 2 功能模块
## 2.1 线程池
## 2.2 多路复用