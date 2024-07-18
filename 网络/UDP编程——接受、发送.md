# 字节序、地址转换

#### 字节序
指多字节数据存储顺序

**==分类==**
小端格式：将低位字节数据存储在低地址
大端格式：将高位字节数据存储在高地址

<mark style="background: #FF5582A6;">注意：</mark>
LSB：低地址
MSB：高地址

![[Pasted image 20240514161636.png|852]]

###### 判断系统字节序
```cpp
#include<iostream>
using namespace std;
union un {
    int a;
    char b;
};
int main ()
{
    union un myun;
    myun.a=0x12345678;

    printf ("a=%#x\n",myun.a);
    printf ("b=%#x\n",myun.b);

    if (myun.b==0x78){
        cout<<"小端存储"<<endl;
    }else{
        cout<<"大端存储"<<endl;
    }
    return 0;
}
```

###### 字节序转换函数

如果两个电脑的字节序不同，那么在通信时会导致数据处理错误，因此要使用字节序转换函数
1. ***网络协议指定了通讯字节序——大端***
2. 运行在同一台计算机上的进程相互通信时，一般不考虑字节序
3. 异构计算机之间通信，需要转换自己字节序为网络字节序

```
host-->network,l=long,s=short
1. htonl
头文件：
	#include <arpa/inet.h>
	uint32_t htonl(uint32_t hostint32);
功能：
	将32位主机字节学数据转换成网络字节序数据
参数：
	hostint32：待转换的32位主机字节序数据
返回值：
	成功：返回网络字节序数据

2. htons

network-->host
1. ntohl
头文件：
	#include <arpa/inet.h>
	uint32_t ntohl(uint32_t hostint32);
功能：
	将32位网络字节学数据转换成主机字节序数据
参数：
	hostint32：待转换的32位网络字节序数据
返回值：
	成功：返回主机字节序数据
	
2. ntohs
```

**==示例==**：
```cpp
//linux
#include <iostream>
#include <arpa/inet.h>
using namespace std;
int main ()
{
    int a=0x12345678;
    printf ("a=%#x\n",a);
    printf("htonl(a)=%#x\n",htonl(a));
    return 0;
}
//输出结果：
//a=0x12345678
//htonl(a)=0x78563412

//windows
#include <iostream>
#include <winsock2.h>
#pragma comment (lib,"Ws2_32.lib")
using namespace std;
int main() {
    int a = 0x12345678;
    printf("a=%#x\n", a);
    printf("htonl(a)=%#x\n", htonl(a));
    return 0;
}
//输出结果：
//a=0x12345678
//htonl(a)=0x78563412

```

#### IP地址转换函数

人类识别的ip地址是点分十进制字符串形式，但是计算机或者网络识别的ip地址是无符号整形数据

![[Pasted image 20240515221002.png]]

###### ipv4地址转化

下面这两个函数只能转化IPv4的地址，因为直接

```
#include<sys/socket.h>
#include<netinet/in.h>
#include<arpa/inet.h>

in_addr_t inet_addr(const char *cp);
功能：
	将点分十进制Ip地址转换位整型数据
参数：
	cp：点分十进制IP地址
返回值：
	成功则返回整型数据

char *inet_ntoa(struct in_addr in);
功能：
	将整形数据转化位点分十进制的IP地址
参数：
	in：保存Ip地址的结构体
返回值：
	成功则返回点分十进制的IP地址
```

###### ipv6和ipv4地址转换

下面这两个函数在IPv4和IPv6中都可以使用

```
//头文件：#include <arpa/inet.h>

//字符串IP地址转整型数据
int inet_pton(int family,const char *strptr,void *addrptr);
功能：
	将点分十进制数串转换成32位无符号整数
参数：
	family 协议族  AF_INET（IPv4网络协议），AF_INET6（IPv6网络协议）
	strptr 点分十进制数串
	addrptr 32位无符号整数的地址
返回值：
	成功返回1，失败返回其他

//整型数据转字符串格式IP地址
const char *inet_ntop(int family,const void *addrptr,char *strptr,size_t len);
功能：
	将32位无符号整数转换成点分十进制数串
参数：
	family 协议族
	addrptr 32位无符号整数
	strptr 点分十进制数串
	len strptr缓存区长度
		len的宏定义
		#define INET_ADDRSTRLEN 16 //for ipv4
		#definr INET6_ADDRSTRLEN 46 //for ipv6
返回值：
	成功则返回字符串首地址

```

