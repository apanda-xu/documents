- [1 基础知识](#1-基础知识)
  - [1.1 \<exception\>用法](#11-exception用法)
    - [（1）介绍](#1介绍)
    - [（2）示例](#2示例)
  - [1.2 sleep](#12-sleep)
  - [1.3 getopt](#13-getopt)
  - [1.4 ".h"头文件的作用](#14-h头文件的作用)
  - [1.5 静态局部变量初始化的线程安全性](#15-静态局部变量初始化的线程安全性)
  - [1.6 饿汉模式 （程序启动时创建并初始化对象）](#16-饿汉模式-程序启动时创建并初始化对象)
    - [（1）介绍](#1介绍-1)
    - [（2）【全局静态变量】实现饿汉模式](#2全局静态变量实现饿汉模式)
  - [1.7 懒汉模式（需要时创建并初始化对象）](#17-懒汉模式需要时创建并初始化对象)
    - [（1）【静态全局变量 + 加锁】实现线程安全懒汉模式：](#1静态全局变量--加锁实现线程安全懒汉模式)
    - [（2）【静态全局变量 + 双重检查锁定】实现线程安全懒汉模式：](#2静态全局变量--双重检查锁定实现线程安全懒汉模式)
    - [（3）【静态局部变量】实现线程安全懒汉模式](#3静态局部变量实现线程安全懒汉模式)
  - [1.8 信号量（C）](#18-信号量c)
    - [（1）示例](#1示例)
    - [（2）\<semaphore.h\>用法](#2semaphoreh用法)
  - [1.9 互斥锁（C）](#19-互斥锁c)
    - [（1）示例（封装）](#1示例封装)
    - [（2）\<pthread.h\>用法](#2pthreadh用法)
  - [1.10 条件变量（C）](#110-条件变量c)
    - [（1）示例（封装）](#1示例封装-1)
    - [（2）示例](#2示例-1)
    - [（3）\<pthread.h\>用法](#3pthreadh用法)
  - [1.11 互斥锁（C++）](#111-互斥锁c)
    - [（1）std::unique\_lock](#1stdunique_lock)
      - [用法](#用法)
      - [示例（单独使用）](#示例单独使用)
      - [示例（配合条件变量）](#示例配合条件变量)
    - [（2）std::lock\_guard](#2stdlock_guard)
      - [介绍](#介绍)
      - [用法](#用法-1)
      - [示例](#示例)
  - [1.12 条件变量（C++）](#112-条件变量c)
    - [（1）介绍](#1介绍-2)
    - [（2）用法](#2用法)
    - [（3）示例](#3示例)
  - [1.13 连接数据库的方法](#113-连接数据库的方法)
  - [1.14 通过 MySQL C API 与MySQL交互](#114-通过-mysql-c-api-与mysql交互)
    - [（1）MySQL C API](#1mysql-c-api)
    - [（2）连接和增删改查](#2连接和增删改查)
  - [1.15 防止头文件重复包含](#115-防止头文件重复包含)
    - [（1）介绍](#1介绍-3)
    - [（2）示例](#2示例-2)
  - [1.16 创建线程](#116-创建线程)
    - [（1）C](#1c)
      - [用法](#用法-2)
      - [示例](#示例-1)
    - [（2）C++](#2c)
      - [用法](#用法-3)
      - [示例](#示例-2)
- [2 功能模块](#2-功能模块)
  - [2.1 线程池](#21-线程池)
    - [（1）介绍](#1介绍-4)
    - [（2）示例](#2示例-3)
      - [示例1](#示例1)
      - [示例2 (webserver)：](#示例2-webserver)
  - [2.2 数据库连接池](#22-数据库连接池)
    - [（1）介绍](#1介绍-5)
    - [（2）示例 (webserver)](#2示例-webserver)
      - [sql\_connection.h](#sql_connectionh)
      - [sql\_connection.cpp](#sql_connectioncpp)
  - [2.3 多路复用](#23-多路复用)



# 1 基础知识 
## 1.1 \<exception>用法
### （1）介绍
    <exception>是一个标准C++库头文件，包含了异常处理相关的类和函数。在C++程序中，异常处理是一种处理错误的机制，当程序出现错误时，可以抛出一个异常，并在适当的地方捕获和处理这个异常。下面是一些常用的异常处理类和函数：

    1. std::exception：标准异常类。所有标准异常类都继承自这个类，用于表示通用的异常错误。
    2. std::runtime_error：运行时异常类。继承自std::exception，用于表示运行时错误。
    3. std::logic_error：逻辑错误异常类。继承自std::exception，用于表示逻辑错误。
    4. std::bad_alloc：内存分配异常类。继承自std::exception，用于表示内存分配失败。
    5. try：异常处理语句块。使用try语句块可以标识可能会抛出异常的代码块。
    6. throw：抛出异常语句。使用throw语句可以抛出一个异常。
    7. catch：异常捕获语句块。使用catch语句块可以捕获并处理抛出的异常。
    8. std::terminate：异常终止函数。如果程序中没有捕获到抛出的异常，会调用这个函数终止程序。

    使用异常处理可以让程序更加健壮，避免出现未处理的错误导致程序崩溃。需要注意的是，在使用异常处理时需要特别注意异常的类型和处理方式，避免出现不必要的异常和异常处理不当导致的问题。
### （2）示例
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

## 1.2 sleep
```c++
#include<thread>
#include<chrono>

std::this_thread::sleep_for(std::chrono::seconds(1));           // 秒
std::this_thread::sleep_for(std::chrono::milliseconds(100));    // 毫秒
std::this_thread::sleep_for(std::chrono::microseconds(500));    // 微秒
```
## 1.3 getopt

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
### （1）介绍
    饿汉模式（Eager Initialization）是一种单例模式的实现方式，它在程序启动时就立即创建并初始化单例对象。

    在饿汉模式中，单例对象在类加载时就被创建，无论是否使用该对象。它的特点是在类加载时就创建了对象实例，因此它是线程安全的。
### （2）【全局静态变量】实现饿汉模式
```c++
class Singleton {
private:
    static Singleton instance;
    // 私有构造函数
    Singleton() {}

public:
    // 全局访问点，用于获取单例对象
    static Singleton* getInstance() {
        return &instance;
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
### （1）【静态全局变量 + 加锁】实现线程安全懒汉模式：
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
### （2）【静态全局变量 + 双重检查锁定】实现线程安全懒汉模式：
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

### （3）【静态局部变量】实现线程安全懒汉模式
```c++
#include <iostream>

class Singleton {
private:
    Singleton() {
        std::cout << "Singleton instance created." << std::endl;
    }

public:
    static Singleton* getInstance() {
        static Singleton instance;
        return &instance;
    }
};

int main() {
    Singleton* singleton1 = Singleton::getInstance();  // 第一次调用getInstance()，会创建Singleton实例
    Singleton* singleton2 = Singleton::getInstance();  // 后续调用getInstance()，直接返回已创建的实例

    return 0;
}
```
    在上面的代码中，Singleton类的构造函数是私有的，这意味着不能直接创建Singleton的实例。通过getInstance()静态函数获取Singleton的实例，该函数内部定义了一个静态局部变量instance，并在第一次调用时进行初始化。后续调用getInstance()时，直接返回已创建的实例。

    在main()函数中，通过Singleton::getInstance()获取Singleton的实例，第一次调用时会输出"Singleton instance created."，后续调用时不会再创建新的实例，只是返回已创建的实例。根据C++11标准，当多个线程同时尝试首次访问静态局部变量时，编译器会保证只有一个线程能够成功初始化该变量，而其他线程将等待初始化完成后直接获取已初始化的变量。

    这种方式使用静态局部变量实现懒汉模式的优点是线程安全，每个线程都有自己的实例，不会出现竞争条件。

## 1.8 信号量（C）
### （1）示例
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
### （2）<semaphore.h>用法
```c++
// 1. sem_init函数
int sem_init(sem_t *sem, int pshared, unsigned int value);

该函数用于初始化一个信号量，其中：
sem参数是指向信号量的指针；
pshared参数指定信号量的类型，取值为0表示信号量是线程内共享的，取值为非0表示信号量是进程间共享的；
value参数指定信号量的初始值。
该函数成功返回0，失败返回-1。
```

```c++
// 2. sem_destroy函数
int sem_destroy(sem_t *sem);

该函数用于销毁一个信号量，其中：
sem参数是指向信号量的指针。
该函数成功返回0，失败返回-1。
```

```c++
// 3. sem_wait函数
int sem_wait(sem_t *sem);

该函数用于获取一个信号量，如果信号量的值大于0，则将其减1并返回0，表示获取成功；如果信号量的值等于0，则线程会被阻塞，直到有其他线程或者进程释放信号量。
需要注意的是，sem_wait函数是原子操作，即在获取信号量的过程中，不会被其他线程或者进程打断。如果在获取信号量的过程中，出现信号中断或者其他错误，则会返回-1，并设置相应的错误码。
```

```c++
// 4. sem_post函数
int sem_post(sem_t *sem);

该函数用于释放一个信号量，将其值加1。
需要注意的是，sem_post函数也是原子操作，即在释放信号量的过程中，不会被其他线程或者进程打断。如果释放信号量失败，则会返回-1，并设置相应的错误码。
这些函数是使用信号量的基本操作。在使用时，需要先通过sem_init函数初始化一个信号量，然后在需要同步的代码段中使用sem_wait和sem_post函数来获取和释放信号量。最后，使用sem_destroy函数来销毁信号量。
```


## 1.9 互斥锁（C）
### （1）示例（封装）
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
### （2）<pthread.h>用法
```c++
//1. 在使用互斥锁时，需要先定义一个pthread_mutex_t类型的变量
pthread_mutex_t m_mutex;
```
```c++
// 2. 使用pthread_mutex_init函数对互斥锁进行初始化, 在初始化时，可以通过第二个参数指定互斥锁的属性，通常情况下可以将其设置为NULL，表示使用默认属性
pthread_mutex_init(&m_mutex, NULL);
```
```c++
// 3. 在使用互斥锁进行同步时，可以使用pthread_mutex_lock函数获取互斥锁。如果获取成功，则可以访问共享资源；如果获取失败，则线程会被阻塞，直到互斥锁被释放
pthread_mutex_lock(&m_mutex);
```
```c++
// 4. 在访问完共享资源后，需要使用pthread_mutex_unlock函数释放互斥锁，这样，其他线程就可以获取互斥锁并访问共享资源了
pthread_mutex_unlock(&m_mutex);
```
```c++
// 5. 在程序结束时，需要使用pthread_mutex_destroy函数销毁互斥锁，这样可以释放互斥锁占用的资源
pthread_mutex_destroy(&m_mutex);
```
## 1.10 条件变量（C）
### （1）示例（封装）
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
### （2）示例
```c++
#include<iostream>
#include<pthread.h>
#include<chrono>
#include<thread>

pthread_mutex_t mutex;
pthread_cond_t cond;
bool flag = false;

void* threadFunc1(void* arg)
{
    pthread_mutex_lock(&mutex);
    // 等待条件变量满足
    while (!flag)
    {
        pthread_cond_wait(&cond, &mutex);
    }
    std::cout << "Thread 1: Condition is met!" << std::endl;
    pthread_mutex_unlock(&mutex);
    return NULL;
}

void* threadFunc2(void* arg)
{
    // 模拟操作
    std::this_thread::sleep_for(std::chrono::seconds(2));
    pthread_mutex_lock(&mutex);
    // 设置条件变量为满足状态
    flag = true;
    // 唤醒等待条件变量的线程
    pthread_cond_signal(&cond);
    pthread_mutex_unlock(&mutex);
    return NULL;
}

int main()
{
    pthread_t thread1, thread2;

    // 初始化互斥锁和条件变量
    pthread_mutex_init(&mutex, NULL);
    pthread_cond_init(&cond, NULL);

    // 创建线程
    pthread_create(&thread1, NULL, threadFunc1, NULL);
    pthread_create(&thread2, NULL, threadFunc2, NULL);

    // 等待线程结束
    pthread_join(thread1, NULL);
    pthread_join(thread2, NULL);

    // 销毁互斥锁和条件变量
    pthread_mutex_destroy(&mutex);
    pthread_cond_destroy(&cond);

    return 0;
}
```
### （3）<pthread.h>用法
```c++
// 1. 初始化条件变量对象
int pthread_cond_init(pthread_cond_t *cond, const pthread_condattr_t *attr)：
参数：
cond：指向条件变量对象的指针。
attr：指向条件变量属性对象的指针，一般可以传入NULL。
返回值：成功返回0，失败返回错误码。
```

```c++
// 2. 销毁条件变量对象
int pthread_cond_destroy(pthread_cond_t *cond)：
参数：
cond：指向条件变量对象的指针。
返回值：成功返回0，失败返回错误码。
```

```c++
// 3. 等待条件变量满足，并在等待期间自动释放关联的互斥锁。
int pthread_cond_wait(pthread_cond_t *cond, pthread_mutex_t *mutex)：
参数：
cond：指向条件变量对象的指针。
mutex：指向互斥锁对象的指针。
返回值：成功返回0，失败返回错误码。
```

```c++
// 4. 发送信号，唤醒一个正在等待条件变量的线程。
int pthread_cond_signal(pthread_cond_t *cond)：
参数：
cond：指向条件变量对象的指针。
返回值：成功返回0，失败返回错误码。
```

```c++
// 5. 发送广播，唤醒所有正在等待条件变量的线程。
int pthread_cond_broadcast(pthread_cond_t *cond)：
参数：
cond：指向条件变量对象的指针。
返回值：成功返回0，失败返回错误码。
```

## 1.11 互斥锁（C++）
### （1）std::unique_lock
#### 用法
    是 C++ 标准库提供的一个灵活的互斥锁管理类模板，它提供了多种用法来操作互斥锁，包括上锁、解锁、延期上锁等操作。
    下面是 std::unique_lock 主要的用法:
```c++
// 1. 构造函数
std::unique_lock<std::mutex> lock(mutex);                   // 创建并上锁互斥锁
std::unique_lock<std::mutex> lock(mutex, std::defer_lock);  // 创建但不上锁互斥锁
```
```c++
// 2. 手动上锁
lock.lock();    // 上锁互斥锁
```

```c++
// 3. 手动解锁
lock.unlock();  // 解锁互斥锁
```

```c++
// 4. 作用域解锁
{
    std::unique_lock<std::mutex> lock(mutex);  // 上锁互斥锁
    // 互斥锁被上锁，执行其他操作
}   // 作用域结束，互斥锁自动解锁

```

```c++
// 5. 配合条件变量
std::unique_lock<std::mutex> lock(mutex);
condition.wait(lock);   // 等待条件满足，自动释放互斥锁并等待条件变量通知重新上锁
```

```c++
// 6. 延迟上锁
std::unique_lock<std::mutex> lock(mutex, std::defer_lock);
// 执行其他操作
lock.lock();    // 手动上锁互斥锁
```

```c++
// 7. 所有权转移
std::unique_lock<std::mutex> lock1(mutex);
std::unique_lock<std::mutex> lock2(std::move(lock1));  // lock2 现在拥有互斥锁的所有权

```
#### 示例（单独使用）
```c++
#include <iostream>
#include <thread>
#include <mutex>

std::mutex mtx;

void printMessage(const std::string& message)
{
    // 上锁互斥锁
    std::unique_lock<std::mutex> lock(mtx);  
    // 执行临界区操作
    std::cout << message << std::endl;
    std::this_thread::sleep_for(std::chrono::seconds(2));
    // 在作用域结束时自动解锁互斥锁
}

int main()
{
    std::thread t1(printMessage, "Thread 1");
    std::thread t2(printMessage, "Thread 2");
    t1.join();
    t2.join();
    return 0;
}
```
#### 示例（配合条件变量）
```c++
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>

std::mutex mtx;
std::condition_variable cv;
bool ready = false;

void waitingThread()
{
    std::unique_lock<std::mutex> lock(mtx);
    while (!ready) {
        std::cout << "Waiting thread: Condition is not met!" << std::endl;
        cv.wait(lock);  // 等待条件变量，同时释放互斥锁
    }
    std::cout << "Waiting thread: Condition is met!" << std::endl;
}

void notifyingThread()
{
    std::this_thread::sleep_for(std::chrono::seconds(2));
    std::unique_lock<std::mutex> lock(mtx);
    ready = true;
    cv.notify_one();  // 通知等待的线程条件变量已满足
}

int main()
{
    std::thread t1(waitingThread);
    std::thread t2(notifyingThread);

    t1.join();
    t2.join();

    return 0;
}
```

### （2）std::lock_guard
#### 介绍
    std::lock_guard 是 C++ 标准库中提供的一个模板类，用于简化互斥锁的上锁和解锁过程。它在构造时上锁互斥锁，并在析构时自动解锁互斥锁，确保互斥锁的正确使用，避免忘记解锁的问题。
#### 用法
    std::lock_guard 的使用非常简单，只需要按以下步骤操作：
    1. 创建一个 std::mutex（互斥锁）对象。
    2. 在需要保护的临界区域内，创建一个 std::lock_guard 对象并传入互斥锁对象。
    3. 执行临界区域的代码。
    当 std::lock_guard 对象的作用域结束时，析构函数会被调用，自动解锁互斥锁。
#### 示例
```c++
#include <iostream>
#include <thread>
#include <mutex>

std::mutex mtx;
int sharedData = 0;

void updateSharedData()
{
    std::lock_guard<std::mutex> lock(mtx);  // 上锁互斥锁
    ++sharedData;   // 执行临界区域的代码
    std::cout << "Updated shared data: " << sharedData << std::endl;
    // 超出作用域后，lock销毁，会自动解锁互斥锁
}

int main()
{
    std::thread t1(updateSharedData);
    std::thread t2(updateSharedData);

    t1.join();
    t2.join();

    return 0;
}

```

## 1.12 条件变量（C++）
### （1）介绍
    C++ 中的条件变量（std::condition_variable）用于线程之间的同步和通信。它允许一个线程等待另一个线程满足特定的条件，从而实现线程的协调和同步。
### （2）用法
```c++
// 1. 等待条件满足
// 一个线程通过调用 wait() 函数来等待条件变量满足特定条件。在等待期间，线程会被阻塞，释放所持有的互斥锁，并等待其他线程通过 notify_one() 或 notify_all() 来通知条件满足。
std::unique_lock<std::mutex> lock(mutex);
cv.wait(lock, []{ return condition; });
```

```c++
// 2. 通知条件满足
// 一个线程通过调用 notify_one() 或 notify_all() 来通知其他等待的线程条件已经满足，从而唤醒等待的线程继续执行。
std::unique_lock<std::mutex> lock(mutex);
condition = true;
cv.notify_one();
```

```c++
// 3. 超时等待
// 除了无限等待外，还可以设置等待的超时时间。可以使用 wait_for() 或 wait_until() 函数来实现。
std::unique_lock<std::mutex> lock(mutex);
if (cv.wait_for(lock, std::chrono::seconds(1), []{ return condition; }))
{
    // 条件满足
}
else
{
    // 超时
}
```

```c++
// 4. 条件变量的结合使用
// 通常与互斥锁配合使用，以保护共享资源的访问
```
### （3）示例
```c++
#include <iostream>
#include <thread>
#include <condition_variable>

std::mutex mtx;
std::condition_variable cv;
bool ready = false;

void worker()
{
    std::unique_lock<std::mutex> lock(mtx);
    std::cout << "Worker: Waiting for signal..." << std::endl;
    cv.wait(lock, [] { return ready; });  // 等待条件变量满足。唤醒时会对ready进行检查，当ready为false时，继续阻塞，防止虚假唤醒。
    std::cout << "Worker: Got the signal!" << std::endl;
}

void setter()
{
    std::this_thread::sleep_for(std::chrono::seconds(2));  // 模拟耗时操作
    {
        std::lock_guard<std::mutex> lock(mtx);
        ready = true;  // 设置条件变量为满足状态
        cv.notify_one();  // 发送信号通知等待的线程
    }
}

int main()
{
    std::thread t1(worker);
    std::thread t2(setter);

    t1.join();
    t2.join();

    return 0;
}
```
```c++
如果：ready = false;
那么：cv.wait(lock, [] { return ready; });  // 线程被唤醒时检查ready, 如果为false，则继续阻塞
等价：
while(!ready) {     
    cv.wait(lock);                         // 线程被唤醒时检查ready, 如果为false，则继续阻塞
}
```

## 1.13 连接数据库的方法
    1. MySQL C API
   
    2. MySQL Connector/C++：这是MySQL官方提供的C++连接器库，也称为MySQL Connector/C++。它是使用MySQL C API构建的面向C++的API，提供了连接、执行SQL查询、处理结果等功能。你可以使用MySQL Connector/C++库来连接和操作MySQL数据库。

    3. ORM（对象关系映射）库：ORM库是一种将对象和数据库之间进行映射的工具，它提供了高级的数据库操作接口，使得在C++中连接和操作数据库更加方便。一些流行的C++ ORM库包括MySQL++、ODB、cppdb等。

    4. ODBC（Open Database Connectivity）：ODBC是一种开放标准的数据库访问接口，它提供了与多种数据库系统进行通信的能力。通过使用ODBC驱动程序，你可以在C++中使用ODBC API来连接和操作MySQL数据库。

    5. 第三方库和框架：除了上述提到的方式，还有许多第三方库和框架可用于连接MySQL数据库。例如，Boost提供了一些与数据库交互的组件，如Boost.Asio和Boost.Beast；Poco库提供了易于使用的数据库接口；Qt框架具有Qt SQL模块等

## 1.14 通过 MySQL C API 与MySQL交互
### （1）MySQL C API
    1. 连接管理函数：
    mysql_init：初始化MySQL对象。
    mysql_real_connect：连接到MySQL服务器。
    mysql_close：关闭与MySQL服务器的连接。

    2. 执行SQL语句的函数：
    mysql_query：执行SQL查询。
    mysql_real_query：执行SQL查询（支持多条SQL语句）。
    mysql_store_result：获取查询结果。
    mysql_use_result：逐行获取查询结果。
    mysql_fetch_row：获取查询结果的下一行数据。
    mysql_free_result：释放查询结果内存。
    
    3. 事务处理函数：
    mysql_autocommit：设置是否自动提交事务。
    mysql_commit：提交当前事务。
    mysql_rollback：回滚当前事务。
    
    4. 数据操作函数：
    mysql_real_escape_string：对字符串进行转义，防止SQL注入。
    mysql_insert_id：获取上一次插入操作生成的自增ID。
    mysql_affected_rows：获取上一次操作受影响的行数。
    
    5. 错误处理函数：
    mysql_errno：获取最近一次MySQL函数调用的错误代码。
    mysql_error：获取最近一次MySQL函数调用的错误描述。

### （2）连接和增删改查
```c++
// mytest.cpp

#include <mysql/mysql.h>
#include <iostream>
#include <string>

// 3. 查询数据
void select(MYSQL *conn) {
    if (mysql_query(conn, "select * from user") != 0) {
        std::cout << "failed to fetch data: " << mysql_error(conn) << std::endl;
    } else {
        MYSQL_RES *result = mysql_store_result(conn);
        if (result == NULL) {
            std::cout << "failed to get result set: " << mysql_error(conn) << std::endl;
        } else {
            MYSQL_ROW row;
            std::cout << "name\tpasswd" << std::endl;
            while ((row = mysql_fetch_row(result)) != NULL) {
                std::cout << row[0] << "\t" << row[1] << std::endl;
            }
            mysql_free_result(result);
        }
    }
}

int main() {
    std::string localhost = "127.0.0.1";
    std::string username  = "root";
    std::string password  = "258088";
    std::string database = "yourdb";

    // 1. 初始化
    MYSQL *conn = mysql_init(NULL);
    if (conn == NULL) {
        std::cout << "failed to initialize" << std::endl;
        return 1;
    }

    // 2. 连接数据库
    if (mysql_real_connect(conn, localhost.c_str(), username.c_str(), password.c_str(), database.c_str(), 0, NULL, 0) == NULL) {
        std::cout << "failed to connect: " << mysql_error(conn) << std::endl;
        mysql_close(conn);
        return 1;
    }

    // 3. 查询数据
    select(conn);

    // 4. 插入数据
    if (mysql_query(conn, "insert into user (username, passwd) values ('test1', '20')") != 0) {
        std::cout << "failed to insert data: " << mysql_error(conn) << std::endl;
    } else {
        std::cout << "inserted successfully" << std::endl;
    }
    
    select(conn);

    // 5. 更新数据
    if (mysql_query(conn, "update user set passwd = '123456' where username = 'test1'") != 0) {
        std::cout << "failed to update data: " << mysql_error(conn) << std::endl;
    } else {
        std::cout << "updated successfully" << std::endl;
    }

    select(conn);

    // 6. 删除数据
    if (mysql_query(conn, "delete from user where username = 'test1'") != 0) {
        std::cout << "failed to delete data: " << mysql_error(conn) << std::endl;
    } else {
        std::cout << "data deleted successfully" << std::endl;
    }

    select(conn);

    // 7. 关闭连接
    mysql_close(conn);
    return 0;
}
```
```bash
# 编译时需要链接库
g++ mytest.cpp -o mytest -lpthread -lmysqlclient && ./mytest
```

## 1.15 防止头文件重复包含
### （1）介绍
在 C++ 中，可以使用头文件保护（header guards）来防止头文件的多重包含。头文件保护使用预处理指令来检查宏是否已定义，并根据定义情况执行不同的代码。
### （2）示例
```c++
#ifndef MYHEADER_H
#define MYHEADER_H

// 在这里写下你的头文件内容

#endif

当你在其他源文件中第一次包含 myheader.h 时，MYHEADER_H 宏将被定义并且条件为真，因此头文件的内容将被包含在编译过程中。
当其他源文件再次包含相同的头文件时，预处理器会检测到 MYHEADER_H 宏已经定义，因此条件为假，头文件的内容将被跳过，避免了多次包含同一个头文件的问题。
这样做的好处是，无论你在多个源文件中多次包含该头文件，编译器只会处理一次头文件的内容，避免了重复定义和编译错误。
```

## 1.16 创建线程
### （1）C
#### 用法
```c++
// 1. 创建一个新的线程
int pthread_create(pthread_t *thread, const pthread_attr_t *attr, void *(*start_routine)(void*), void *arg)
参数：
    thread：指向 pthread_t 类型的变量，用于存储新线程的标识符
    attr：指向 pthread_attr_t 类型的变量，用于设置线程的属性，通常使用 NULL 以使用默认属性
    start_routine：指向线程函数的指针，该函数接受一个 void* 类型的参数并返回一个 void* 类型的值
    arg：传递给线程函数的参数。
返回：若成功创建线程，则返回 0；否则返回一个非零错误码。

// 2. 终止当前线程，并返回一个指向 value_ptr 的指针作为线程的退出状态
void pthread_exit(void *value_ptr)
参数：value_ptr 是一个指针，用于传递线程的退出状态
注意：调用 pthread_exit() 将立即终止线程的执行，不会等待其他线程完成

// 3. 等待指定的线程结束，并获取其退出状态
int pthread_join(pthread_t thread, void **value_ptr)
参数：
    thread：要等待的线程的标识符
    value_ptr：指向指针的指针，用于接收线程的退出状态
返回：若成功等待线程并获取退出状态，则返回 0；否则返回一个非零错误码

// 4. 请求取消指定的线程
int pthread_cancel(pthread_t thread)
参数：thread 是要取消的线程的标识符
返回：若成功发送取消请求，则返回 0；否则返回一个非零错误码

// 5. 将指定的线程标记为可分离的状态，使其在结束时自动释放资源
int pthread_detach(pthread_t thread)
参数：thread 是要标记为可分离的线程的标识符
返回：若成功将线程标记为可分离状态，则返回 0；否则返回一个非零错误码

// 6. 返回调用线程的标识符
pthread_t pthread_self(void)
返回：当前线程的标识符

```

#### 示例
```c++
#include <iostream>
#include <pthread.h>

// 线程函数，打印消息
void* printMessage(void* arg) {
    std::string message = static_cast<char*>(arg);
    std::cout << "Thread ID: " << pthread_self() << " Message: " << message << std::endl;
    pthread_exit(NULL);
}

int main() {
    pthread_t thread1, thread2;
    const char* message1 = "Hello from Thread 1";
    const char* message2 = "Hello from Thread 2";

    // 创建线程1，并执行线程函数printMessage
    if (pthread_create(&thread1, NULL, printMessage, (void*)message1) != 0) {
        std::cerr << "Failed to create thread 1" << std::endl;
        return 1;
    }

    // 创建线程2，并执行线程函数printMessage
    if (pthread_create(&thread2, NULL, printMessage, (void*)message2) != 0) {
        std::cerr << "Failed to create thread 2" << std::endl;
        return 1;
    }

    // 等待线程1和线程2执行完成
    if (pthread_join(thread1, NULL) != 0) {
        std::cerr << "Failed to join thread 1" << std::endl;
        return 1;
    }

    if (pthread_join(thread2, NULL) != 0) {
        std::cerr << "Failed to join thread 2" << std::endl;
        return 1;
    }

    return 0;
}
```
### （2）C++
#### 用法
```c++
// 1. 创建一个新的线程，并执行指定的函数
std::thread::thread()
参数：
    f：要在新线程中执行的函数（可以是函数指针、函数对象、Lambda 表达式等）
    args：传递给函数的参数。
例子：
    void myFunction(int arg) {
        // 线程执行的函数体
    }
    std::thread myThread(myFunction, 42); // 创建一个新线程，执行 myFunction(42)

// 2. 等待线程的执行完成
std::thread::join()
例子：
    std::thread myThread(myFunction, 42);
    myThread.join(); // 将 myThread 线程与当前线程分离

// 3. 将线程与当前线程分离，使其成为独立执行的线程
std::thread::detach()
例子：
    std::thread myThread(myFunction, 42);
    myThread.detach(); // 将 myThread 线程与当前线程分离

// 4. 获取当前线程的唯一标识符。
std::this_thread::get_id()
返回：std::thread::id 类型的线程标识符
例子：std::thread::id threadId = std::this_thread::get_id();

// 5. 使当前线程休眠指定的时间
std::this_thread::sleep_for()
参数：sleep_duration 是一个 std::chrono::duration 类型的时间间隔
例子：std::this_thread::sleep_for(std::chrono::seconds(5)); // 休眠 5 秒
```
#### 示例
```c++
#include <iostream>
#include <thread>

// 线程函数，打印消息
void printMessage(const std::string& message) {
    std::cout << "Thread ID: " << std::this_thread::get_id() << " Message: " << message << std::endl;
}

int main() {
    // 创建两个线程并执行线程函数
    std::thread thread1(printMessage, "Hello from Thread 1");
    std::thread thread2(printMessage, "Hello from Thread 2");

    // 等待线程执行完成
    thread1.join();
    thread2.join();

    return 0;
}
```

# 2 功能模块
## 2.1 线程池
### （1）介绍
    目的：
    线程池的主要目的是解决线程创建和销毁的开销问题，以及避免创建过多的线程导致系统资源耗尽的风险。通过预先创建一定数量的线程，并将任务分配给这些线程执行，线程池可以有效地管理线程的生命周期和资源消耗。

    组成：
    1. 线程池管理器（ThreadPool Manager）：负责创建、销毁和管理线程池中的线程。
    2. 工作队列（Work Queue）：用于存储待执行的任务，线程池中的线程会从工作队列中获取任务进行处理。
    3. 线程池（Thread Pool）：包含一定数量的线程，用于执行工作队列中的任务。
    4. 任务（Task）：需要执行的具体工作单元，可以是函数、方法或其他可执行的代码块。
    
    优点：
    5. 提高性能和响应性：线程池可以减少线程创建和销毁的开销，使得线程可以立即执行任务，提高系统的响应速度。
    6. 资源管理：线程池可以限制线程的数量，避免创建过多的线程，从而避免系统资源的浪费和耗尽。
    7. 提供线程复用和可管理性：线程池可以重复利用线程来执行多个任务，提高线程的复用率，并提供统一的管理和监控机制。
    8. 控制并发度：通过控制线程池中的线程数量和任务队列的大小，可以控制系统的并发度，防止系统被过多的任务拖垮。
### （2）示例
#### 示例1
```c++
// threadpool.h

#include<iostream>
#include<thread>
#include<atomic>
#include<mutex>
#include<condition_variable>
#include<chrono>
#include<functional>
#include<vector>
#include<queue>

using ThreadTask = std::function<void()>;

class ThreadPool
{
private:
    int max_threads;
    int max_tasks;
    std::vector<std::thread> threads;
    std::queue<ThreadTask> tasks;
    std::mutex tasks_guard;
    bool is_running;
    std::condition_variable tasks_event;

public:
    ThreadPool(int threads_num = std::thread::hardware_concurrency(), int tasks_num = -1) {
        if(threads_num < 1) {
            max_threads = 1;
        } else {
            max_threads = threads_num;
        }
        max_tasks = tasks_num;
        is_running = false;
    }

    ~ThreadPool(){WaitForStop();}

    // 添加一个任务
    bool AddTask(ThreadTask task) {
        // 加锁
        {
            std::unique_lock<std::mutex> lock(tasks_guard);
            if(max_tasks == -1) {
                tasks.push(task);
            } else {
                if(tasks.size() >= max_tasks) {
                    return false;
                } else {
                    tasks.push(task);
                }
            }
        }
        // 唤醒一个线程
        tasks_event.notify_one();
        return true;
    }

    // 启动线程池
    bool Start() {
        if(is_running) {
            return false;
        } 
        is_running = true;
        if(threads.empty()) {
            CreateThreads();
        }
        return true;
    }

    // 停止所有线程
    void WaitForStop() {
        if(!is_running) {
            return;
        }
        is_running = false;
        tasks_event.notify_all();
        for(auto &t:threads) {
            t.join();
        }
        threads.clear();
    }

private:
    // 创建线程
    void CreateThreads() {
        for(int i = 0; i < max_threads; i++) {
            threads.push_back(std::thread(&ThreadPool::ThreadRoutine, this));
        } 
    }

    // 工作线程:从任务队列中取一个运行
    static void ThreadRoutine(ThreadPool *ptr) {
        if(ptr == nullptr) {
            return;
        }
        while(ptr->is_running || !ptr->tasks.empty()) {
            ThreadTask task;
            // 加锁
            {
                std::unique_lock<std::mutex> lock(ptr->tasks_guard);
                ptr->tasks_event.wait(lock, [&](){return !ptr->tasks.empty();});
                task = ptr->tasks.front();
                ptr->tasks.pop();
            }
            task();
        }
    }
};

```
```c++
// example.cpp

#include "threadpool.h"

int product_sell = 0;
void ProductCounter(std::mutex* task_protect)
{
    std::this_thread::sleep_for(std::chrono::seconds(1));

    std::lock_guard<std::mutex> lock(*task_protect);
    std::cout <<"How many products sell: " << product_sell++ << std::endl;
}

int main()
{
    std::mutex protect_task;
    ThreadPool pool(20, -1);
    for(int i = 0; i < 100; i++)
    {
        pool.AddTask(std::bind(ProductCounter, &protect_task));
    }
    pool.Start();
    // Do more stuff...
    for(int i = 0; i < 50; i++)
    {
        pool.AddTask(std::bind(ProductCounter, &protect_task));
    }
    pool.WaitForStop();
    return 0;
}
```
#### 示例2 (webserver)：
```c++
#ifndef THREADPOOL_H
#define THREADPOOL_H

#include <list>
#include <cstdio>
#include <exception>
#include <pthread.h>
#include "../lock/locker.h"
#include "../CGImysql/sql_connection_pool.h"

template <typename T>
class threadpool
{
public:
    /*thread_number是线程池中线程的数量，max_requests是请求队列中最多允许的、等待处理的请求的数量*/
    threadpool(int actor_model, connection_pool *connPool, int thread_number = 8, int max_request = 10000);
    ~threadpool();
    bool append(T *request, int state);
    bool append_p(T *request);

private:
    /*工作线程运行的函数，它不断从工作队列中取出任务并执行之*/
    static void *worker(void *arg);
    void run();

private:
    int m_thread_number;        //线程池中的线程数
    int m_max_requests;         //请求队列中允许的最大请求数
    pthread_t *m_threads;       //描述线程池的数组，其大小为m_thread_number
    std::list<T *> m_workqueue; //请求队列
    locker m_queuelocker;       //保护请求队列的互斥锁
    sem m_queuestat;            //是否有任务需要处理
    connection_pool *m_connPool;  //数据库
    int m_actor_model;          //模型切换
};
template <typename T>
threadpool<T>::threadpool( int actor_model, connection_pool *connPool, int thread_number, int max_requests) : m_actor_model(actor_model),m_thread_number(thread_number), m_max_requests(max_requests), m_threads(NULL),m_connPool(connPool)
{
    if (thread_number <= 0 || max_requests <= 0)
        throw std::exception();
    m_threads = new pthread_t[m_thread_number];
    if (!m_threads)
        throw std::exception();
    for (int i = 0; i < thread_number; ++i)
    {
        if (pthread_create(m_threads + i, NULL, worker, this) != 0)
        {
            delete[] m_threads;
            throw std::exception();
        }
        if (pthread_detach(m_threads[i]))
        {
            delete[] m_threads;
            throw std::exception();
        }
    }
}
template <typename T>
threadpool<T>::~threadpool()
{
    delete[] m_threads;
}
template <typename T>
bool threadpool<T>::append(T *request, int state)
{
    m_queuelocker.lock();
    if (m_workqueue.size() >= m_max_requests)
    {
        m_queuelocker.unlock();
        return false;
    }
    request->m_state = state;
    m_workqueue.push_back(request);
    m_queuelocker.unlock();
    m_queuestat.post();
    return true;
}
template <typename T>
bool threadpool<T>::append_p(T *request)
{
    m_queuelocker.lock();
    if (m_workqueue.size() >= m_max_requests)
    {
        m_queuelocker.unlock();
        return false;
    }
    m_workqueue.push_back(request);
    m_queuelocker.unlock();
    m_queuestat.post();
    return true;
}
template <typename T>
void *threadpool<T>::worker(void *arg)
{
    threadpool *pool = (threadpool *)arg;
    pool->run();
    return pool;
}
template <typename T>
void threadpool<T>::run()
{
    while (true)
    {
        m_queuestat.wait();
        m_queuelocker.lock();
        if (m_workqueue.empty())
        {
            m_queuelocker.unlock();
            continue;
        }
        T *request = m_workqueue.front();
        m_workqueue.pop_front();
        m_queuelocker.unlock();
        if (!request)
            continue;
        if (1 == m_actor_model)
        {
            if (0 == request->m_state)
            {
                if (request->read_once())
                {
                    request->improv = 1;
                    connectionRAII mysqlcon(&request->mysql, m_connPool);
                    request->process();
                }
                else
                {
                    request->improv = 1;
                    request->timer_flag = 1;
                }
            }
            else
            {
                if (request->write())
                {
                    request->improv = 1;
                }
                else
                {
                    request->improv = 1;
                    request->timer_flag = 1;
                }
            }
        }
        else
        {
            connectionRAII mysqlcon(&request->mysql, m_connPool);
            request->process();
        }
    }
}
#endif
```
## 2.2 数据库连接池
### （1）介绍
    数据库连接池是一个管理数据库连接的软件组件或模块，它允许应用程序在需要时从预先创建的一组数据库连接中获取连接，并在使用完毕后将其返回给连接池，
    而不是频繁地创建和销毁数据库连接。连接池的目的是提高数据库访问的性能和效率。
### （2）示例 (webserver)
#### sql_connection.h
```c++
#ifndef _CONNECTION_POOL_
#define _CONNECTION_POOL_

#include <stdio.h>
#include <list>
#include <mysql/mysql.h>
#include <error.h>
#include <string.h>
#include <iostream>
#include <string>
#include "../lock/locker.h"
#include "../log/log.h"

using namespace std;

class connection_pool
{
public:
	MYSQL *GetConnection();				 //获取数据库连接
	bool ReleaseConnection(MYSQL *conn); //释放连接
	int GetFreeConn();					 //获取连接
	void DestroyPool();					 //销毁所有连接

	//单例模式
	static connection_pool *GetInstance();

	void init(string url, string User, string PassWord, string DataBaseName, int Port, int MaxConn, int close_log); 

private:
	connection_pool();
	~connection_pool();

	int m_MaxConn;  //最大连接数
	int m_CurConn;  //当前已使用的连接数
	int m_FreeConn; //当前空闲的连接数
	locker lock;
	list<MYSQL *> connList; //连接池
	sem reserve;

public:
	string m_url;			//主机地址
	string m_Port;		 	//数据库端口号
	string m_User;		 	//登陆数据库用户名
	string m_PassWord;	 	//登陆数据库密码
	string m_DatabaseName; 	//使用数据库名
	int m_close_log;		//日志开关
};

class connectionRAII
{
public:
	connectionRAII(MYSQL **con, connection_pool *connPool);
	~connectionRAII();	
	
private:
	MYSQL *conRAII;
	connection_pool *poolRAII;
};

#endif
```
#### sql_connection.cpp
```c++
#include <mysql/mysql.h>
#include <stdio.h>
#include <string>
#include <string.h>
#include <stdlib.h>
#include <list>
#include <pthread.h>
#include <iostream>
#include "sql_connection_pool.h"

using namespace std;

connection_pool::connection_pool()
{
	m_CurConn = 0;
	m_FreeConn = 0;
}

connection_pool *connection_pool::GetInstance()
{
	static connection_pool connPool;
	return &connPool;
}

// 构造初始化：连接数据库，创建一定数量的连接，并初始化信号量
void connection_pool::init(string url, string User, string PassWord, string DBName, int Port, int MaxConn, int close_log)
{
	m_url = url;
	m_Port = Port;
	m_User = User;
	m_PassWord = PassWord;
	m_DatabaseName = DBName;
	m_close_log = close_log;

	for (int i = 0; i < MaxConn; i++)
	{
		MYSQL *con = NULL;
		con = mysql_init(con);

		if (con == NULL)
		{
			LOG_ERROR("MySQL Error");
			exit(1);
		}
		con = mysql_real_connect(con, url.c_str(), User.c_str(), PassWord.c_str(), DBName.c_str(), Port, NULL, 0);

		if (con == NULL)
		{
			LOG_ERROR("MySQL Error");
			exit(1);
		}
		connList.push_back(con);
		++m_FreeConn;
	}

	reserve = sem(m_FreeConn);

	m_MaxConn = m_FreeConn;
}


// 当有请求时，从数据库连接池中返回一个可用连接，更新使用和空闲连接数
MYSQL *connection_pool::GetConnection()
{
	MYSQL *con = NULL;

	if (0 == connList.size())
		return NULL;

	reserve.wait();
	
	lock.lock();

	con = connList.front();
	connList.pop_front();

	--m_FreeConn;
	++m_CurConn;

	lock.unlock();
	return con;
}

// 释放当前使用的连接
bool connection_pool::ReleaseConnection(MYSQL *con)
{
	if (con == NULL)
		return false;

	lock.lock();

	connList.push_back(con);
	++m_FreeConn;
	--m_CurConn;

	lock.unlock();

	reserve.post();
	return true;
}

// 销毁数据库连接池
void connection_pool::DestroyPool()
{

	lock.lock();
	if (connList.size() > 0)
	{
		list<MYSQL *>::iterator it;
		for (it = connList.begin(); it != connList.end(); ++it)
		{
			MYSQL *con = *it;
			mysql_close(con);
		}
		m_CurConn = 0;
		m_FreeConn = 0;
		connList.clear();
	}

	lock.unlock();
}

// 当前空闲的连接数
int connection_pool::GetFreeConn()
{
	return this->m_FreeConn;
}

connection_pool::~connection_pool()
{
	DestroyPool();
}

// 资源获取即初始化
connectionRAII::connectionRAII(MYSQL **SQL, connection_pool *connPool){
	*SQL = connPool->GetConnection();
	
	conRAII = *SQL;
	poolRAII = connPool;
}

connectionRAII::~connectionRAII(){
	poolRAII->ReleaseConnection(conRAII);
}
```

## 2.3 多路复用