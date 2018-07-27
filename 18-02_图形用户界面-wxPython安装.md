## 18.2 wxPython安装

1：Windows和macOS平台安装：  

    pip install –U wxPython

其中install是按装软件包，-U是将指定软件包升级到最新版本。

例如：  

    Microsoft Windows [版本 10.0.16299.371]
    (c) 2017 Microsoft Corporation。保留所有权利。

    C:\Users\Marshal>pip install -U wxPython
    Collecting wxPython
      Downloading https://files.pythonhosted.org/packages/6d/90/4a64990c564d02b1934c666f859d07a9a15abc5c59cfdd1606860421fe16/wxPython-4.0.3-cp36-cp36m-win_amd64.whl (22.9MB)
    100% |████████████████████████████████| 22.9MB 32kB/s
    Collecting PyPubSub (from wxPython)
      Retrying (Retry(total=4, connect=None, read=None, redirect=None, status=None)) after connection broken by 'ProtocolError('Connection aborted.', ConnectionResetError(10054, '远程主机强迫关闭了一个现有的连接。', None, 10054, None))': /simple/pypubsub/
      Downloading https://files.pythonhosted.org/packages/ab/9e/3b50915d3346971aaa49074425788598ee4907e67c097e013f1a862bd45c/Pypubsub-4.0.0-py3-none-any.whl (63kB)
    100% |████████████████████████████████| 71kB 40kB/s
    Requirement already up-to-date: six in c:\users\marshal\anaconda3\lib\site-packages (from wxPython)
    Installing collected packages: PyPubSub, wxPython
    Successfully installed PyPubSub-4.0.0 wxPython-4.0.3
    You are using pip version 9.0.1, however version 10.0.1 is available.
    You should consider upgrading via the 'python -m pip install --upgrade pip' command.

    C:\Users\Marshal>

2：Linux平台下使用pip安装有点麻烦，例如在Ubuntu 16.04安装，打开终端输入如下指令：  

    pip install  -U –f https://extras.wxpython.org/wxPython4/extras/linux/gtk3/ubuntu-16.04\wxPython
   
    -f：到指定网页去解析

3：下载wxPython帮助文档和案例  

    https://extras.wxpython.org/wxPython4/extras

可以到官网去看一下案例，官网地址：

    https://wxpython.org  

wxPython的在线文档地址：

    https://docs.wxpython.org/  

里面有各种控件的说明，非常详细。

