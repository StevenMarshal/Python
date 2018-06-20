## 12.2 random模块

* random.random()

返回在范围大于等于0.0，且小于1.0中的随机浮点数。  

例如：  

    >>> import random
    >>> for i in range(1,10):
	    x = random.random()
	    print(x)
   	
    0.9844726820682896
    0.3483405745091269
    0.1264692159657803
    0.9622432065856956
    0.25657121427719776
    0.4091743632664977
    0.6478546866381357
    0.19567287630009644
    0.5842648793476614

* random.randrange(stop)

返回在范围大于等于0，且小于stop，步长为1随机整数。  

例如：  

    >>> import random
    >>> for i in range(1,10):
	    x = random.randrange(10)
	    print(x)

    5
    1
    8
    2
    7
    3
    8
    2
    3
    >>>

* random.randrange(start, stop[, step])

返回在范围大于等于start，且小于stop，步长为step随机整数。

例如：

    >>> import random
    >>> for i in range(1,10):
	    x = random.randrange(1, 10, 3)
	    print(x)

    1
    4
    4
    4
    7
    1
    7
    4
    7

* random.randint(a, b)

返回在范围大于等于a，且小于等于b之间的随机整数。

例如：

    >>> import random
    >>> for i in range(1,10):
	    x = random.randint(1, 100)
	    print(x)

    40
    96
    77
    75
    19
    13
    39
    97
    76
    >>>
