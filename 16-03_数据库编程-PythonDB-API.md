## 16.3 PythonDB—API

Python DB-API规范涉及到三种不同的角色：Python官方、开发人员和数据库厂商。  
1：为开发人员提供一致API  
2：为数据库厂商提供接口的实现  
3：为Python官方制定Python DB-API规范  

### 16.3.1 建立数据库连接

connect(parameters…)函数  

例如：

    connection = pymysql.connect(host=’localhost’,
                                 user=’root’,
                                 password=’admin’,
                                 database=’mydb’,
                                 charset=’utf8’)

    pymysql.connect()函数中常用的连接参数有：
    host：数据库主机名或IP地址
    port：连接数据库端口号
    user：访问数据库账号
    password或passwd：访问数据库密码
    database或db：数据库中的库名
    charset：数据库编码格式

### 16.3.2 PythonDB-创建游标

Cursor游标暂时保存了SQL操作所影响到的数据。  
游标Cursor对象有很多方法和属性，其中基本SQL操作方法如下：  

* execute(operation[, parameters])

执行一条SQL语句，operation是SQL语句。  
parameters是为SQL提供的参数，可以是序列或字典类型。  
返回值是整数，表示执行SQL语句影响的行数。  

* executemany(operation[, seq_of_params])

执行批量SQL语句，operation是SQL语句。  
seq_of_params是为SQL提供的参数，seq_of_params是序列。  
返回值是整数，表示执行SQL语句影响的行数。    

* callproc(procname[ , parameters])

执行存储过程，procname是存储过程名，parameters是为存储过程提供参数。
  
相关提取方法如下：  

* fetchone()

从结果集中返回一条记录的序列，如果没有数据返回None。  

* fetchmany([size=cursor.arraysize])。  

从结果集返回小于等于size记录数序列，如果没有数据返回空序列，size默认情况下是整个游标的行数。  

* fetchall()  

从结果集返回所有数据。
