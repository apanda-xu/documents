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
## 1.3  

# 2 功能模块
## 2.1 线程池
## 2.2 多路复用