# TCP介绍，编程流程
###### TCP 回顾
1. 面向连接的流式协议;可靠、出错重传、且每收到一个数据都要给出相应的确认
2. 通信之前需要建立链接
3. 服务器被动链接，客户端是主动链接

###### TCP 与 UDP 的差异
![[Pasted image 20240521092924.png]]

###### TCP与UDP流程对比
![[Pasted image 20240521093624.png]] 


###### TCP编程流程
**==服务器：==**
1. 创建套接字 socket()
2. 将套接字与服务器网络信息结构体绑定 bind()
3. 将套接字设置为监听状态 listen()
4. 阻塞等待客户端的连接请求 accept()
5. 进行通信 recv()/send()
6. 关闭套接字 close()
**==客户端:==**
1. 创建套接字 socket()
2. 发送客户端连接请求 connect()
3. 进行通信 send()/recv()
4. 关闭套接字 close()

# TCP编程——socket
###### 创建套接字
```
#include <sys/types .h>
#include <sys/socket.h>
int socket(int domain, int type, int protocol);
功能:
	创建一个套接字，返回一个文件描述符
参数:
	domain: 通信域，协议族
		AF_UNIX: 本地通信
		AF_INET: ipv4网络协议
		AF_INET6: ipv6网络协议
		AF_IPACKET: 底层接口
	type: 套接字的类型
		SOCK_STREAM: 流式套接字 (tcp)
		SOCK_DGRAM: 数据报套接字 (udp)
		SOCK_RAW: 原始套接字(用于链路层)
	protocol: 附加协议，如果不需要，设置为0
返回值：
	文件描述符，失败返回-1
```
###### 代码
```cpp
#include <iostream>
#include <sys/socket.h> //socket
#include <sys/types.h>
#include <arpa/inet.h>  //gtons inet_addr
#include <netinet/in.h> //sockaddr_in
#include <unistd.h>     //close，文件描述符
#include <stdlib.h>     //exit
#include <string.h>
#define len 128
using namespace std;
int main ()
{
    int sockfd;
    if ((sockfd=socket(AF_INET,SOCK_STREAM,0))==-1){
        perror("fail to socket");
        exit(1);
    }
    cout<<sockfd<<endl;
    close(sockfd);
    return 0;
}
```


# TCP通信
#### TCP客户端-connect、send、recv
###### connect函数
发送连接请求
```cpp
int connect(int sockfd,
		const struct sockaddr *addr,
		socklen_t len);
/*
功能
	主动跟服务器建立链接
参数:
	bockfd: socket 套接字
	addr：连接的服务器地址结构
	len：地址结构体长度
返回值:
	成功:0，失败: 其他
*/
```

<mark style="background: #FF5582A6;">注意：</mark>
1. connect 建立连接之后不会产生新的套接字
2. 连接成功后才可以开始传输 TCP 数据
3. 头文件: `#include <sys/socket.h>`

###### send函数
发送数据
```cpp
#include <sys/socket.h>
sszie_t send(int sockfd,
			const void* buf,
			size_t len,
			int flags);
/*
功能:
	用于发送数据
参数:
	sockfd:已建立连接的套接字
		客户端：socket函数的返回值
		服务器：accept函数的返回值
	buf:发送数据的地址
	len:发送缓数据的大小(以字节为单位)，就是buf的长度
	flags:套接字标志(常为 0)
		0             阻塞
		MSG_DONTWAIT  非阻塞
返回值:
	成功发送的字节数，失败返回-1

*/		 
```
<mark style="background: #FF5582A6;">注意:</mark>
**不能用 TCP 协议发送0长度的数据包**



###### recv函数
接受数据
```cpp
#incluoe<sys/types.h>
#include <sys/socket.h>
ssize_t recv(int sockfd, 
			 void *buf, 
			 size t len, 
			 int flags);
/*
功能:接收数据
参数:
	sockfd: 文件描述符
		客户端: socket函数的返回值
		服务器: accept函数的返回值
	buf: 保存接收到的数据
	len: buf的长度
	flags: 标志位
		0              阻塞
		MSG DONTWAIT   非阻塞

返回值
	成功:接收的字节数,失败:-1
	如果发送端关闭文件描述符或者关闭进程，则recv函数会返回o
*/
```

###### 客户端示例

**==网络调试助手==**
在windows下使用网络调试助手模拟服务器端
![[Pasted image 20240521102557.png]]

**==代码实现==**

