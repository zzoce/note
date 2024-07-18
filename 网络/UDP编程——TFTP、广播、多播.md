
# TFTP简介、通信过程
###### TFTP概述
**==TFTP:简单文件传送协议==**
最初用于引导无盘系统，被设计用来传输小文件

**==特点:==**
基于 UDP 协议实现
不进行用户有效性认证

**==数据传输模式:==**
1. octet:二进制模式
2. netascii:文本模式
3. mail: 已经不再支持

###### TFTP通信过程
![[Pasted image 20240518183529.png]]


**==TFTP 通信过程总结==**
1. 服务器在 69 号端口等待客户端的请求
2. 服务器若批准此请求,则使用临时端口与客户端进行通信
3. 每个数据包的编号都有变化(从1开始)
4. 每个数据包都要得到 ACK 的确认如果出现超时,则需要重新发送最后的包(数据或ACK)
5. 数据的长度以 512Byte 传输
6. 小于 512Byte 的数据意味着传输结束

###### TFTP协议分析

![[Pasted image 20240518185305.png]]

**==注意：==**
以上0代表的是'\0'，不同的差错码对应不同的错误信息。

**==差错码：==**

	0 未定义,参见错误信息
	1 File not found.//文件没有找到
	2 Access violation.
	3 Disk full or allocation exceeded
	4 illegal TFTP operation.
	5 Unknown transfer ID.
	6 File already exists.
	7 No such user.
	8 Unsupported option(s) requested.

###### TFTP发送——客户端

**==练习要求==**
使用TFTP协议，下载server上的文件到本地

**==实现思路==**
1. 构造请求报文，送至服务器(69号端口)
2. 等待服务器回应
3. 分析服务器回应
4. 接收数据,直到接收到的数据包小于规定数据长度 

服务器端事先准备好的软件来实现，软件设置
![[Pasted image 20240518203707.png]]



**==客户端代码==**
```cpp
#include <iostream>
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

void do_download(int sockfd, struct sockaddr_in serveradder)
{
    char filename[len] = "";
    printf("请输入要下载的文件夹名：");
    scanf("%s", filename);

    // 给服务器发送消息，告知服务器执行下载操作
    char text[1024] = "";
    int text_len;
    socklen_t addrlen = sizeof(struct sockaddr_in);
    int fd;
    int flags = 0;
    int num = 0;
    ssize_t bytes;
    text_len = sprintf(text, "%c%c%s%c%s%c", 0, 1, filename, 0, "octet", 0);
    if (sendto(sockfd, text, text_len, 0, (struct sockaddr *)&serveradder, addrlen) < 0)
    {
        perror("fsil to sendto");
        exit(1);
    }

    while (1)
    {
        // 接收下载数据
        if ((bytes = recvfrom(sockfd, text, sizeof(text), 0, (struct sockaddr *)&serveradder, &addrlen)) == -1)
        {
            perror("fail to recvfrom");
            exit(1);
        }
        cout << "data packet num:" << num << endl;

        if (text[1] == 5)
        {
            printf("error: %s\n", text + 4);
            return;
        }
        else if (text[1] == 3)
        {
            if (flags == 0)
            {
                if ((fd = open(filename, O_WRONLY | O_CREAT | O_TRUNC, 0664)) < 0)
                {
                    perror("fail to open");
                    exit(1);
                }
                flags = 1;
            }

            if ((num + 1 == ntohs(*(unsigned short *)(text + 2))) && (bytes == 516))
            {

                // 写到文件里
                num = ntohs(*(unsigned short *)(text + 2));
                if (write(fd, text + 4, bytes - 4) < 0)
                {
                    perror("fail to write");
                    exit(1);
                }

                // 当文件写入完毕后，给服务器发送ACK
                text[1] = 4;
                if (sendto(sockfd, text, 4, 0, (struct sockaddr *)&serveradder, addrlen) == -1)
                {
                    perror("fail to sendto");
                    exit(1);
                }
            }
            else if ((num + 1 == ntohs(*(unsigned short *)(text + 2))) && (bytes < 516))
            {
                if (write(fd, text + 4, bytes - 4) < 0)
                {
                    perror("fail to write");
                    exit(1);
                }

                text[1] = 4;
                if (sendto(sockfd, text, 4, 0, (struct sockaddr *)&serveradder, addrlen) == -1)
                {
                    perror("fail to sendto");
                    exit(1);
                }
                printf("文件下载完毕\n");
                return;
            }
        }
    }
}

int main(int argc, char const *argv[])
{

    int sockfd = -1;
    if ((sockfd = socket(AF_INET, SOCK_DGRAM, 0)) == -1)
    {
        perror("fail to socket");
        exit(1);
    }

    struct sockaddr_in serveradder; // 网络编程结构体
    socklen_t addlen = sizeof(serveradder);
    serveradder.sin_family = AF_INET;                        // 网络编程，ipv4地址
    serveradder.sin_addr.s_addr = inet_addr("172.22.192.1"); //
    serveradder.sin_port = htons(69);

    do_download(sockfd, serveradder);

    // 关闭文件套接字描述符
    close(sockfd);

    return 0;
}
```
# UDP广播

