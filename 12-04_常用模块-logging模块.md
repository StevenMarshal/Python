## 12.4 logging日志模块

### 12.4.1 日志级别

Python语言的日志级别一共有五种，这五种级别从上到下由低到高，如果设置了DEBUG级别，debug()函数和其他级别的函数的日志信息都会输出；如果设置了ERROR级别，error()和critical()函数的日志信息会输出。

|日志级别|	日志函数|	说明|
|-------|----------|--------|
|DEBUG|	debug()|	最详细的日志信息，主要用于输出调试信息|
|INFO|	info()|	输出一些关键节点信息，用于确定程序的流程|
|WARNING|	warning()|	一些不期望的事情发生时输出的信息（如磁盘可用空间较低），但是此时程序还是正常运行的|
|ERROR|	error()|	由于一个更严重的问题导致某些功能不能正常运行时日志信息|
|CRITICAL|	critical()|	当发生严重错误，导致应用程序不能继续运行时信息|

例如：  

    import logging

    logging.basicConfig(level=logging.DEBUG)

    logging.debug('debug')
    logging.info('info')
    logging.warning('warning')
    logging.error('error')
    logging.critical('critical')

    DEBUG:root:debug
    INFO:root:info
    WARNING:root:warning
    ERROR:root:error
    CRITICAL:root:critical

当日志级别设置成“warning”后，如下：

    import logging

    logging.basicConfig(level=logging.WARNING)

    logging.debug('debug')
    logging.info('info')
    logging.warning('warning')
    logging.error('error')
    logging.critical('critical')

    WARNING:root:warning
    ERROR:root:error
    CRITICAL:root:critical

### 12.4.2 自定义日志器

根据指定的名字，进行自定义日志器。
可以将函数改成它的方法进行使用。

例如：

    import logging

    logging.basicConfig(level=logging.WARNING)
    logger = logging.getLogger(__name__)

    logger.debug('debug')
    logger.info('info')
    logger.warning('warning')
    logger.error('error')
    logger.critical('critical')

    WARNING:__main__:warning
    ERROR:__main__:error
    CRITICAL:__main__:critical

### 12.4.3 日志信息的格式化

常用的日志信息格式化参数，如下表所示：  

|日志格式参数|	说明|
|-----------|-------|
|%（name）s|	日志器名|
|%（asctime）s|	输出日志事件|
|%（filename）s|	包括路径的文件名|
|%（funcName）s|	函数名|
|%（levelname）s|	日志等级|
|%（processName）s|	进程名|
|%（threadName）s|	线程名|
|%（message）s|	输出的消息|

例如：  

    import logging

    logging.basicConfig(level=logging.WARNING, format = '%(asctime)s-%(threadName)s-%(name)s-%(message)s')
    logger = logging.getLogger(__name__)

    logger.debug('debug')
    logger.info('info')
    logger.warning('warning')
    logger.error('error')
    logger.critical('critical')

    2018-06-23 09:18:59,177-MainThread-__main__-warning
    2018-06-23 09:18:59,191-MainThread-__main__-error
    2018-06-23 09:18:59,191-MainThread-__main__-critical

### 12.4.4 日志重定位  

在日志配置方法中，设置一个日志的写入文件路径，就可将日志信息写入该文件之中，如：  

    import logging

    logging.basicConfig(level=logging.WARNING,
                    format = '%(asctime)s-%(threadName)s-%(name)s-%(message)s',
                    filename = 'test.log')
    logger = logging.getLogger(__name__)

    logger.debug('debug')
    logger.info('info')
    logger.warning('warning')
    logger.error('error')
    logger.critical('critical')
    
在相应的日志文件中，可以查看到相关日志信息：  

    2018-06-23 09:27:40,267-MainThread-__main__-warning
    2018-06-23 09:27:40,267-MainThread-__main__-error
    2018-06-23 09:27:40,267-MainThread-__main__-critical

### 12.4.5 配置文件的使用  

目的是将日志的配置信息提取到配置文件中，这样便于将来的维护和修改。

需要配置：日志器、处理器和格式化器。

配置文件logger.conf代码如下：  

    [loggers] # 配置日志器
    Keys=root,simpleExample # 日志器包含了root和simpleExample

    [logger_root] # 配置根日志器
    level=DEBUG
    handlers = consoleHandler # 日志器对应的处理器

    [logger_simpleExample] # 配置simpleExample日志器
    level=DEBUG
    handlers=fileHandler # 日志器对应的处理器
    qualname=logger1  # 日志器名字

    [handlers]  # 配置处理器
    keys = consoleHandler, fileHandler # 处理器包含了两个

    [handler_consoleHandler] # 配置consoleHandler日志器
    class = StreamHandler
    level = DEBUG
    formatter = simpleFormatter
    args = (sys.stdout,)

    [handler_fileHandler] # 配置fileHandler日志器
    class = FileHandler
    level = DEBUG
    formatter = simpleFormatter
    args=(‘test.log’,’a’)

    [formatters] # 配置格式化器
    keys = simpleFormatter # 日志器包含simpleFormatter

    [formatter_simpleFormatter] # 配置simpleFormatter格式化器
    format = %(asctime)s-%(levelname)s-%(message)s
