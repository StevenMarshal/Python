## 12.3 datetime模块

### 12.3.1 模块介绍

日期和时间模块主要有两个：time和datetime模块。  

time偏重于底层平台，模块中大多数函数会调用本地平台上的C链接库，因此有些函数运行的结果在不同平台上会有所不同。  

datetime模块对time模块进行了封装，提供了高级API，因此本章重点介绍datetime模块。  

### 12.3.2 datetime模块类

datetime模块提供了几个类：
* datetime：包含日期和时间
* date：只包含日期
* time：只包含时间
* timedelta：计算时间跨度
* tzinfo：时区信息

### 12.3.3 datetime、date和time类

1：datetime类  

一个datetime对象可以表示日期和时间等信息，创建datetime对象可以使用如下构造方法：  

    datetime.datetime(year, month, day, hout = 0, minute = 0, second = 0, microsecond = 0, tzinfo = None)

|参数|	取值范围|	说明|
|----|----------|-------|  
|year|	datetime.MINYEAR≤year≤datetime.MAXYEAR|	datetime.MINYEAR 常量是最小年 datetime.MAXYEAR 常量是最大年|
|month|	1 ≤ month ≤ 12|  ———— |	
|day|	1 ≤ day ≤ 给定年份和月份时，该月的最大天数|	注意闰年 二月份时比较特殊的有29天|
|hour|	0 ≤ hour ≤ 24|————|	
|minute|	0 ≤ minute ≤ 60|————|	
|second|	0 ≤ second ≤ 60|	
microsecond	0 ≤ microsecond ≤ 1000000|	

通过datetime类提供的一些类方法获得datetime对象，这些类方法有：  
* datetime.today()
返回当前本地日期和时间。
* datetime.now(tz=None)
返回本地当前日期和时间。  
如果参数tz为None或未指定，则等同于today()。  
* datetime.utcnow()
返回当前的UTC日期和时间。
* datetime.fromtimestamp(timestamp, tz = None)
返回与UNIX时间戳对应的本地日期和时间。
* datetime.utcfromtimestamp(timestamp)
返回与UNIX时间戳对应的UTC日期和时间。

UTC：协调世界时间，它以原子时为基础，UTC比GMT更加精准。GMT即格林尼治标准时间。  

UNIX时间戳：1970年1月1日00：00：00以来至现在的总秒数。  

例如：  

    >>> import datetime
    >>> dt = datetime.datetime(2018,2,20)
    >>> dt
    datetime.datetime(2018, 2, 20, 0, 0)

    >>> import datetime
    >>> dt = datetime.datetime.today()
    >>> dt
    datetime.datetime(2018, 6, 21, 15, 25, 23, 769829)
    >>>

    >>> import datetime
    >>> dt = datetime.datetime.now()
    >>> dt
    datetime.datetime(2018, 6, 21, 15, 26, 10, 977246)

    >>> import datetime
    >>> dt = datetime.datetime.utcnow()
    >>> dt
    datetime.datetime(2018, 6, 21, 7, 27, 14, 299841)

2：date类  

一个date对象可以表示日期等信息，创建date对象可以使用如下构造方法：  

    datetime.date(year, month, day)

date类提供的一些类方法获得date对象，这些类方法有：

* date.today()

返回当前本地日期

例如：

    >>> import datetime
    >>> dt = datetime.date.today()
    >>> dt
    datetime.date(2018, 6, 21)

* date.fromtimestamp(timestamp)

返回与UNIX时间戳对应的本地日期。  
例如：  

    >>> import datetime
    >>> dt = datetime.date.fromtimestamp(9999999999.9999)
    >>> dt
    datetime.date(2286, 11, 21)

3：time类  

一个time对象可以表示一天中时间信息，创建time对象可以使用如下构造方法：  

    datetime.time(hour = 0, minute = 0, second = 0, microsecond = 0, tzinfo = None)

