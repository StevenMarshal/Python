## 17.3.	UDP Socket低层次网络编程

UPD（用户数据报协议）类似于日常生活中的邮件投递，是不能保证可靠的寄到目的地。UDP是无连接的，对系统资源的要求较少，UDP可能丢包不保证数据顺序。但是对于网络游戏和在线视频等要求传输快、实时性高、质量可稍差一点的数据传输，UDP还是非常不错的。

### 17.3.1.	UDP Socket编程API

1：创建UDP Socket
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
与创建TCP Socket对象不同，使用socket类型是socket.SOCK_DGRAM（用户数据报通信协议）。

2：UDP Socket服务器编程方法
Socket对象中与UPD Socket服务器编程有关的方法是bind()方法，注意不需要listen()和accept()，这是因为UDP通信不需要像TCP一样的监听端口，建立连接。

3：服务器和客户端编程socket公用方法
Socket对象中有一些方法是服务器和客户端编程共用方法，这些方法如下：
* socket.recvfrom(bufsize)  
接受UDP Socket数据，该方法返回二元组对象（data, address），data是接收的字节序列对象；address是发送数据的远程Socket地址。参数buffsize指定一次接收的最大字节数，因此如果要接收的数据量大于buffsize，则需要多次调用该方法进行接收。
* socket.sendto(bytes, address)  
发送UDP Socket数据，将bytes数据发送到地址为address远程Socket，返回成功发送的字节数。如果要发送的数据量很大，需要多次调用该方法发送数据。
* socket.settimeout(timeout)  
同TCP Socket。
* socket.close()  
同TCP Socket。

### 17.3.2.	案例：简单聊天工具

服务器端程序：

    import socket

	s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
	s.bind(('', 8888))
	print('UDP服务器启动...')
	
	# 从客户端接收数据
	data, client_address = s.recvfrom(1024)
	print('从客户端接收的消息：{0}'.format(data.decode()))
	s.sendto('你好'.encode(), client_address)
	
	# 释放资源
	s.close()
	
	客户端程序：
	import socket
	
	s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
	
	# 服务器端地址
	server_address = ('127.0.0.1', 8888)
	
	# 给服务器发数据
	s.sendto(b'Hello', server_address)
	# 从服务器端接收数据
	data, _ = s.recvfrom(1024)
	print('从服务器端接收消息：{0}'.format(data.decode()))
	
	# 释放资源
	s.close()

	服务器端运行结果：
	UDP服务器启动...
	从客户端接收的消息：Hello
	
	客户端运行结果：
	从服务器端接收消息：你好

### 17.3.3.	案例：文件上传工具

服务器端程序：

	import socket
	
	HOST = ''
	PORT = 7776
	
	f_name = r'testcopy.txt'
	
	with socket.socket(socket.AF_INET, socket.SOCK_DGRAM) as s:
	    s.bind((HOST, PORT))
	
	    print('服务器启动...')
	
	    while True:
	        buffer = []
	        while True:   # 反复接收客户端数据
	            data, _ = s.recvfrom(1024)
	            if data:
	                flag = data.decode()
	                if flag == 'bye':
	                    break
	                # 接收的数据添加到缓冲区
	                buffer.append(data)
	            else:
	                continue
	        b = bytes()   # 创建了一个空的bytes对象
	        b = b.join(buffer)
	
	        with open(f_name, 'w') as f:
	            f.write(b.decode())
	
	        print('服务器接收完成。')

客户端程序：

	import socket
	
	HOST = '127.0.0.1'
	PORT = 7776
	
	f_name = 'test.txt'
	
	server_address = (HOST, PORT)
	
	with socket.socket(socket.AF_INET, socket.SOCK_DGRAM) as s:
	
	    with open(f_name, 'r') as f:
	        while True:
	            data = f.read(1024)
	            if data:
	                s.sendto(data.encode(), server_address)
	            else:
	                s.sendto(b'bye', server_address)
	                break
	
	    print('客户端数据发送完成...')