**==示例==**：
```cpp
#include <iostream>
#include <arpa/inet.h>

using namespace std;

int main()
{
    char str[] = "192.168.1.1";
    unsigned int ip_int=0;
    unsigned char ip_p=0;
    if (1==inet_pton(AF_INET,str,&ip_int)){//字符串IP地址转整型数据
        cout<<ip_int<<endl;//转换成功就输出
    }
    ip_p = static_cast<char>(ip_int);//按字节读取整型
    printf("in_uint=%d.%d.%d.%d\n",ip_p,*(&ip_p+1),*(&ip_p+2),*(&ip_p+3));
    return 0;
}

//输出
//16885952
//in_uint=192.192.168.1


#include <iostream>
#include <arpa/inet.h>
//#include <iomanip> // 用于setw
using namespace std;

int main()
{
    unsigned char ip_int[]={192,168,1,1};
    char ip_str[16]="";
    inet_ntop(AF_INET,&ip_int,ip_str,16);//整型数据转字符串格式IP地址
    cout<<ip_str<<endl;//输出
    return 0;
}
//输出
//192.168.1.1

```


# UDP介绍、编程流程
###### UDP概述
**==UDP 协议==**
面向无连接的用户数据报协议，在传输数据前不需要先建立连接:目地主机的运输层收到 UDP 报文后，不需
要给出任何确认

**==UDP特点==**
1. 相比 TCP 速度稍快些
2. 简单的请求/应答应用程序可以使用 UDP
3. 对于海量数据传输不应该使用 UDP
4. 广播和多播应用必须使用 UDP

**==UDP 应用==**
DNS(域名解析)、NFS(网络文件系统)、RTP(流媒体)等

###### 网络编程接口socket

**网络通信要解决的是不同主机进程间的通信**
1. 首要问题是网络间进程标识问题
2. 以及多重协议的识别问题

20世纪80 年代初，加州大学 Berkeley 分校在 BSD(一个UNIX OS 版本)系统内实现了TCP/IP 协议；其网络程
序编程开发接口为 socket，随着 UNIX 以及类UNIX 操作系统的广泛应用，socket 成为最流行的网络程序开发接口

**==socket 作用==**
提供不同主机上的进程之间的通信

**==socket 特点==**
1. socket 也称<mark style="background: #FFB8EBA6;">“套接字”</mark>
2. 是一种文件描述符,代表了一个通信管道的一个端点
3. 类似对文件的操作一样，可以使用 read、writeclose 等函数对 socket 套接字进行网络数据的收取和发送等操作
4. <mark style="background: #ADCCFFA6;">得到 socket 套接字(描述符) 的方法调用 socket()</mark>

**==socket分类==**
1. SOCK_STREAM，流式套接字，用于TCP
2. SOCK_DGRAM，数据报套接字，用于UDP
3. SOCK_RAW，原始套接字，对于其他层次的协议操作时使用

###### UDP编程C/S架构
![[Pasted image 20240516205146.png]]
![[Pasted image 20240516205248.png|907]]

###### UDP网络编程流程
**==服务器==**
1. 创建套接字 socket( )
2. 将服务器的ip地址、端口号与套接字进行绑定 bind( )
3. 接收数据 recvfrom()
4. 发送数据 sendto()

**==客户端:==**
1. 创建套接字 socket()
2. 发送数据 sendto()
3. 接收数据 recvfrom()
4. 关闭套接字 close()

# UDP编程-创建套接字
###### 创建socket套接字
```cpp
#include <sys/socket.h>
int socket(int family,int type,int protocol);
```

**==功能：==**
创建一个用于网络通信的 socket 套接字(描述符)，相当于创建一个文件描述符

