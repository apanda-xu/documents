# 1 基础知识 
## 1.1 std::unique_lock和std::unique_guard的用法和区别

## 1.2 睡眠
```
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