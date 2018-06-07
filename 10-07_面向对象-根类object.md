## 10.7 Python根类 — object

在Python中所有类的根类是object。  

重点介绍两个方法：
* \_\_str\_\_()。   
返回该对象的字符串表示。
* \_\_eq\_\_(other)。  
指示其他某个对象是否与此对象“相等”。

### 10.7.1	\_\_str\_\_()方法

该方法的目的是使每个对象都可以日志输出该对象描述信息，\_\_str\_\_()方法获得描述信息。  

例如：  

    class Person:

        def __init__(self, name, age):
            self.name = name
            self.age = age

    def info(self):
        temhplate = 'Person [name={0}, age={1}]'
        s = temhplate.format(self.name, self.age)
        return s

    p = Person('小赵', 18)
    print(p)

    <__main__.Person object at 0x051E63D0>

将\_\_str\_\_()方法重写的效果如下：  

    class Person:

        def __init__(self, name, age):
            self.name = name
            self.age = age

        def __str__(self):
            temhplate = 'Person [name={0}, age={1}]'
            s = temhplate.format(self.name, self.age)
            return s

    p = Person('小赵', 18)
    print(p)

    Person [name=小赵, age=18]

### 10.7.2	对象比较方法

当使用运算符“==”进行比较对象时，在对象的内部是通过\_\_eq\_\_()方法进行比较。

例如：  

    class Person:

        def __init__(self, name, age):
            self.name = name
            self.age = age

        def __str__(self):
            temhplate = 'Person [name={0}, age={1}]'
            s = temhplate.format(self.name, self.age)
            return s

        def __eq__(self, other):
            if self.name == other.name and self.age == other.age:
                return True
            else:
                return False

    p1 = Person('小赵', 18)
    p2 = Person('小赵', 18)
    print(p1==p2)

    True

如果没有覆盖\_\_eq\_\_()方法，则：

    class Person:

        def __init__(self, name, age):
            self.name = name
            self.age = age

        def __str__(self):
            temhplate = 'Person [name={0}, age={1}]'
            s = temhplate.format(self.name, self.age)
            return s

        # def __eq__(self, other):
        #     if self.name == other.name and self.age == other.age:
        #         return True
        #     else:
        #         return False

    p1 = Person('小赵', 18)
    p2 = Person('小赵', 18)
    p3 = p2

    print(p1 == p2)

    print(p2 == p3)
    print(p3 is p2)

    False
    True
    True
