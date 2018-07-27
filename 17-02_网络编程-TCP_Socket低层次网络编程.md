## 17.2 TCP Socket低层次网络编程

TCP/IP协议的传输层又有两种传输协议：TCP（传输控制协议）和UDP（用户数据报协议）。  
TCP是面向连接的可靠数据传输协议。  
TCP就像打电话，电话接通后双方才能通话，在挂断电话之前，电话一直占线。  
好处：传输稳定，数据安全。  
缺点：会要求线路一直被占用。具体体现位端口号会一致被占用，端口号类似于电话号码。  

### 17.2.1 TCP Socket通信概述

在客户端和服务器端都会有一个“Socket”对象，Socket可以被比喻为进行网络通信的一个小港口。具体而言，它的数据是双向的，Socket底层会有“流”这种对象，通过流来实现数据的传入和传出，可以把流比喻和想象为海洋中的船。流是一个载体  

### 17.2.2 TCP Socket通信过程

服务器端的工作：  

1：服务器会先启动，并处于等待状态（绑定IP和 端口）。  
2：一直处于监听状态（监听端口）。  
3：等待客户端连接请求。  
4：接收请求并建立连接。  
5：向客户端发送数据或从客户端接收数据。  
6：关闭Socket释放资源  

客户端的工作：  

1：指定服务器IP和端口  
2：向服务器发出连接请求并与服务器建立连接。  
3：从服务器接收数据或向服务器发送数据。  
4：关闭Socket释放资源  

### 17.2.3 TCP Socket编程API

Python提供了两个Socket模块：  
* Socket
* Socket Server

1：创建TCP Socket  

    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

参数：  
* socket.AF_INET —— 设置IP地址类型是IPv4 
* socket.AF_INET ——设置IP地址类型是IPv6
* socket.SOCK_STREAM —— 设置Socket通信类型是TCP。

2：TCP Socket服务器编程方法  

    socket.bind(address)

绑定地址和端口，address是包含主机名（或IP地址）和端口的二元组对象。  

    socket.listen(backlog)

监听端口，backlog最大连接数，backlog默认值是1。
	
    socket.accept()

等待客户端连接，连接成功返回二元组对象（conn, address），其中conn是新的socket对象，可以用来接收和发送数据，address是客户端的地址。

3：客户端socket方法

    socket.connect(address)

连接服务器socket，address是包含主机名（或IP地址）和端口的二元组对象。

4：服务器和客户端编程socket共用方法

    socket.recv(buffsize)

接受TCP Socket数据，该方法返回字节序列对象。  
参数buffsize指定一次接收的最大字节数，因此如果要接收的数据量大于buffsize，则需要多次调用该方法进行接收。  

    socket.send(bytes)

发送TCP Socket数据，将bytes数据发送到远程Socket，返回成功发送的字节数。  
如果要发送的数据量很大，需要多次调用该方法发送数据。  

    socket.sendall(bytes)

发送TCP Socket数据，将bytes数据发送到远程Socket，如果发送成功返回None，如果发送失败则抛出异常。  
与socket.send(bytes)不同的是，该方法连续发送数据，直到发送完所有数据或发生异常。  

    socket.settimeout(timeout)

设置socket超时时间，timeout是一个浮点数，单位是秒，值为None则表示永远不会超时。一般超时时间应在刚创建Socket时设置。  

    Socket.close()

关闭Socket。该方法虽然可以释放资源，但不一定立即关闭连接，如果要及时关闭连接，需要在调用该方法之前调用shutdown()方法。  

### 17.2.4 案例：简单聊天工具

服务器端程序：

    import socket

    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.bind(('', 8888)) # 绑定本节IP，端口8888
    s.listen()    # 监听8888端口
    print('服务器端启动...')

    # 等待客户端
    conn, address = s.accept()
    # 客户端连接成功
    print(address)

    # 开始通信
    data = conn.recv(1024)
    print('从客户端接收的消息：{0}'.format(data.decode()))
    # 给客户端发送数据
    conn.send('你好'.encode())   # b'你好'

    # 释放资源
    conn.close()
    s.close()

客户端程序：

    import socket

    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

    # 连接服务器端
    s.connect(('127.0.0.1', 8888))
    print('连接建立...')
    # 给服务器端发送数据
    s.send(b'Hello')
    # 从服务器端接收数据
    data = s.recv(1024)
    print('从服务器端接收消息：{0}'.format(data.decode()))

    # 释放资源
    s.close()

服务器端的运行结果：

    服务器端启动...
    ('127.0.0.1', 54821)
    从客户端接收的消息：Hello

客户端运行结果：

    连接建立...
    从服务器端接收消息：你好

### 17.2.5 案例：文件上传工具

服务器端程序：

    import socket

    HOST = ''
    PORT = 7776

    f_name = r'home.JPG'

    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.bind((HOST, PORT))
        s.listen(10)
        print('服务器启动...')

        while True:
            with s.accept()[0] as conn:
                buffer = []
                while True:   # 反复接收客户端数据
                    data = conn.recv(1024)
                    if data:
                        # 接收的数据添加到缓冲区
                        buffer.append(data)
                    else:
                        break
                b = bytes()   # 创建了一个空的bytes对象
                b = b.join(buffer)

                with open(f_name, 'wb') as f:
                    f.write(b)

                print('服务器接收完成。')

客户端程序：

    import socket

    HOST = '127.0.0.1'
    PORT = 7776

    f_name = 'IMG_1227.JPG'

    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.connect((HOST, PORT))

        with open(f_name, 'rb') as f:
            b = f.read()
            s.sendall(b)

        print('客户端数据发送完成...')

服务器端运行结果：

    服务器启动...
    服务器接收完成。

客户端运行结果：

    客户端数据发送完成...