```cpp
#include <iostream>
#include <stdio.h>
#include <sys/socket.h> //socket
#include <sys/types.h>
#include <arpa/inet.h>  //gtons inet_addr
#include <netinet/in.h> //sockaddr_in
#include <unistd.h>     //close，文件描述符
#include <stdlib.h>     //exit
#include <string.h>
#include <sys/socket.h>
#include <sys/stat.h>
#include <fcntl.h>
#define len 128
using namespace std;
int main()
{
    int sockfd;
    if ((sockfd = socket(AF_INET, SOCK_STREAM, 0)) == -1)
    {
        perror("fail to socket");
        exit(1);
    }
    // 客户端发送连接请求
    struct sockaddr_in serveraddr;
    socklen_t addrlen = sizeof(serveraddr);
    serveraddr.sin_family = AF_INET;
    serveraddr.sin_addr.s_addr = inet_addr("172.29.96.1");
    serveraddr.sin_port = htons(8080);

    if (connect(sockfd, (struct sockaddr *)&serveraddr, addrlen) == -1)
    {
        perror("fail to connect");
        exit(1);
    }

    // 发送数据
    char buf[len] = "";
    fgets(buf, len, stdin);
    buf[strlen(buf) - 1] = '\0';
    if (send(sockfd, buf, len, 0) == -1)
    {
        perror("fail to send");
        exit(1);
    }

    char text[len] = "";
    if (recv(sockfd, text, len, 0) == -1)
    {
        perror("fail to recv");
        exit(1);
    }
    cout << text << endl;
    close(sockfd);
    return 0;
}
```

**==运行结果：==**
![[Pasted image 20240521110059.png]]

  
#### TCP服务器-bind、listen、accept
###### 做为 TCP 服务器需要具备的条件
1. 具备一个可以确知的地址，bind
2. 让操作系统知道是一个服务器，而不是客户端，listen
3. 等待连接的到来，accept
对于面向连接的 TCP 协议来说，连接的建立才真正意味着数据通信的开始

###### bind函数
绑定服务器IP地址和端口号
```cpp
#include <sys/types.h>
#include <sys/socket.h>
int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
/*
功能:将套接字与网络信息结构体绑定
参数:
	sockfd: 文件描述符，socket的返回值
	addr: 网络信息结构体
		通用结构体 (一般不用)
			struct sockaddr
		网络信息结构体 sockaddr_in
			#include <netinet/in.h>
			struct sockaddr_in
	addrlen: addr的长度
返回值:
	成功:0,失败:-1
*/
```

###### listen 
保存同一时间连接服务器的客户端的套接字
```cpp
#include <sys/socket.h>
int listen(int sockfd,
			int backlog);
/*
功能:
	将套接字由主动修改为被动
	使操作系统为该套接字设置一个连接队列，用来记录所有连接到该套接字的连接
参数:
	socket：sockfd 监听套接字
	backlog: 连接队列的长度
返回值:
	成功:返回0,失败:其他
*/
```
###### accept函数
阻塞等待客户端的连接请求
```cpp
#include<sys/socket.h>
int accept(int sockfd, struct sockaddr *cliaddr ,socklen_t *addrlen);
/*
功能:
	从已连接队列中取出一个已经建立的连接，如果没有任何连接可用，则进入睡眠等待(阻塞）
参数:
	sockfd: socket 监听套接字
	cliaddr: 用于存放客户端套接字地址结构，自动填充，定义变量即可
	addrlen: 套接字地址结构体长度的地址
返回值：
	成功:新的文件描述符(只要有客户端连接，就会产生新的文件描述符这个新的文件描述符专门与指定的客户端进行通信的)
	失败: -1
*/
```

<mark style="background: #FF5582A6;">注意：</mark>
返回的是一个已连接套接字，这个***套接字代表当前这个连接***，连接几个就返回几个，且在使用这个函数之后再进行通信时就要使用这个函数的返回值 
![[Pasted image 20240521151149.png]]