广播:由一台主机向该主机所在子网内的所有主机发送数据的方式，例如192.168.3.103主机发送广播信息，则192.168.3.1~192.168.3.254所有主机都可以接收到数据，广播只能用UDP或原始IP实现，不能用TCP

**==广播的用途==**
单个服务器与多个客户主机通信时减少分组流通
以下几个协议都用到广播
1. 地址解析协议(ARP)
2. 动态主机配置协议( DHCP )
3. 网络时间协议(NTP )

**==广播的特点==**
1. 处于同一子网的所有主机都必须处理数据
2. UDP数据包会沿协议栈向上一直到UDP层
3. 运行音视频等较高速率工作的应用，会带来大负
4. ***局限于局域网内使用***

**==广播地址==**

1. 网络ID表示由子网掩码中1覆盖的连续位
2. 主机ID表示由子网掩码中0覆盖的连续位

**==定向广播地址: 主机ID全1==**
1. 例: 对于192.168.220.0/24，其定向广播地址为192.168.220.255
2. 通常路由器不转发该广播，也有可能转发

**==受限广播地址: 255.255.255.255==**
路由器从不转发该广播


###### 广播与单播的区别 

**==单播==**
![[Pasted image 20240519210005.png]]

**==广播==**
![[Pasted image 20240519210749.png]] 

###### 广播流程
**==发送者==**
1. 第一步 : 创建套接字 socket()
2. 第二步: 设置为允许发送广播权限 setsockopt()
3. 第三步: 向广播地址发送数据 sendto()

**==接收者==**
1. 第一步:创建套接字 socket()
2. 第二步: 将套接字与广播的信息结构体绑定 bind()
3. 第三步: 接收数据 recvfrom()
###### 套接字选项——setsockopt

```cpp
int setsockopt(int sockfd,
			   int level,
			   int optname,
			   const void *optval,
			   socklen_t optlen);
//成功返回0，否则返回-1
/*
功能: 
	设置一个套接字的选项(属性)
参数:
	socket:文件描述符
	level: 协议层次
		SOL_SOCKET：套接字层次
		IPPROTO_TCP：tcp层次
		IPPROTO_IP：IP层次
	optname: 选项的名称
		SO BROADCAST 允许发送广播数据 (SOL SOCKET层次的)
	optval:设置的选项的值
		int类型的值，存储的是boo1的数据 (1和e)
			0		不允许
			1       允许
	optlen: option_value的长度
*/ 
```
![[Pasted image 20240519212113.png]]


#### 广播代码

###### 发送端代码

```cpp
#include <iostream>
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
    if ((sockfd = socket(AF_INET6, SOCK_DGRAM, 0)) == -1)
    {
        perror("fail to socket");
        exit(1);
    }

    struct sockaddr_in serveradder; // 网络编程结构体
    socklen_t addlen = sizeof(serveradder);

    serveradder.sin_family = AF_INET;                          // 网络编程，ipv4地址
    serveradder.sin_addr.s_addr = inet_addr(" 172.23.159.255"); // 或者广播地址，使用命令行可以看到 broadcast 172.23.159.255
    serveradder.sin_port = htons(9999);

    int on = 1;
    if (setsockopt(sockfd, SOL_SOCKET, SO_BROADCAST, &on, sizeof(on)) < 0)
    {
        perror("fail to setsockopt");
        exit(1);
    }

    char buf[len] = "";
    while (1)
    {
        fgets(buf, len, stdin);

        if (sendto(sockfd, buf, len, 0, (struct sockaddr *)&serveradder, addlen) == -1)
        {
            perror("fial to sendto");
            exit(1);
        }
    }

    // 关闭文件套接字描述符
    close(sockfd);

    return 0;
}
```