例如：  

    >>> import datetime
    >>> dt = datetime.time(23, 23, 23)
    >>> dt
    datetime.time(23, 23, 23)

### 12.3.4 时间日期计算

timedelta对象用于计算datetime、date和time对象时间间隔。  

timedelta类构造方法如下：  

    datetime.timedelta(days = 0, seconds = 0, microseconds = 0, milliseconds = 0, minutes = 0, hours = 0, weeks = 0)

例如1：  

    >>> import datetime
    >>> dt = datetime.date.today()
    >>> dt
    datetime.date(2018, 6, 21)
    >>> delta = datetime.timedelta(10)
    >>> dt += delta
    >>> dt
    datetime.date(2018, 7, 1)

例如2：

    >>> import datetime
    >>> dt = datetime.date.today()
    >>> dt
    datetime.date(2018, 6, 21)
    >>> delta = datetime.timedelta(weeks = 5)
    >>> dt += delta
    >>> dt
    datetime.date(2018, 7, 26)

12.3.5.	日期时间格式化和解析

日期格式化：  

使用strftime(format)实例方法。   
datetime、date和time三个类中都有一个实例方法strftime(format)。

日期时间解析：  
使用datetime.strptime(date_string.format)类方法，date和time没有strptime()方法。  

|指令|	含义	示例|
|---|-----------|
|%m|	两位月份表示	01、02、12|
|%y|	两位年份表示	08、18|
|%Y|	四位年份表示	2008、2018|
|%d|	月内中的一天	1、2、3|
|%H|	两位小时表示（24小时制）	00、01、23|
|%I|	两位小时表示（12小时制）	01、02、12|
|%p|	AM或PM区域性设置	AM和PM|
|%M|	两位分钟表示	00、01、59|
|%S|	两位秒表示	00、01、59|
|%f|	以6位数表示微秒	000000、000001、999999|
|%z|	+HHMM或-HHMM形式的UTC偏移	+0000、-0400、+1030，如果没有设置时区为空|
|%Z|	时区名称	UTC、EST、CST，如果没有设置时区为空|

例如1：  

    >>> import datetime
    >>> dt = datetime.date.today()
    >>> dt.strftime('%Y-%m-%d')
    '2018-06-21'
    >>> dt.strftime('%y-%m-%d')
    '18-06-21'

例如2：  

    >>> import datetime
    >>> str_date = '2018-02-28 10:40:26'
    >>> date = datetime.datetime.strptime(str_date, '%Y-%m-%d %H:%M:%S')
    >>> date
    datetime.datetime(2018, 2, 28, 10, 40, 26)

### 12.3.6 时区 
 
datetime和time对象只是单纯地表示本地的日期时间和时间，没有时区信息。  
timezone类，是tzinfo的子类，提供了UTC偏移时区的实现。  

timezone类构造方法如下：  
datetime.timezone(offset, name = None)  
offset —— 偏移量（整数） 

例如：  

    >>> from datetime import datetime, timezone, timedelta
    >>> utc_dt = datetime(2008, 8, 19, 23, 59, 59, tzinfo = timezone.utc)
    >>> utc_dt
    datetime.datetime(2008, 8, 19, 23, 59, 59, tzinfo=datetime.timezone.utc)
    >>> utc_dt.strftime('%Y-%m-%d %H:%M:%S %Z')
    '2008-08-19 23:59:59 UTC'
    >>> utc_dt.strftime('%Y-%m-%d %H:%M:%S %z')
    '2008-08-19 23:59:59 +0000'
    >>> bj_tz = timezone(offset=timedelta(hours=8),name='Asia/Beijing')
    >>> bj_tz
    datetime.timezone(datetime.timedelta(0, 28800), 'Asia/Beijing')
    >>> bj_dt = utc_dt.astimezone(bj_tz)
    >>> bj_dt
    datetime.datetime(2008, 8, 20, 7, 59, 59, tzinfo=datetime.timezone(datetime.timedelta(0, 28800), 'Asia/Beijing'))