###### 代码实现
```cpp
#include <iostream>
#include <stdio.h>
#include <sys/socket.h> //socket
#include <sys/types.h>
#include <arpa/inet.h>  //gtons inet_addr
#include <netinet/in.h> //sockaddr_in
#include <unistd.h>     //close，文件描述符
#include <stdlib.h>     //exit
#include <string.h>
#include <sys/socket.h>
#include <sys/stat.h>
#include <fcntl.h>
#define len 128
using namespace std;
int main()
{
    int sockfd;
    if ((sockfd = socket(AF_INET, SOCK_STREAM, 0)) == -1)
    {
        perror("fail to socket");
        exit(1);
    }

    // 将套接字与服务器网络信息结构体绑定
    struct sockaddr_in serveraddr;
    socklen_t serverlen = sizeof(serveraddr);
    serveraddr.sin_family = AF_INET;
    serveraddr.sin_addr.s_addr = inet_addr("172.24.204.145"); // 服务器IP地址和端口号，也就是自己的IP地址
    serveraddr.sin_port = htons(8080);
    if (bind(sockfd, (struct sockaddr *)&serveraddr, serverlen) == -1)
    {
        perror("fail to bind");
        exit(1);
    }

    // 将套接字设置为被动监听动态
    if (listen(sockfd, 10) == -1)
    {
        perror("fail to listen");
        exit(1);
    }

    // 阻塞等待客户端的连接请求
    int acceptfd;
    struct sockaddr_in clientaddr;

    if ((acceptfd = accept(sockfd, (struct sockaddr *)&clientaddr, &serverlen)) == -1)
    {
        perror("fail to accept");
        exit(1);
    }

    // 打印连接的客户端的信息
    cout << "[client_ip:" << inet_ntoa(clientaddr.sin_addr) << "-port:" << ntohs(clientaddr.sin_port) << "]" << endl;

    // 第五步：进行通信
    // tcp服务器与客户端通信时，阻塞等待客户端信息
    char buf[len] = "";
    if (recv(acceptfd, buf, len, 0) == -1)
    {
        perror("fail to recv");
        exit(1);
    }
    cout << "form client:" << buf << endl;
    strcat(buf, "*_*");
    if (send(acceptfd, buf, len, 0) == -1)
    {
        perror("fail to send");
        exit(1);
    }

    close(acceptfd);
    close(sockfd);
    return 0;
}
```

#### 示例运行结果
![[Pasted image 20240521162405.png]]