**==参数：==**
1. family:协议族(AF_INET、AF_INET6、PF_PACKET 等)
2. type:套接字类(SOCK_STREAM、SOCK_DGRAM、SOCK_RAW 等)
3. protocol:协议类别(0、IPPROTO_TCP、IPPROTO_UDP 等，一般为0

**==返回值:==**
套接字

**==特点:==**
创建套接字时，系统不会分配端口，也就是我们只创建了一个文件描述符，但它所对应的文件不会被别的计算机找到，需要创建端口才能使用。
创建的套接字默认属性是主动的，即主动发起服务的请求,当作为服务器时，往往需要修改为被动的
```cpp
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

###### 创建UDP套接字demo

```cpp
#include <iostream>
#include <sys/socket.h>
#include <sys/types.h>
#include <arpa/inet.h>
#include <stdlib.h>
using namespace std;

int main()
{
    // 使用socket函数创建套接字
    // 创建一个用于UDP网络编程的套接字
    int sockfd = -1;
    if ((sockfd = socket(AF_INET6, SOCK_DGRAM, 0)) == -1)
    {
        // 创建失败
        perror("fail to socket"); // C标准数据库，配合错误码，输出错误信息
        exit(1);                  // 表示程序以非正常的方式退出，通常表示程序在执行过程中遇到了错误或异常情况。在这种情况下，操作系统可以根据返回的退出代码来确定程序的执行状态，以便进一步处理或记录错误信息。
    }
    cout << sockfd << endl;
    return 0;
}
//输出
//3
//每个进程都会默认打开3个文件描述符，即0、1、2。其中0代表标准输入流、1代表标准输出流、2代表标准错误流。在编程中通常使用宏STDIN_FILENO、STDOUT_FILENO和STDERR_FILENO分别来代表0，1，2。
//所以返回的文件描述符是3
```

# UDP编程-发送、绑定、接收数据

###### IPv4套接字的地址结构

服务器与客户机进行通信必须要知道对方是谁，以及通信的进程，也就是IP地址和端口号，在编程中封装在结构体内
**==在网络编程中常用的结构体==**
***头文件***：
```cpp 
#include <netinet/in.>
```
![[Pasted image 20240516214143.png]]


**==通用结构体==**：
为了使不同格式地址能被传入套接字函数,地址须要强制转换成通用套接字地址结构，原因是因为不同场合使用的结构体不同，但使用的函数时同一个，所以定义一个同样结构体，当在指定场合使用时，再根据要求传入指定的结构体即可

![[Pasted image 20240516214730.png]]

**==两种地址结构使用场合==**
在定义源地址和目的地址结构的时候，选用 struct sockaddr_in;
例:
```cpp
struct sockaddr_in my_addr;
```

当调用编程接口函数，且该函数需要传入地址结构时需要用 struct sockaddr 进行强制转换
例:
```cpp
bind(sockfd,(struct sockaddr*)&my_addr,sizeof(my_addr);
```


###### 发送数据——sendto函数

```cpp
ssize_t sendto(int sockfd,//文件描述符，socket函数的返回值
			const void *buf,//庶发送的数据
			size_t nbytes,//要发送的数据大小
			int flags,//0表示阻塞，MSG_DONTWAIT表示非阻塞
			const struct sockaddr *to,//通用结构体，包含指定的IP
			socklen t addrlen);//结构体的大小
```

**==功能：==**
向 to 结构体指针中指定的 IP，发送 UDP 数据

**==参数:==**
1. sockfd:套接字，文件描述符，socket函数的返回值
2. buf:发送数据缓冲区
3. nbytes: 发送数据缓冲区的大小
4. flags:一般为 0，
5. to: 指向目的主机地址结构体的指针
6. addrlen: to 所指向内容的长度

**==注意：==**
通过to和addrlen确定目的地址，可发送0长度的UDP数据包

**==返回：==**
成功返回发送的数据长度，失败返回-1

###### 向“网络调试助手”发送消息

在windows打开“网络调试助手”，来模拟服务端，在linux编程向服务端发送信息
![[Pasted image 20240517204736.png]]

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

int main(int argc, char const *argv[])
{
    // if (argc < 3)
    // {
    //     fprintf(stderr, "Usage:%s ip port\n", argv[0]);
    //     exit(1);
    // }
    // 第一步：创建套接字
    int sockfd = -1;
    if ((sockfd = socket(AF_INET6, SOCK_DGRAM, 0)) == -1)
    {

        perror("fail to socket");
        exit(1);
    }
    cout << sockfd << endl;
    // 第二步：填充服务器网络结构体，sockaddr_in,主要是IP地址和端口号
    // IP地址：10.10.211.241
    // 端口号：8080
    struct sockaddr_in serveradder; // 网络编程结构体
    socklen_t addlen = sizeof(serveradder);
    serveradder.sin_family = AF_INET; // 网络编程，ipv4地址
     serveradder.sin_addr.s_addr = inet_addr("172.27.80.1"); // 将IP地址转换为无符号整型数据,并写到结构体里面
     serveradder.sin_port = htons(8080);                       // 写入端口号，网络时大端存储所以需要，且sin_port是短整型，所以使用htons函数

    /*
     上面这两行相当于把Ip地址和端口号写死了，同样的代码换个主机就不能用
     通过第一步之前的操作可以在命令行读入IP地址和端口号
    serveradder.sin_addr.s_addr = inet_addr(argv[1]);
    serveradder.sin_port = htons(atoi(argv[2])); // atoi将字符串转换成int类型
    输入：./main 10.10.211.241 8080
    */

    // 第三步：发送数据
    char buf[len] = "";
    buf[strlen(buf) - 1] = '\0'; // 把buf字符串末尾的\n换成\0
    // string str;

    while (1)
    {
        fgets(buf, len, stdin);
        // cin >> str;
        // str[sizeof(str)-1]='\n';
        if (sendto(sockfd, buf, len, 0, (struct sockaddr *)&serveradder, addlen) == -1)
        { // 第五个参数强制转换成通用结构体
            perror("fial to sendto");
            exit(1);
        }
    }

    // 关闭文件套接字描述符
    close(sockfd);

    return 0;
}
```

**==实现结果：==**
![[Pasted image 20240517204831.png|601]]

###### 绑定bing函数

**==UDP网络程序想要收取数据需什么条件 ?==**
确定的ip地址，确定的port

**==怎样完成上面的条件呢 ?==**
1. 接收端 使用bind函数，来完成地址结构与socket套接字的绑定，这样ip、port就固定了
2. 发送端 在sendto函数中指定接收端的ip、port，就可以发送数据了
由于服务器是被动的，客户端是主动的，所以一般先运行服务器，后运行客户端，所以服务器需要固定自己的IP地址和端口号，才能让客户端找到服务器并通信，客户端一般不需要bing绑定，因为系统会自动给客户端分配

```cpp
int bing (int sockfd,//文件描述符
		 const struct sockaddr *myaddr,//网络通信结构体
		 socklen_t addrlen);//结构体大小
```

**==功能:==**
将套接字与网络信息结构体绑定

**==参数:==**

```cpp
sockfd: 文件描述符，socket的返回值
addr: 网络信息结构体
	通用结构体(一般不用)
		struct sockaddr
	网络信息结构体 sockaddr_in
#include <netinet/in.h>
struct sockaddr in
```
		
**==返回值:==**
	成功: 0，失败: -1

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

int main(int argc, char const *argv[])
{
    if (argc < 3)
    {
        fprintf(stderr, "Usage:%s ip port\n", argv[0]);
        exit(1);
    }
    // 第一步：创建套接字
    int sockfd;
    if ((sockfd = socket(AF_INET, SOCK_DGRAM, 0)) == -1)
    {
        perror("fail to socket");
        exit(1);
    }

    // 将服务器的网络信息结构体绑定前进行填充
    struct sockaddr_in serveradder; // 网络编程结构体
    serveradder.sin_family = AF_INET;
    serveradder.sin_addr.s_addr = inet_addr(argv[1]);
    serveradder.sin_port = htons(atoi(argv[2])); // atoi将字符串转换成int类型

    // 第三步：将网络信息结构体与套接字绑定
    if (bind(sockfd, (struct sockaddr *)&serveradder, sizeof(serveradder)) == -1)
    {
        perror("fail to bind");
        exit(1);
    }
    else
    {
        cout << "ok" << endl;
    }

    // 关闭文件套接字描述符
    close(sockfd);

    return 0;
}
//输出
//ok
```


###### 接收数据——recvfrom函数

```cpp
ssize_t recvfrom(int sockfd, //文件描述符
			void *buf,//数据缓冲区
			size_t nbytes,//缓冲区大小
			int flags,//套接字标志，常为0
			struct sockaddr *from,//源地址结构体指针，保存数据来源，可以是NULL
			socklen_t *addrlen);//结构体大小，可以是NULL
```

**==功能:==**
接收UDP 数据，并将源地址信息保存在 from 指向的结构中

**==参数:==**
1. sockfd:套接字
2. buf: 接收数据缓冲区
3. nbytes:接收数据缓冲区的大小  
4. flags:套接字标志(常为 0)
5. from:源地址结构体指针，用来保存数据的来源
6. addrlen:from 所指内容的长度

**==注意：==**
通过from和addrlen参数来存放数据信息来源，from和addrlen可以为NULL，表示不保存数据来源


###### 接受“网络调试助手”的数据
此时"网络调试助手"作为客户端，Ubuntu的程序作为服务器接收数据

**==设置客户端：==**
![[Pasted image 20240517201042.png]]

**==代码：==** 服务器端（Ubuntu代码）
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

int main(int argc, char const *argv[])
{
    if (argc < 3)
    {
        fprintf(stderr, "Usage:%s ip port\n", argv[0]);
        exit(1);
    }
    // 第一步：创建套接字
    int sockfd;
    if ((sockfd = socket(AF_INET, SOCK_DGRAM, 0)) == -1)
    {

        perror("fail to socket");
        exit(1);
    }

    // 将服务器的网络信息结构体绑定前进行填充
    struct sockaddr_in serveradder; // 网络编程结构体
    serveradder.sin_family = AF_INET;
    serveradder.sin_addr.s_addr = inet_addr(argv[1]);
    serveradder.sin_port = htons(atoi(argv[2])); // atoi将字符串转换成int类型

    // 第三步：将网络信息结构体与套接字绑定
    if (bind(sockfd, (struct sockaddr *)&serveradder, sizeof(serveradder)) == -1)
    {
        perror("fail to bind");
        exit(1);
    }

    //接收数据
    char buf[len]="";
    struct sockaddr_in clientaddr;
    socklen_t addrlen = sizeof (struct sockaddr_in);
    if (recvfrom(sockfd,buf,len,0,(struct  sockaddr *)&clientaddr,&addrlen)==-1)
    {
       perror("fail to recvfrom");
       exit (1);
    }

    //打印接收到的数据
    cout<<"form client:"<<buf<<endl;
    //打印客户端的IP地址和端口号
    cout <<"ip:"<< inet_ntoa(clientaddr.sin_addr)<<"port:"<<ntohs (clientaddr.sin_port)<<endl;

    // 关闭文件套接字描述符
    close(sockfd);

    return 0;
}
```

服务器也就是ubuntu程序接收到的数据
![[Pasted image 20240517201341.png]]


# UDP编程-client、server

![[Pasted image 20240517203847.png]]


###### 作业

> [!NOTE] 作业
> 服务器端：接受客户端的消息，并回信
> 客户端：向服务器发送信息，并接收服务器端的回信
> 服务器端：可以接受多个客户端的消息并回信，因为每个客户端都有单独的端口，
> 提示：在一个linux系统中同时运行服务器端和客户端，只需要不同的终端就可以，同时IP地址都是回环地址，只是端口号不同，没新开一个终端都可以模拟成一个客户端
> **==附加题：==**采用多线程编程，使其随时随地都可以接受和发送数据，类似于QQ上两个人聊天