###### 接收端代码
```cpp
#include <iostream>
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

int main(int argc, char const *argv[])
{
    // if (argc < 3)
    // {
    //     fprintf(stderr, "Usage:%s ip port\n", argv[0]);
    //     exit(1);
    // }
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
    serveradder.sin_addr.s_addr = inet_addr(" 172.23.159.255"); // 或者广播地址，使用命令行可以看到 broadcast 172.23.159.255
    serveradder.sin_port = htons(9999);

    // 第三步：将网络信息结构体与套接字绑定
    if (bind(sockfd, (struct sockaddr *)&serveradder, sizeof(serveradder)) == -1)
    {
        perror("fail to bind");
        exit(1);
    }

    // 接收数据
    char buf[len] = "";
    struct sockaddr_in clientaddr;
    socklen_t addrlen = sizeof(struct sockaddr_in);
    while (1)
    {
        if (recvfrom(sockfd, buf, len, 0, (struct sockaddr *)&clientaddr, &addrlen) == -1)
        {
            perror("fail to recvfrom");
            exit(1);
        }

        cout << "form client:" << buf << endl;
        cout << "ip:" << inet_ntoa(clientaddr.sin_addr) << "port:" << ntohs(clientaddr.sin_port) << endl;
    }

    // 关闭文件套接字描述符
    close(sockfd);

    return 0;
}
```

###### 运行结果
![[Pasted image 20240519222612.png]]


#### IPv6不支持广播
1. **地址空间和数量**：IPv6采用128位标识，理论上可以拥有（43亿×43亿×43亿×43亿）个地址，几乎是无限的。相比之下，IPv4的32位地址空间已经耗尽。IPv6的地址数量足够满足全球需求，不再需要广播来解决地址短缺问题。
    
2. **组播替代广播**：IPv6引入了组播地址类型，用于向一组设备传递信息。组播取代了广播，减少了对其他设备的干扰，提高了网络安全性。
    
3. **性能和效率**：IPv4广播需要中间路由设备进行软件处理，对性能造成较大消耗。IPv6报文头的处理更为简化，提高了效率
# UDP多播

**==多播:==**
数据的收发仅仅在同一分组中进行
**==多播的特点:==**
1. 多播地址标示一组接口
2. ***多播可以用于广域网使用***
3. 在IPv4 中，多播是可选的
![[Pasted image 20240520154713.png]]


###### 多播地址
**==IPv4 的 D 类地址是多播地址==**
1. 十进制: 224.0.0.1~239.255.255.254
2. 十六进制:EO.00.00.01~EF.FF.FF.FE

**==多播地址向以太网 MAC 地址的映射==**
多播的MAC地址：高24位固定，低23位将多播IP地址的低23位映射过来。  
eg: IP为224.0.0.1，即多播地址为01:00:5e:00:01:01
 ![[Pasted image 20240520161205.png]]

###### 多播工作过程

广播：发送者必须允许广播，接收方只需在同一个网段就可以
多播：发送者直接发送信息，接收方必须加入多播组才能接受信息

比起广播多播具有可控性，只有加入多播组的才能接收数据
![[Pasted image 20240520162238.png]]

**==发送者==**
1. 创建套接字 socket
2. 向多播地址发送数据 recvfrom

**==接收者==**
1. 创建套接字 socket
2. 设置加入多播组 setsockopt
3. 将套接字与多播信息结构体绑定：bind
4. 接收数据

#### 多播地址结构体，套接口选项

###### 多播地址结构体
在 IPv4 因特网域(AF_INET)中，多播地址结构体用如下结构体 ip_mreg 表示
```cpp
struct in addr{
	in_addr_t s_addr;
};
struct ip mreq
{
	struct in_addr imr_multiaddr;//多播组IP
	struct in_addr imr_inerface;//将要添加到多播组的IP
};
```

