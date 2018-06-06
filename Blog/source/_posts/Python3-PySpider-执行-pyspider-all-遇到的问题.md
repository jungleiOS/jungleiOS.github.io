---
title: Python3 PySpider 执行 pyspider all 遇到的问题
---
1. Could not create web server listening on port 25555
2. pycurl: libcurl link-time ssl backend (openssl) is different from compile-time ssl backend (none/other)

[参考文献](https://www.zhihu.com/question/267901970)

## netstat、lsof查看端口
### netstat 
netstat用来查看系统当前系统网络状态信息，包括端口，连接情况等，常用方式如下：
* -t : 指明显示TCP端口
* -u : 指明显示UDP端口
* -l : 仅显示监听套接字(LISTEN状态的套接字)
* -p : 显示进程标识符和程序名称，每一个套接字/端口都属于一个程序
* -n : 不进行DNS解析
* -a 显示所有连接的端口

直接查看端口命令。netstat -an | grep 22   
note：22就是改为要查询的端口

### lsof
[参考链接](http://www.cnblogs.com/peida/archive/2013/02/26/2932972.html)
lsof的作用是列出当前系统打开文件(list open files)，不过通过-i参数也能查看端口的连接情况，-i后跟冒号端口可以查看指定端口信息，直接-i是系统当前所有打开的端口
* -a 列出打开文件存在的进程
* -c<进程名> 列出指定进程所打开的文件
* -g  列出GID号进程详情
* -d<文件号> 列出占用该文件号的进程
* +d<目录>  列出目录下被打开的文件
* +D<目录>  递归列出目录下被打开的文件
* -n<目录>  列出使用NFS的文件
* -i<条件>  列出符合条件的进程。（4、6、协议、:端口、 @ip ）
* -p<进程号> 列出指定进程号所打开的文件
* -u  列出UID号进程详情
* -h 显示帮助信息
* -v 显示版本信息

lsof -i:22 #查看22端口连接情况，默认为sshd端口

### 解决第一个问题
1. 使用lsof命令查看那条线程占用了25555端口
2. 执行kill命令杀掉那条线程 如： kill 15889
### 解决第二个问题
1. pip3 uninstall pycurl
2. export PYCURL_SSL_LIBRARY=openssl
3. export LDFLAGS=-L/usr/local/opt/openssl/lib
4. export CPPFLAGS=-I/usr/local/opt/openssl/include
5. pip3 install pycurl --compile --no-cache-dir

执行以上命令行卸载pycurl再重装


