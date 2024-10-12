# 设计模式

## 单例类主流写法

以下是一个使用单例类的代码例子：

```cpp
#include <iostream>
#include <fstream>

class Logger {
public:
    static Logger& getInstance() {
        static Logger instance;
        return instance;
    }

    void log(const std::string& message) {
        file_ << message << std::endl;
    }

private:
    Logger() {
        file_.open("log.txt");
    }

    ~Logger() {
        file_.close();
    }

    std::ofstream file_;
};

int main() {
    Logger& logger = Logger::getInstance();
    logger.log("Program started.");

    // Do some work...
    logger.log("Doing some work...");

    // Log an error
    logger.log("Error occurred!");

    return 0;
}
```

在这个例子中，我们使用单例类 `Logger` 来记录程序的日志。日志被写入文件 `log.txt` 中。

我们使用 `Logger::getInstance()` 来获取单例实例，并使用 `log()` 方法来记录日志。

这个例子展示了单例类的以下优点：

*   只有一个实例：我们只创建了一个 `Logger` 实例，这样可以避免多个实例之间的冲突。
*   全局访问：我们可以从任何地方访问 `Logger` 实例，这样可以方便地记录日志。
*   延迟初始化：我们在需要时才创建 `Logger` 实例，这样可以避免不必要的资源浪费。

## 单例类其他写法

是的，单例类的写法有多种方式。以下是几种常见的单例类写法：

1. 懒汉式单例类：
```cpp
class Singleton {
public:
    static Singleton& getInstance() {
        if (!instance) {
            instance = new Singleton();
        }
        return *instance;
    }

private:
    Singleton() {}
    ~Singleton() {}

    static Singleton* instance;
};

Singleton* Singleton::instance = nullptr;
```

2. 双重检查锁定单例类：
```cpp
class Singleton {
public:
    static Singleton& getInstance() {
        if (!instance) {
            std::lock_guard<std::mutex> lock(mutex);
            if (!instance) {
                instance = new Singleton();
            }
        }
        return *instance;
    }

private:
    Singleton() {}
    ~Singleton() {}

    static Singleton* instance;
    static std::mutex mutex;
};

Singleton* Singleton::instance = nullptr;
std::mutex Singleton::mutex;
```

3. 饿汉式单例类：
```cpp
class Singleton {
public:
    static Singleton& getInstance() {
        return instance;
    }

private:
    Singleton() {}
    ~Singleton() {}

    static Singleton instance;
};

Singleton Singleton::instance;
```

4. 使用 `std::call_once` 实现单例类：
```cpp
class Singleton {
public:
    static Singleton& getInstance() {
        std::call_once(initFlag, [] {
            instance.reset(new Singleton());
        });
        return *instance;
    }

private:
    Singleton() {}
    ~Singleton() {}

    static std::unique_ptr<Singleton> instance;
    static std::once_flag initFlag;
};

std::unique_ptr<Singleton> Singleton::instance;
std::once_flag Singleton::initFlag;
```

这些是常见的单例类写法，每种写法都有其优缺点和使用场景。你可以根据你的需求选择适合的写法。