# TCP 编程-close、三次握手、四次挥手
[一文彻底搞懂 TCP三次握手、四次挥手过程及原理 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/108504297)
[TCP四次挥手及原因 - GuoXinxin - 博客园 (cnblogs.com)](https://www.cnblogs.com/GuoXinxin/p/11657933.html)
###### close关闭套接字
1. 使用 close 函数即可关闭套接字
	1. 关闭一个代表已连接套接字将导致另一端接收到一个 0 长度的数据包
2. 做服务器时
	1. 关闭监听套接字将导致服务器无法接收新的连接，但不会影响已经建立的连接
	2. 关闭 accept 返回的已连接套接字将导致它所代表的连接被关闭，但不会影响服务器的监听
3. 做客户端时
	关闭连接就是关闭连接，不意味着其他

###### 三次握手

客户端与服务器在通信之前的连接
![[Pasted image 20240521205055.png]]


###### 四次挥手

![[Pasted image 20240521205628.png]]

# TCP并发服务器
TCP原本并不是并发服务器，TCP服务器同一时间只能和一个客户通信，就是上面的示例
![[Pasted image 20240521213148.png]]
###### 为什么TCP不能并发，UDP可以

**==TCP不能实现并发的原因 :==**
由于TCP服务器端有两个读阻塞函数，accept和recv，两个函数需要先后运行，所以导致运行一个函
数的时候另一个函数无法执行，所以无法保证一边连接客户端，一边与其他客户端通信

#### 多进程实现并发

###### 客户端
客户端不需要修改与上面的代码一样


###### 服务器端
**==流程如下：==**
```cpp
int sockfd = socket ();//创建套接字
bing();//绑定套接字
listen ();//监听
while(1){
	accrptfd =accept ();//阻塞等待客户端连接请求
	pid =fork ();//创建进程
	//每有一个客户端连接服务器就创建一个新进程，进程数=客户端数量+1；
	if (pid==0){
		while (1){
			recv()/send();
		}
	}
		//当客户端关闭时只结束对应的进程就好
}
```


**==代码实现：==**
```cpp
#include <iostream>
#include <stdio.h>
#include <sys/socket.h> //socket
#include <sys/types.h>
#include <arpa/inet.h>  //gtons inet_addr
#include <netinet/in.h> //sockaddr_in
#include <unistd.h>     //close，文件描述符
#include <stdlib.h>     //exit
#include <string.h>
#include <sys/socket.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <signal.h>
#include <sys/wait.h>

// 使用多进程实现TCP并发服务器

#define len 128
#define ERR_LOG(errmsg) \
    do                  \
    {                   \
        perror(errmsg); \
        exit(1);        \
    } while (0)

using namespace std;

void handler(int sig)
{ // 信号处理函数，结束僵尸进程
    wait(NULL);
}

int main()
{
    int sockfd;

    if ((sockfd = socket(AF_INET, SOCK_STREAM, 0)) == -1)
    {
        perror("fail to socket");
        exit(1);
    }

    // 将套接字设置为允许重复使用本机地址或者设置为端口复用，不设置也可以
    int on = 1;
    if (setsockopt(sockfd, SOL_SOCKET, SO_REUSEADDR, &on, sizeof(on)) < 0)
    {
        ERR_LOG("fail to setsockopt");
    }

    struct sockaddr_in serveraddr, clientaddr;
    socklen_t addrlen = sizeof(serveraddr);
    serveraddr.sin_family = AF_INET;
    serveraddr.sin_addr.s_addr = inet_addr("172.20.156.139");
    serveraddr.sin_port = htons(8080);

    if (bind(sockfd, (struct sockaddr *)&serveraddr, addrlen) == -1)
    {
        ERR_LOG("fail to bind");
    }

    if (listen(sockfd, 5) < 0)
    {
        ERR_LOG("fail to listen");
    }

    // 信号，使用信号异步通信的方式来处理僵尸进程，也就是结束的客户端通信的进程
    signal(SIGCHLD, handler); // 使用handler函数

    while (1)
    {
        int acceptfd;
        if ((acceptfd = accept(sockfd, (struct sockaddr *)&clientaddr, &addrlen)) == -1)
        {
            ERR_LOG("fail to accept");
        }

        cout << inet_ntoa(clientaddr.sin_addr) << "---" << ntohs(clientaddr.sin_port) << endl;

        // 创建子进程，父进程负责连接客户端，子进程负责通信
        pid_t pid;
        if ((pid = fork()) < 0)
        {
            ERR_LOG("fail to fork");
        }
        else if (pid > 0)
        {
            // 父进程什么也不用做
        }
        else
        { // 子进程
            char buf[len] = "";
            ssize_t bytes;
            while (1)
            {
                if ((bytes = recv(acceptfd, buf, len, 0)) == -1)
                {
                    ERR_LOG("fail to recv");
                }
                else if (bytes == 0)
                {
                    cout << "The client quited" << endl;
                    exit(0);
                }
                if (strncmp(buf, "quit", 4) == 0)
                {
                    exit(0);
                }

                strcat(buf, "*_*");
                if (send(acceptfd, buf, len, 0) == -1)
                {
                    ERR_LOG("fail to send");
                }
            }
        }
    }
}
```

**==运行结果：==**

![[Pasted image 20240522101735.png]]
#### 多线程并发
###### 客户端
客户端不需要修改与上面的代码一样


###### 服务器端
**==流程如下：==**
```cpp
void *thread_fun(void *arg){
	while (1){
			recv()/send();
		}
}
int sockfd = socket ();//创建套接字
bing();//绑定套接字
listen ();//监听
while(1){
	accrptfd =accept ();//阻塞等待客户端连接请求
	pthread_create(&thread,NULL,thread_fun);//创建子线程
	//每有一个客户端连接服务器就创建一个子线程，线程数=客户端数量+1；
	pthread_detach();//将线程设置成分离态
	//当客户端关闭时只结束对应的线程就好
}
```

**==代码实现：==**
```cpp
#include <iostream>
#include <stdio.h>
#include <sys/socket.h> //socket
#include <sys/types.h>
#include <arpa/inet.h>  //gtons inet_addr
#include <netinet/in.h> //sockaddr_in
#include <unistd.h>     //close，文件描述符
#include <stdlib.h>     //exit
#include <string.h>
#include <sys/socket.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <signal.h>
#include <sys/wait.h>

// 使用多进程实现TCP并发服务器

#define len 128
#define ERR_LOG(errmsg) \
    do                  \
    {                   \
        perror(errmsg); \
        exit(1);        \
    } while (0)

using namespace std;

typedef struct
{
    struct sockaddr_in addr;//网络结构体变量
    int acceptfd;
} MSG;
//子线程通信函数
void *pthread_fun(void *arg)
{
    char buf[len] = "";
    ssize_t bytes;
    MSG msg = *(MSG *)arg;
    while (1)
    {
        if ((bytes = recv(msg.acceptfd, buf, len, 0)) == -1)
        {
            ERR_LOG("fail to recv");
        }
        else if (bytes == 0)//有线程退出
        {
            cout << "The client quited" << endl;
            pthread_exit(NULL);//结束线程，因为是分离要线程资源自动回收
        }
        if (strncmp(buf, "quit", 4) == 0)//线程退出
        {
            cout << "fail client quited" << endl;
            pthread_exit(NULL);
        }

        cout << inet_ntoa(msg.addr.sin_addr) << "---" << ntohs(msg.addr.sin_port) << endl;

        strcat(buf, "*_*");
        if (send(msg.acceptfd, buf, len, 0) == -1)
        {
            ERR_LOG("fail to send");
        }
    }
}
int main()
{
    int sockfd;

    if ((sockfd = socket(AF_INET, SOCK_STREAM, 0)) == -1)
    {
        perror("fail to socket");
        exit(1);
    }

    struct sockaddr_in serveraddr, clientaddr;
    socklen_t addrlen = sizeof(serveraddr);
    serveraddr.sin_family = AF_INET;
    serveraddr.sin_addr.s_addr = inet_addr("172.20.156.139");
    serveraddr.sin_port = htons(8080);

    if (bind(sockfd, (struct sockaddr *)&serveraddr, addrlen) == -1)
    {
        ERR_LOG("fail to bind");
    }

    if (listen(sockfd, 5) < 0)
    {
        ERR_LOG("fail to listen");
    }

    while (1)
    {
        int acceptfd;
        if ((acceptfd = accept(sockfd, (struct sockaddr *)&clientaddr, &addrlen)) == -1)
        {
            ERR_LOG("fail to accept");
        }

        // 创建子线程与客户端通信
        MSG msg;
        msg.addr = clientaddr;
        msg.acceptfd = acceptfd;
        pthread_t thread;
        if (pthread_create(&thread, NULL, pthread_fun, &msg) != 0)
        {
            ERR_LOG("fail to pthread_create");
        }
        pthread_detach(thread);//将线程设置成分离态，子线程推出后资源自动回收
    }
```





**==运行结果：==**
![[Pasted image 20240522104924.png]]




# web 服务器简介
Web 服务器又称 www 服务器、网站服务器等

**==特点==**
1. 使用 HTTP 协议与客户机浏览器进行信息交流
2. 不仅能存储信息，还能在用户通过 web 浏览器提供的信息的基础上运行脚本和程序
3. 该服务器需可安装在UNIX、Linux 或 Windows 等操作系统上
4. 著名的服务器有 Apache、Tomcat、 IIS 等

#### HTTP协议


###### Webserver HTTP 协议
**==概念==**
一种详细规定了浏览器和万维网服务器之间互相通信的规则，通过因特网传送万维网文档的数据传送协议

**==特点==**
1. 支持 C/S 架构
2. 简单快速: 客户向服务器请求服务时，只需传送请求方法和路径 ，常用方法:GET（明文）、POST（密文）
3. 无连接:限制每次连接只处理一个请求
4. 无状态:即如果后续处理需要前面的信息，它必须重传，这样可能导致每次连接传送的数据量会增大


###### Webserver 通信过程
![[Pasted image 20240522112946.png]]


#### web编程开发 

浏览器做客户端，通过IP地址和端口号来连接服务器

###### 网页浏览 使用GET方式

**==客户端浏览器请求：==**
![[Pasted image 20240522150724.png]]web服务器的IP地址是10.0.31.110，端口号是8000，要访问的网页是index.html

**==服务器收到的数据==**
![[Pasted image 20240522151113.png]]


**==服务器的应答格式：==**
服务器接收到浏览器发送的数据之后，需要判断GET/后面跟的网页是否存在，如果存在则请求成功发送指定的指令，并发送文件内容给浏览器，如果不存在，则发送请求失败的指令
<mark style="background: #ADCCFFA6;">请求成功</mark>
```cpp
"HTTP/1.1 200 OK\r\n"           \
"Content-Type: text/html\r\n"   \
"\r\n";
```

<mark style="background: #ADCCFFA6;;">请求失败</mark>
```cpp
"HTTP/1.1 404 Not Found\r\n"      \
"Content-Type: text/html\r\n'     \
"r\n"                             \
"<HTML><BODY>File not found</BODY></HTML>"；
```


###### 代码实现
```cpp
#include <iostream>
#include <stdio.h>
#include <sys/socket.h> //socket
#include <sys/types.h>
#include <arpa/inet.h>  //gtons inet_addr
#include <netinet/in.h> //sockaddr_in
#include <unistd.h>     //close，文件描述符
#include <stdlib.h>     //exit
#include <string.h>
#include <sys/socket.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <signal.h>
#include <sys/wait.h>


#define len 128
#define ERR_LOG(errmsg) \
    do                  \
    {                   \
        perror(errmsg); \
        exit(1);        \
    } while (0)

using namespace std;

typedef struct
{
    struct sockaddr_in addr; // 网络结构体变量
    int acceptfd;
} MSG;
// 子线程通信函数
void *pthread_fun(void *arg)
{
    int acceptfd =*(int *)arg;
    char buf[len]="";
    char head[] = "HTTP/1.1 200 OK\r\n"           \
                  "Content-Type: text/html\r\n"   \
                  "\r\n";
    char err[] = "HTTP/1.1 404 Not Found\r\n"
                 "Content-Type: text/html\r\n"     \
                 "\r\n "                             \
                 "< HTML><BODY>File not found</BODY> </HTML>";

    if (recv(acceptfd,buf,len,0)==-1){
        ERR_LOG("fail to recv");
    }
    cout<<"***************************************"<<endl;
    cout<<buf<<endl;
    cout << "***************************************" << endl;

//通过获取的数据包中得到浏览器要访问的网页文件名
//GET /index.html http/1.1
    char filename[len]="";
    sscanf(buf,"GET /%s",filename);//sscanf函数遇空格结束，所以直接可以获取文件名
    if(strncmp(filename,"HTTP/1.1",strlen("http/1.1")==0)){
        strcpy(filename, "index.html");
    }
    cout<<"filename = "<<filename<<endl;

    char path[len] = "./www.w3cschool.cc/";
    strcat (path,filename);

    //通过解析出来的网页文件名，查找本地中有没有这个文件
    int fd;
    if ((fd=open (path,O_RDONLY))<0){
        if (errno==ENOENT){//如果文件不存在,则发送不在对应的指令
            if (send (acceptfd,err,strlen (err),0)<0){
                ERR_LOG("fail to send");
            }
            close (acceptfd);
            pthread_exit(NULL);
        }
        else
        {
            ERR_LOG("fail to open");
        }
    }

    // 如果文件存在，先发送指令告知浏览器
    if (send(acceptfd, head, strlen(head), 0) < 0)
    {
        ERR_LOG("fail to send");
    }

    // 读取网页文件中的内容并发送给浏览器
    ssize_t bytes;
    char text[1024] = "";
    while ((bytes = read(fd, text, 1024)) > 0)
    {

        if (send(acceptfd, text, bytes, 0) < 0)
        {
            ERR_LOG("fail to send");
        }
    }
    pthread_exit(NULL);
}
int main()
{
    int sockfd;

    if ((sockfd = socket(AF_INET, SOCK_STREAM, 0)) == -1)
    {
        perror("fail to socket");
        exit(1);
    }

    // 将套接字设置为允许重复使用本机地址或者设置为端口复用，不设置也可以
    int on = 1;
    if (setsockopt(sockfd, SOL_SOCKET, SO_REUSEADDR, &on, sizeof(on)) < 0)
    {
        ERR_LOG("fail to setsockopt");
    }

    struct sockaddr_in serveraddr, clientaddr;
    socklen_t addrlen = sizeof(serveraddr);
    serveraddr.sin_family = AF_INET;
    serveraddr.sin_addr.s_addr = inet_addr("172.28.132.234");
    serveraddr.sin_port = htons(8080);

    if (bind(sockfd, (struct sockaddr *)&serveraddr, addrlen) == -1)
    {
        ERR_LOG("fail to bind");
    }

    if (listen(sockfd, 5) < 0)
    {
        ERR_LOG("fail to listen");
    }

    while (1)
    {
        int acceptfd;
        if ((acceptfd = accept(sockfd, (struct sockaddr *)&clientaddr, &addrlen)) == -1)
        {
            ERR_LOG("fail to accept");
        }

        // 创建子线程与客户端通信
        
        pthread_t thread;
        if (pthread_create(&thread, NULL, pthread_fun, &acceptfd) != 0)
        {
            ERR_LOG("fail to pthread_create");
        }
        pthread_detach(thread); // 将线程设置成分离态，子线程推出后资源自动回收
    }
}
```