###### 多播套接口选项
```cpp
int setsockopt(int sockfd,
			   int level,
			   int optname,
			   const void *optval,
			   socklen_t optlen);
//成功返回0，否则返回-1
/*
功能: 设置一个套接字的选项(属性)
参数:
	socket: 文件描述符
	level: 协议层次
		IPPROTO IP IP层次
	option_name: 选项的名称
		IP_ADD_MEMBERSHIP 加入多播组
	option_value: 设置的选项的值
		struct ip_mreq{
			struct in_addr imr_multiaddr; //组播ip地址
			struct in_addr imr_interface; //将要添加到多播组的IP
				INADDR_ANY 任意主机地址(自动获取你的主机地址)
			};
	option_len: option_value的长度
返回值:
	成功:0,失败:-1
*/
```
![[Pasted image 20240520163603.png]]

###### 加入多播组示例
 
```cpp
char group[INET_ADDRSTRLEN]="224.0.1.1";
//定义一个多播组地址
struct in_mreg mreq;
//添加一个多播组IP
mreq.imr_multiaddr.s_addr = inet addr(group);//多播地址
//添加一个将要添加到多播组的IP
mreq.imr_interface.s_addr = htonl(INADDR_ANY);//要加入到多播组的IP
setsockopt(sockfd，IPPROTO_IP，IP_ADD_MEMBERSHIP,&mreq, sizeof(mreq));
```
#### 代码实现

###### 发送端 groupcast_send
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
    if ((sockfd = socket(AF_INET, SOCK_DGRAM, 0)) == -1)
    {
        perror("fail to socket");
        exit(1);
    }

    struct sockaddr_in groupcastaddr;
    socklen_t addrlen = sizeof(groupcastaddr);

    groupcastaddr.sin_family = AF_INET;
    groupcastaddr.sin_addr.s_addr = inet_addr("224.0.0.1"); // 多播地址范围224.0.0.1~239.255.255.254
    groupcastaddr.sin_port = htons(9999);                   // 端口号随便

    char buf[128] = "";
    while (1)
    {
        fgets(buf, sizeof(buf), stdin);
        buf[strlen(buf) - 1] = '\0';
        if (sendto(sockfd, buf, sizeof(buf), 0, (struct sockaddr *)&groupcastaddr, addrlen) == -1)
        {
            perror("fail to sento");
            exit(1);
        }
    }
    close(sockfd);
    return 0;
}
```

###### 接收端groupcast_recv
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
    if ((sockfd = socket(AF_INET, SOCK_DGRAM, 0)) == -1)
    {
        perror("fail to socket");
        exit(1);
    }

    struct ip_mreq mreq;
    mreq.imr_multiaddr.s_addr = inet_addr("224.0.0.1"); // 广播IP地址，也是发送端的
    mreq.imr_interface.s_addr = INADDR_ANY;             // INADDR_ANY，自动获取主机的IP地址
    if (setsockopt(sockfd, IPPROTO_IP, IP_ADD_MEMBERSHIP, &mreq, sizeof(mreq)) == -1)
    {
        perror("fail to setsockopt");
        exit(1);
    }

    struct sockaddr_in groupcastaddr;
    socklen_t addrlen = sizeof(groupcastaddr);
    groupcastaddr.sin_family = AF_INET;
    // 广播IP地址和端口号，也就是发送端的
    groupcastaddr.sin_addr.s_addr = inet_addr("224.0.0.1");
    groupcastaddr.sin_port = htons(9999);

    if (bind(sockfd, (struct sockaddr *)&groupcastaddr, addrlen) == -1)
    {
        perror("fail to bind");
        exit(1);
    }

    char text[32] = "";
    struct sockaddr_in sendaddr;
    socklen_t sendaddrlen = sizeof(sendaddr);
    while (1)
    {
        if (recvfrom(sockfd, text, sizeof(text), 0, (struct sockaddr *)&sendaddr, &sendaddrlen) == -1)
        {
            perror("fail to recvfrom");
            exit(1);
        }
        cout << "[ip:" << inet_ntoa(sendaddr.sin_addr) << "port:" << ntohs(sendaddr.sin_port) << "] form client:" << text << endl;
    }
    close(sockfd);
    return 0;
}
```

###### 运行结果
![[Pasted image 20240520214337.png]]