## 19.2 使用threading模块

### 19.2.1 使用threading模块

Python中有两个模块可以进行多线程编程，即：\_thread和threading模块。

* \_thread模块提供了多线程编程的低级API，使用起来比较繁琐。
* threading模块提供了多线程编程的高级API，threading基于_thread封装，使用起来比较简单。

threading模块API是面向对象的，其中最重要的是线程类Thread，此外还有很多线程相关函数，这些函数常用的有：  

* threading.active_count()  
返回当前处于活动状态的线程数。
* threading.current_thread()  
返回当前的Thread对象。
* threading.main_thread()
返回主线程对象。

例如：  

    # coding=utf8

    import threading

    # 获得当前线程对象
    t = threading.current_thread()
    # 打印当前线程对象名
    print(t.name)

    # 返回当前活动线程个数
    print(threading.active_count())

    # 当前主线程
    t = threading.main_thread()
    # 主线程名
    print(t.name)


    MainThread
    1
    MainThread

