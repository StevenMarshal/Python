## 16.5 NoSQL数据库存储

NoSQL是不是用SQL语句的数据，也是非关系数据库。  
dbm是使用键值对应的数据库。  

### 16.5.1 dbm数据库的打开和关闭

Python内置dbm模块提供了存储dbm数据的API。  

它的语法如下：  

    dbm.open(file, flag=’r’)

flag取值说明如下：
* ‘r’  
以只读方式打开现有数据库，这是默认值。
* ‘w’  
以读写方式打开数据库
* ‘c’  
以读写方式打开数据库，如果数据库不存在则创建。
* ‘n’  
始终创建一个新的空数据库，打开方式为读写。

### 16.5.2 dbm数据存储

dbm数据存储相关语句如下：  

1：写入数据  

    d[key] = data

    其中：  
    d ：是打开的数据库对象
    key：键
    data：保存的数据

如果key不存在则创建key-data数据项，如果key已经存在则使用data覆盖旧数据。  

2：读取数据  

    data = d[key] 或 data = d.get(key, defaultvalue)

使用data = d[key]语句读取数据时，如果没有key对应的数据则会抛出KeyError异常。为了防止这种情况的发生可以使用data = d.get(key, defaultvalue)语句，如果没有key对应的数据，返回默认值defaultvalue。  

3：删除数据  

    del d[key]

按照key删除数据，如果没有key对应的数据则会抛出keyError异常。

4：查找数据  

    flag = key in d  

例如：  

    # coding = utf-8

    import dbm

    with dbm.open('mydb', 'c') as db:
        db['name'] = 'tony'
        """取出的数据实际是一种字符序列（bytes）需要调用decode()方法转换为字符串
           同时产生了三个文件，分别为“mydb.dat”、“mydb.bak”、“mydb.dir”,这几
           个文件就是他的数据文件
        """
        print(db['name'].decode())

        age = int(db.get('age', b'18').decode())  # 转换问18的字节序列

        if 'age' in db:
            db['age'] = '20'

        del db['name']

