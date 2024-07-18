
## 第一章：计算机网络概述

### 1.1计算机网络发展简史

#### 1.1.1最早的广域网

在通信双方或多方之间，通过电路交换建立电路连接的网络。以前都是接线员人工接线。

![在这里插入图片描述](https://img-blog.csdnimg.cn/fa73739a98664393b8a0745752d23665.png)

#### 1.1.2电路交换网特点

- 1、建立链接->使用链接->释放链接
    
- 2、物理通路被通信双方**独占**
    

计算机数据是突发式出现在数据链路上的，而电路交换网的建立链接、使用链接、释放链接的三个过程使得传输效率太低，故**电路交换不适合传输计算机数据**。

#### 1.1.3计算机网络的要求

1957年10月4日，苏联发射了世界上第一颗人造地球卫星——Sputnik。  
针对Sputnik所带来的威胁，美国国会于1958年1月7日拨款成立ARPA(the Advanced Research Projects Agency 美国高级研究计划署)

对计算机网络的要求  
1、不是为了打电话  
2、结构简单，可靠的传输数据  
3、能够连接不同种类的计算机  
4、所有网络节点同等重要  
5、必须有冗余的路由（不同路径的选择）

#### 1.1.4分组交换

通过标有地址的分组进行路由选择传送数据，使通信通道仅在传送期间被占用的一种交换方式——比如下载英雄联盟，大文件肯定要分开走不同的路径下载。

分组的组成：

每个分组都由首部和数据段组成；为什么？——携带信息，比如第几个数据、目的地、发送地地址等等。

![在这里插入图片描述](https://img-blog.csdnimg.cn/730892064d4e434fbbd78fbe1018836a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

实现分包传输，最大网络MTU是1500；防止后包先置，接收端根据报文编号重组。

#### 1.1.5交换方式

交换方式—存储转发

节点收到分组，先暂时存储下来，再检查其**首部**，按照首部中的目的地址，找到合适的节点转发出去(路由器做的工作，==路由表==)。

![在这里插入图片描述](https://img-blog.csdnimg.cn/556c709570994e628262b288e18239a0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_10,color_FFFFFF,t_70,g_se,x_16)

特点：  
1、以分组作为传输单位(下载英雄联盟数据分开传)  
2、独立的选择转发路由（不同的数据包走的路由线路是独立选择的，不一定一样）  
3、逐段占用，动态分配传输带宽

想一想：节点收到的分组有序吗？——不是的，数据包到达的时间不一定是有序的，有可能后包先置

#### 1.1.6因特网发展史

从单个 ARPANET（阿帕网）向因特网发展的过程  
1983年 TCP/IP 协议成为 APRANET 的标准协议

**1.1.6.1三级结构的因特网**

（NSFNET美国国家科学基金网）  
围绕六台大型计算机中心建设起来的计算机网络  
主干网、地区网、校园网

![在这里插入图片描述](https://img-blog.csdnimg.cn/6e2596928e284b6b80b11c17f2cc5063.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

blog.csdnimg.cn/02d4e68f966547dba982064d67e84313.png)

**1.1.6.2多级结构因特网**

NSFNET逐步被商用因特网主干网替代

![在这里插入图片描述](https://img-blog.csdnimg.cn/78649f30bbfb4e7991ab93f24926544b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_19,color_FFFFFF,t_70,g_se,x_16)

### 1.2 TCP/IP协议族简介

为了使各种不同的计算机之间可以互联，ARPANet指定了一套计算机通信协议，即TCP/IP协议(族)

应用领域：

![在这里插入图片描述](https://img-blog.csdnimg.cn/8df8bddf2d9c4bf09556067ff84d33f9.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/a8961a15332c4ebb90b14df0100c7321.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/5eaa71d8f3bb44ceafdbb39cb69b02db.png)

- 为了减少协议设计的复杂性，大多数网络模型均采用分层的方式来组织
- 每一层利用下一层提供的服务来为上一层提供服务
- 本层服务的实现细节对上层屏蔽

#### 1.2.1 分层结构

**七层模型：**

![在这里插入图片描述](https://img-blog.csdnimg.cn/8efd86b114ac46e38aed3ca3892e519c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

**四层模型：**

![在这里插入图片描述](https://img-blog.csdnimg.cn/1932908fbe034c2fae27cacad8c520e7.png)

- 应用层：应用程序间沟通的层  
    例如：FTP、Telnet、HTTP、SSH等
- 传输层：提供进程间(程序到程序)的数据传送服务(端口)，比如数据包是传给电脑的qq程序还是微信程序  
    负责传送数据，提供应用程序端到端的逻辑通信  
    例如：TCP、UDP
- 网络层：提供基本的数据封包传送功能  
    最大可能的让每个数据包都能够到达目的主机(主机到主机，源IP到目的IP)  
    例如：IP、ICMP(PING命令)等
- 链路层：负责数据帧的发送和接收(MAC地址，设备到设备，也就是网卡到网卡，源MAC到目的MAC)  
    例如：ARP 地址解析 作用：通过IP找MAC地址  
    RARP反向地址解析协议 作用：通过MAC地址找IP

> 电脑每个网卡的信息：  
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/93d98fd688a54f9ea5f58a5b68e86a79.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_17,color_FFFFFF,t_70,g_se,x_16)

每层完成自己的任务，最终通过不同层次的处理完成数据的收发

![在这里插入图片描述](https://img-blog.csdnimg.cn/852fc9b0938b48f2b466643834a9051a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

#### 1.2.2 IP协议简介

特指为实现在一个相互连接的网络系统上从源地址到目的地传输数据包（互联网数据包）所提供必要功能的协议  
特点：  
不可靠：它不能保证IP数据包能成功地到达它的目的地，仅提供尽力而为的传输服务  
无连接：IP并不维护任何关于后续数据包的状态信息。每个数据包的处理是相互独立的。IP数据包可以不按发送顺序接收  
IP数据包中含有发送它主机的IP地址（源地址）和接收它主机的IP地址（目的地址）

#### 1.2.3 TCP协议简介

TCP是一种面向连接的,可靠的传输层通信协议  
功能：  
提供不同主机上的进程间通信  
特点  
1、建立链接->使用链接->释放链接（虚电路）  
2、TCP数据包中包含序号和确认序号  
3、对包进行排序并检错，而损坏的包可以被重传  
服务对象  
需要高度可靠性且面向连接的服务  
如HTTP、FTP、SMTP等

#### 1.2.4 UDP协议简介

UDP是一种面向无连接的传输层通信协议  
功能：  
提供不同主机上的进程间通信  
特点  
1、发送数据之前不需要建立链接  
2、不对数据包的顺序进行检查  
3、没有错误检测和重传机制  
服务对象  
主要用于“查询—应答”的服务  
如：NFS、NTP、DNS等

### 1.3 MAC地址、IP地址、Netmask子网掩码、端口

#### 1.3.1 网卡

无线网卡，有线网卡，虚拟网卡。。

又称为网络适配器或网络接口卡NIC，但是现在更多的人愿意使用更为简单的名称“网卡”。用来发送接收数据，将模拟信号转成数字信号。  
通过网卡能够使不同的计算机之间连接，从而完成数据通信等功能。

每块网卡都有一个全球唯一的ID号，也就是MAC地址。MAC地址6个字节48位，2^48个MAC地址全球够用了。

![在这里插入图片描述](https://img-blog.csdnimg.cn/c4b003f08ffb4335b5e09dbdadecc7b0.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/7a3d667facbb4dcca3b1a4cdd96ed3dd.png)

#### 1.3.2 mac地址

MAC地址,用于标识网络设备,类似于身份证号，且理论上**全球唯一**。  
组成：以太网内的MAC地址是一个48bit的值

![在这里插入图片描述](https://img-blog.csdnimg.cn/3e5e80c9652042aab715d930e6afc350.png)

#### 1.3.3 IP地址

用来标识主机或网卡的一个虚拟IP(可以改变)。

IP地址是一种Internet上的主机编址方式，也称为网际协议地址。

**1.3.3.1 ip地址组成**

使用32bit(IPV4),由{子网ID，主机ID}两部分组成：

- ==子网ID==:IP地址中由**子网掩码**中1覆盖的连续位。
- ==主机ID==:IP地址中由**子网掩码**中0覆盖的连续位。  
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/b172247aee39421485c26c9abf75caeb.png)

**1.3.3.2 ip地址特点**

- 子网ID不同的网络不能直接通信，如果要通信则需要路由器转发
- 主机ID全为0的IP地址表示**网段地址**，例如 10.1.2.0，此IP不能分配给设备使用。
- 主机ID全为1的IP地址表示该网段的**广播地址**，例如10.1.2.255，此IP不能分配给设备使用。

> ![这里是引用](https://img-blog.csdnimg.cn/f321f77fc3f34f28a0cba52b8f917efd.png)

**1.3.3.3 ip地址分类如下:**

- A类地址：默认8bit子网ID,第一位为0
- B类地址：默认16bit子网ID,前两位为10
- **C类地址：默认24bit子网ID,前三位为110**
- D类地址：前四位为1110,多播地址
- E类地址: 前五位为11110,保留为今后使用

A,B,C三类地址是最常用的

> C类地址的子网掩码及IP地址如下图红色字体所示：![在这里插入图片描述](https://img-blog.csdnimg.cn/9fd6df402c624f7eb8ff8db66f8a7944.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

- 公有IP（可直接连接Internet）  
    经由InterNIC所统一规划的IP
- 私有IP（不可直接连接Internet ）—— 花生壳软件，内网穿透，访问家里的IP  
    主要用于局域网络内的主机联机规划

![在这里插入图片描述](https://img-blog.csdnimg.cn/10fa712ccd9c4cdbb34fb953a4834a1f.png)

**1.3.3.4回环ip地址**

通常 127.0.0.1 称为回环地址(本主机地址)。四层网络模型不会到达数据链路层，到网络层就回去了，回到本主机。

![在这里插入图片描述](https://img-blog.csdnimg.cn/9ebbe28b72734de29f8dc471dd89b9c1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_14,color_FFFFFF,t_70,g_se,x_16)

**功能：**  
主要是测试本机的网络配置，能ping通127.0.0.1说明本机的网卡和IP协议安装都没有问题。

**注意：**  
127.0.0.1~127.255.255.254中的任何地址都将回环到本地主机中。  
不属于任何一个有类别地址类,它代表设备的本地虚拟接口。

**1.3.3.5 ip地址设置**

- linux：ifconfig eth0 192.168.1.1
- windows:  
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/759a6080cd774c7da874671d9d49809f.png)

自动获取网络参数（DHCP）  
在局域网内会有1台主机负责管理所有的计算机网络参数，当PC启动时就会主动向服务器要求IP参数，若PC获取到服务器给的网络相关参数，那么就可以使用网络功能了进行通信  
windows中：如图

![在这里插入图片描述](https://img-blog.csdnimg.cn/41b0993762804f819a8d1ea8a9e6091b.png)

在linux中重新获取ip的方式：  
sudo dhclient

通过拨号取得  
向ISP申请注册，直接拨到ISP，ISP会自动设置电脑的正确的网络参数  
windows：如图

![在这里插入图片描述](https://img-blog.csdnimg.cn/5d25194b96a343c48204cf74dfcdb14e.png)

linux：  
sudo pppoeconf  
sudo pon dsl-provider //拨号ADSL  
sudo poff //断开ADSL

#### 1.3.4 子网掩码

子网掩码（subnet mask）又叫网络掩码、地址掩码是一个32bit由1和0组成的数值，并且1和0分别连续。

指明IP地址中哪些位标识的是主机所在的子网以及哪些位标识的是主机号

**必须结合IP地址一起使用，不能单独存在**。  
IP地址中由子网掩码中1覆盖的连续位为子网ID,其余为主机ID。

子网掩码的表现形式

```bash
192.168.220.0/255.255.255.0
也可以写成：192.168.220.0/24(255就是8个1,3个255就是24个1)
```

手动进行配置如下(linux)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/c0886ae19c4a465090a986f26383f8f5.png)

#### 1.3.5端口

**1.3.5.1 端口概述**

TCP/IP协议采用端口标识通信的进程用于区分一个系统里的多个进程(应用程序)

特点  
1、对于同一个端口，在不同系统中对应着不同的**进程**  
2、对于同一个系统，一个端口只能被一个进程拥有  
3、一个进程拥有一个端口后，传输层送到该端口的数据全部被该进程缓冲区接收，同样，进程送交传输层的数据也通过该端口被送出。

**1.3.5.2端口号**

类似pid标识一个进程；在网络程序中，用端口号（port）来标识一个运行的网络程序。

特点  
1、端口号是无符号短整型的类型  
2、每个端口都拥有一个端口号  
3、TCP、UDP维护各自独立的端口号  
4、网络应用程序,至少要占用一个端口号,也可以占有多个端口号

> 想一想 为什么有了pid，还需要端口来标识一个进程呢？——应用程序启动一下进程号就会变

- 知名端口（1~1023）——我们不要用这些端口号  
    由互联网数字分配机构(IANA)根据用户需要进行统一分配  
    例如：FTP—21，HTTP—80等  
    服务器通常使用的范围;若强制使用,须加root特权
- 动态端口（1024~65535）——我们随便用  
    应用程序通常使用的范围

注意  
端口号类似于进程号，同一时刻只能标志一个进程，可以重复使用。

### 1.4数据包的组装、拆解

#### 1.4.1数据包在各个层之间的传输

![在这里插入图片描述](https://img-blog.csdnimg.cn/db5cb3d7ebc04ca8a8254486a650c43e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

飞秋聊天软件为例，飞秋是局域网聊天软件，走UDP协议，端口号为2425：

![在这里插入图片描述](https://img-blog.csdnimg.cn/3ba0babe1cbd492fba6a13f317184194.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

#### 1.4.2链路层封包格式

![在这里插入图片描述](https://img-blog.csdnimg.cn/0c7a4bc21a394255864983dbedfefb10.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

注意  
1、IEEE802.2/802.3封装常用在无线  
2、以太网封装常用在有线局域网

#### 1.4.3网络层、传输层封包格式

![在这里插入图片描述](https://img-blog.csdnimg.cn/ee44d91c99a143f7a8f54489e43b4c66.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

### 报头介绍

#### MAC头部

目的MAC、源MAC  
![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/72f22e68c1ca4eacb150ddce6668598e.png)  
1.CRC、PAD 在组包时可以忽略  
2.FCS  
CRC即循环冗余校验码：是数据通信领域中最常用的一种查错校验码，其特征是信息字段和  
校验字段的长度可以任意选定。循环冗余检查是一种数据传输检错功能，对数据进行h多项  
式计算，并将得到的结果附在帧的后面，接收设备也执行类似的算法，以保证数据传输的正  
确性和完整性。

#### IP报头

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/39ce2eeeee1b44dbb0b750db55392668.png)  
1.版本：IP协议的版本。通信双方使用过的IP协议的版本必须一致，目前最广泛使用的IP协议版本号为4（即IPv4 )  
2.首部长度：单位是32位（4字节）  
3.服务类型：一般不适用，取值为0。前3位：优先级，第4­7位：延时，吞吐量，可靠性，花费。第8位保留  
4.总长度：指首部加上数据的总长度，单位为字节。最大长度为65535字节。  
5.标识（identification）：用来标识主机发送的每一份数据报。IP软件在存储器中维持一个计数器，每产生一个数据报，计数器就加1，并将此值赋给标识字段。  
6.标志（flag）：目前只有两位有意义。  
标志字段中的最低位记为MF。MF=1即表示后面“还有分片”的数据报。MF=0表示这已是若干数据报片中的最后一个。  
标志字段中间的一位记为DF，意思是“不能分片”，只有当DF=0时才允许分片  
7.片偏移：指出较长的分组在分片后，某片在源分组中的相对位置，也就是说，相对于用户数据段的起点，该片从何处开始。片偏移以8字节为偏移单位。  
8.生存时间(经过多少路由器)：TTL，表明是数据报在网络中的寿命，即为“跳数限制”，由发出数据报的源点设置这个字段。路由器在转发数据之前就把TTL值**减一**，当TTL值减为零时，就丢弃这个数据报。通常设置为32、64、128。

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/e5039004c78a4bf683e1929a7835bd21.png)

> 当TTL减到0的时候，这个数据包就会从网络上消失，路由器不再转发。

9.协议：指出此数据报携带的数据时使用何种协议，以便使目的主机的IP层知道应将数据部分上交给哪个处理过程，常用的ICMP(1),IGMP(2),TCP(6),UDP(17),IPv6（41）  
10.首部校验和：只校验数据报的首部，不包括数据部分。  
11.源地址：发送方IP地址  
12.目的地址：接收方IP地址  
13.选项：用来定义一些任选项；如记录路径、时间戳等。这些选项很少被使用，同时并不是所有主机和路由器都支持这些选项。一般忽略不计。

#### UDP报头

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/c17d43dc77c2461f93741235509353e2.png)

1.源端口号：发送方端口号  
2.目的端口号：接收方端口号  
3.长度：UDP用户数据报的长度，最小值是8（仅有首部）  
4.校验和：检测UDP用户数据报在传输中是否有错，有错就丢弃

#### TCP报头

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/6cf4b7649aad46f2b42f848baadc25df.png)

1.源端口号：发送方端口号  
2.目的端口号：接收方端口号  
3.序列号：本报文段的数据的第一个字节的序号  
4.确认序号：期望收到对方下一个报文段的第一个数据字节的序号  
5.首部长度（数据偏移）：TCP报文段的数据起始处距离TCP报文段的起始处有多远，即首部长度。单位：32位，即以4字节为计算单位。  
6.保留：占6位，保留为今后使用，目前应置为0  
7.紧急URG: 此位置1，表明紧急指针字段有效，它告诉系统此报文段中有紧急数据，应尽快传送  
8.确认ACK: 仅当ACK=1时确认号字段才有效，TCP规定，在连接建立后所有传达的报文段都必须把ACK置1  
9.推送PSH：当两个应用进程进行交互式的通信时，有时在一端的应用进程希望在键入一个命令后立即就能够收到对方的响应。在这种情况下，TCP就可以使用推送（push）操作，这时，发送方TCP把PSH置1，并立即创建一个报文段发送出去，接收方收到PSH=1的报文段，就尽快地（即“推送”向前）交付给接收应用进程，而不再等到整个缓存都填满后再向上交付  
10.复位RST: 用于复位相应的TCP连接  
11.同步SYN: 仅在三次握手建立TCP连接时有效。当SYN=1而ACK=0时，表明这是一个连接请求报文段，对方若同意建立连接，则应在相应的报文段中使用SYN=1和ACK=1.因此，SYN置1就表示这是一个连接请求或连接接受报文  
12.终止FIN：用来释放一个连接。当FIN=1时，表明此报文段的发送方的数据已经发送完毕，并要求释放运输连接。  
13.窗口尺寸：指发送本报文段的一方的接收窗口（而不是自己的发送窗口），告知对方我的缓冲区还有多大，后面你给我发的时候不要超过这个长度，流量控制。  
14.校验和：校验和字段检验的范围包括首部和数据两部分，在计算校验和时需要加上12字节的伪头部  
15.紧急指针：仅在URG=1时才有意义，它指出本报文段中的紧急数据的字节数（紧急数据结束后就是普通数据），即指出了紧急数据的末尾在报文中的位置，  
注意：即使窗口为零时也可发送紧急数据  
16.选项：长度可变，最长可达40字节，当没有使用选项时，TCP首部长度是20字节

#### ARP头部

通过广播，获取指定节点的MAC地址。

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/449e2ca21d244e3ba4b2abedba36864a.png)

1.Dest MAC:目的MAC地址  
2.Src MAC：源MAC地址  
3.帧类型：0x0806  
4.硬件类型：1（以太网）  
5.协议类型：0x0800（IP地址）  
6.硬件地址长度：6  
7.协议地址长度：4  
8.OP：1（ARP请求），2（ARP应答），3（RARP请求），4（RARP应答）

### 1.5网络应用程序开发流程

#### 1.5.1 TCP—面向连接

![在这里插入图片描述](https://img-blog.csdnimg.cn/d83bbe81feef46b99b5479fa079b1540.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_16,color_FFFFFF,t_70,g_se,x_16)

**电话系统服务模式的抽象**

每一次完整的数据传输都要经过建立连接、使用连接、终止连接的过程  
本质上,连接是一个管道,收发数据不但顺序一致,而且内容相同  
保证数据传输的可靠性(出错重传)

![在这里插入图片描述](https://img-blog.csdnimg.cn/ba6e07aa3a0741629187bbb2c6dac18a.png)

#### 1.5.2 UDP — 面向无连接

**邮件系统服务模式的抽象**

每个分组都携带完整的目的地址

- 不能保证分组的先后顺序
- 不进行分组出错的恢复和重传
- 不保证数据传输的可靠性

![在这里插入图片描述](https://img-blog.csdnimg.cn/d08552edcc1e4e248b0f3ac0d3e569b2.png)

> UDP不用建立连接，每次传输数据不用给出回复，不用管对方收不收到，没有出错重传(可以在应用层解决，人为让它可靠)，速度相对TCP会快一些。数据发送出去，是否收到不管，不像TCP出错重发。UDP数据传输多经过几个路由器丢包率就比较高了，在局域网里面是不会丢包的。

无论采用面向连接的还是无连接，两个进程通信过程中，大多采用C/S架构  
client向server发出请求,server接收到后提供相应的服务  
在通信过程中往往都是client先发送请求，而server等待请求然后进行服务

![在这里插入图片描述](https://img-blog.csdnimg.cn/4f339c07f57948688f13bfb0fdcb1d4d.png)

#### 1.5.3 C/S B/S架构示例 — 面向连接

![在这里插入图片描述](https://img-blog.csdnimg.cn/6ef3128dd01c4c13b3bc9d30025ae7f5.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_19,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/3e712111672a487cb5d9da71d9e1f965.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

server工作过程  
打开一通信通道并告知本地主机,它愿意在一特定端口(如80)上接收客户请求  
等待客户请求到达该端口  
接收客户请求，并发送应答信号,激活一新的线程处理这个客户请求  
服务完成后,关闭新线程与客户的通信链路

client工作过程  
打开一通信通道并连接到服务器特定端口  
向服务器发出服务请求,等待并接收应答  
根据需要继续提出请求  
请求结束后关闭通信通道并终止

## 第二章：UDP编程

### 2.1编程准备-字节序、地址转换

#### 2.1.1字节序概述

字节序概念  
是指多字节数据的存储顺序

分类

- 小端格式:将低位字节数据存储在低地址
- 大端格式:将高位字节数据存储在低地址

注意

- LSB：低地址
- MSB：高地址

想一想  
怎样确定主机的字节序？

![在这里插入图片描述](https://img-blog.csdnimg.cn/f68bf6a8cd6f4124a246067e8622b6c8.png)

确定主机字节序程序

```c
#include<stdio.h>
union{			
	unsigned short s;
	unsigned char c[sizeof(short)];
}un;

int main()
{
	un.s =0x0102;
	if((un.c[0]==1)&&(un.c[1]==2))	// 共用体，多个成员共用一块空间   c[0]是低地址，c[2]是高地址
	{
		printf("big-endian\n");
	}
	if((un.c[0]==2)&&(un.c[1]==1))
	{
		printf("little-endian\n");
	}
	return 0;
}
```

特点  
1、网络协议指定了通讯字节序—大端  
2、只有在多字节数据处理时才需要考虑字节序  
3、运行在同一台计算机上的进程相互通信时,一般不用考虑字节序  
4、异构计算机之间通讯，需要转换自己的字节序为网络字节序

在需要字节序转换的时候一般调用特定字节序转换函数

#### 2.1.2 htonl函数

```c
uint32_t htonl(uint32_t hostint32);
功能:
将32位主机字节序（小端/大端）数据转换成网络字节序（大端）数据
参数：
hostint32：待转换的32位主机字节序数据
返回值：
成功：返回网络字节序的值
头文件：
#include <arpa/inet.h>
```

例如

```c
#include <stdio.h>
#include <arpa/inet.h>
int main(int argc, char *argv[])
{
	int  num = 0x01020304;//
	short  a = 0x0102;
	int sum = htonl(num);
	printf("%x\n",sum);
	short b = htons(a);

	printf("%x\n",b);
	return 0;
}
```

#### 2.1.3 htons函数

```c
uint16_t htons(uint16_t hostint16);
功能：
将16位主机字节序数据转换成网络字节序数据
参数：
uint16_t：unsigned short int
hostint16：待转换的16位主机字节序数据
返回值：
成功：返回网络字节序的值
头文件：
#include <arpa/inet.h>
```

#### 2.1.4 ntohl函数

```c
uint32_t ntohl(uint32_t netint32);
功能：
将32位网络字节序数据转换成主机字节序数据
参数：
uint32_t： unsigned int
netint32：待转换的32位网络字节序数据
返回值：
成功：返回主机字节序的值
头文件：
#include <arpa/inet.h>
```

```c
#include <stdio.h>
#include <arpa/inet.h>
int main(int argc, char *argv[])
{
	int  num = 0x01020304;//
	int sum = htonl(num);

	printf("%x\n",ntohl(sum));

	return 0;
}
```

#### 2.1.5 ntohs函数

```c
uint16_t ntohs(uint16_t netint16);
功能：
将16位网络字节序数据转换成主机字节序数据
参数：
uint16_t： unsigned short int
netint16：待转换的16位网络字节序数据
返回值：
成功：返回主机字节序的值
头文件：
#include <arpa/inet.h>
```

```c
#include <stdio.h>
#include <arpa/inet.h>
int main(int argc, char *argv[])
{
	short  a = 0x0102;
	short b = htons(a);
	printf("%x\n",ntohs(b));
	return 0;
}
```

示例：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/b1cc28aa4afa4f858621f26f3dba99a3.png)

结果：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/dcb67c2c2d604af89ef660b8911f8f6d.png)

#### 2.1.6地址转换函数

**2.1.6.1 inet_pton函数**

```c
字符串ip地址转整型数据
int inet_pton(int family,const char *strptr, void *addrptr);

功能：
将点分十进制数串转换成32位无符号(大端)整数
参数：
family	 协议族  选IPV4对应的宏AF_INET
strptr 	 待转换的点分十进制数串首地址
addrptr  转换后的32位无符号整数的地址
返回值：
成功返回1 、 失败返回其它
头文件：
#include <arpa/inet.h>
```

例:

```c
#include <stdio.h>
#include <arpa/inet.h>
int main(int argc,char *argv[])
{
	char ip_str[] = "10.0.13.100";
	unsigned int ip_uint = 0;
	unsigned char * ip_p =NULL;//可以用char吗？不可以   
	
	inet_pton(AF_INET,ip_str,&ip_uint);
	printf("ip_uint = %d\n",ip_uint);
	
	ip_p = (unsigned char *) &ip_uint;
	printf("ip_uint = %d.%d.%d.%d\n",*ip_p,*(ip_p+1),*(ip_p+2),*(ip_p+3));
	
	return 0;
}
```

运行结果：

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/3fc0eb2fb1764e0d8e101bb0b8c2fc52.png)

**2.1.6.2inet_ntop函数**

```c
整型数据转字符串格式ip地址
const char *inet_ntop(int family, const void *addrptr,char *strptr, size_t len);
功能：
将32位无符号整数转换成点分十进制数串
参数：
family	   协议族
addrptr	   32位无符号整数
strptr	   点分十进制数串
len	   	   strptr缓存区长度
len的宏定义
#define INET_ADDRSTRLEN   16  //for ipv4
#define INET6_ADDRSTRLEN  46  //for ipv6
返回值：
成功:则返回字符串的首地址
失败:返回NULL
头文件：
#include <arpa/inet.h>
```

例：

```c
#include<stdio.h>
#include<arpa/inet.h>
int main()
{
	unsigned char ip[]={10,0,13,252};
	char ip_str[16];
	inet_ntop(AF_INET,(unsigned int *)ip,ip_str,16);
	printf("ip_str = %s\n",ip_str);
	return 0;
}
```

运行结果：

![在这里插入图片描述](https://img-blog.csdnimg.cn/ab8a9586de45456caeab16360ac8614b.png)

### 2.2 UDP介绍、编程流程

#### 2.2.1 UDP概述

UDP协议  
面向无连接的用户数据报协议，在传输数据前不需要先建立连接；目地主机的运输层收到UDP报文后，不需要给出任何确认  
UDP特点  
1、相比TCP速度稍快些  
2、简单的请求/应答应用程序可以使用UDP  
3、对于海量数据传输不应该使用UDP  
4、广播和多播应用必须使用UDP  
UDP应用  
DNS(域名解析)、NFS(网络文件系统)、RTP(流媒体)等

#### 2.2.2网络编程接口socket

网络通信要解决的是不同主机进程间的通信，而管道、消息队列这些是同一个主机进程间通信，实现不了。

1、首要问题是网络间进程标识问题  
2、以及多重协议的识别问题

20世纪80年代初，加州大学Berkeley分校在BSD(一个UNIX OS版本)系统内实现了TCP/IP协议；其网络程序编程开发接口为socket  
随着UNIX以及类UNIX操作系统的广泛应用， socket成为最流行的网络程序开发接口。

socket作用  
提供不同主机上的进程之间的通信  
socket特点  
1、socket也称“套接字”  
2、是一种文件描述符,代表了一个通信管道的一个端点  
3、类似对文件的操作一样，可以使用read、write、close等函数对socket套接字进行网络数据的收取和发送等操作，也可以用sendto、recvfrom。  
4、得到socket套接字（描述符）的方法调用socket()

#### 2.2.3 UDP编程C/S架构流程图

![在这里插入图片描述](https://img-blog.csdnimg.cn/e714e75fb02c4ee197c3fb26eda9d607.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_13,color_FFFFFF,t_70,g_se,x_16)

- 服务端要绑定确定的端口号
- 客户端不能使用read、write，因为需要发送给指定的服务端地址。

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/d10165d74a8247ada781ba15b467b60d.png)

### 2.3 UDP编程-创建套接字

#### 2.3.1创建socket套接字

```c
int socket(int family,int type,int protocol);

功能：
创建一个用于网络通信的socket套接字（描述符）
参数：
family:协议族(AF_INET、AF_INET6、PF_PACKET等)

             /流式套接字 用于TCP通信 	/报式套接字 用于UDP通信		/原始套接字
type:套接字类( SOCK_STREAM、         	SOCK_DGRAM、            	SOCK_RAW等)

protocol:协议类别(0、	IPPROTO_TCP、	IPPROTO_UDP等)
                /0自动指定协议
返回值：
套接字
特点：
创建套接字时，系统不会分配端口
创建的套接字默认属性是主动的，即主动发起服务的请求;当作为服务器时，往往需要修改为被动的
头文件：
#include <sys/socket.h>
```

#### 2.3.2创建UDP套接字demo

```c
int sockfd = 0;
sockfd = socket(AF_INET,SOCK_DGRAM,0);
if(sockfd < 0)
{
	perror("socket");
	exit(-1);
}


注意：
AF_INET:IPv4协议
SOCK_DGRAM：数据报套接字
0：选择所给定的family和type组合的系统默认值
```

### 2.4 UDP编程-发送、绑定、接收数据

#### 2.4.1 IPv4套接字地址结构

```c
头文件：#include <netinet/in.h>
struct in_addr
{
	in_addr_t s_addr;//4字节
};
struct sockaddr_in
{
	sa_family_t sin_family;//2字节
	in_port_t sin_port;//2字节
	struct in_addr sin_addr;//4字节
	char sin_zero[8]//8字节
};
为了使不同格式地址能被传入套接字函数,地址须要强制转换成通用套接字地址结构
头文件:#include <netinet/in.h>
struct sockaddr
{
	sa_family_t sa_family;	// 2字节
	char sa_data[14]	//14字节
};
```

注意：  
以上3个结构在linux系统中已经定义

#### 2.4.3两种地址结构使用场合

```c
在定义源地址和目的地址结构的时候，选用struct sockaddr_in;
例：
struct  sockaddr_in  my_addr;

当调用编程接口函数，且该函数需要传入地址结构时需要用struct sockaddr进行强制转换
例：
bind(sockfd,(struct sockaddr*)&my_addr,sizeof(my_addr));
```

#### 2.4.4发送数据—sendto函数

```c
ssize_t sendto(int sockfd，
			   const void *buf,
               size_t nbytes,
               int flags,
               const struct sockaddr *to,        
               socklen_t addrlen);
功能：
向to结构体指针中指定的ip，发送UDP数据
参数：
sockfd：套接字
buf：	发送数据的地址
nbytes: 发送数据的大小

flags： 一般为0
to：	指向目的主机地址结构体的指针
addrlen：to所指向内容的长度

注意：
通过to和addrlen确定目的地址
可以发送0长度的UDP数据包
返回值：
成功:发送数据的字符数
失败: -1
```

#### 2.4.5向“网络调试助手”发送消息

![在这里插入图片描述](https://img-blog.csdnimg.cn/afbb95dcbb0d4fbb9f5e16a48947faaa.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/4adae1790f7c420d9db4665bb8b2d769.png)

例：udp_send.c

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>
int main(int argc,char *argv[])
{
	unsigned short port = 8080;
	char *server_ip = "10.0.31.245";
	if(argc > 1)//通过main函数传参，传入ip地址
	{
		server_ip = argv[1];
	}
	if(argc > 2)//通过main函数传参，传入端口号
	{
		port = atoi(argv[2]);
	}
	
	/*创建UDP套接字*/
	int sockfd;
	sockfd = socket(AF_INET,SOCK_DGRAM,0);
	if(sockfd < 0)
	{
		perror("socket");
		exit(-1);
	}
	
	/*填充目的server socket地址*/
	struct sockaddr_in dest_addr;
	bzero(&dest_addr,sizeof(dest_addr));
	dest_addr.sin_family = AF_INET;//目的套接字地址的协议家族赋值
	dest_addr.sin_port = htons(port);//目的套接字地址的端口号赋值
	inet_pton(AF_INET,server_ip,&dest_addr.sin_addr);//目的套接字地址的ip地址赋值
	
	printf("send data to UDP server %s:%d!\n",server_ip,port);
	
	/*发送数据到目的server*/
	while(1)
	{
		char send_buf[512];
		fgets(send_buf,sizeof(send_buf),stdin);
		send_buf[strlen(send_buf)-1] = '\0';//字符串最后一个'\n'变成'\0'
		sendto(sockfd,send_buf,strlen(send_buf),0,(struct sockaddr *)&dest_addr,sizeof(dest_addr));
	}
	close(sockfd);
	return 0;
}
```

#### 2.4.6绑定 bind函数

UDP网络程序想要收取数据需什么条件？  
确定的ip地址  
确定的port  
怎样完成上面的条件呢？  
接收端 使用bind函数，来完成地址结构与socket套接字的绑定，这样ip、port就固定了  
发送端 在sendto函数中指定接收端的ip、port，就可以发送数据了

```c
int bind(int sockfd,const struct sockaddr *myaddr，socklen_t addrlen);
功能：
将本地协议地址与sockfd绑定

参数：
sockfd： socket套接字
myaddr： 指向特定协议的地址结构指针
addrlen：该地址结构的长度

返回值：
成功：返回0
失败：其他
```

#### 2.4.7 bind示例

![在这里插入图片描述](https://img-blog.csdnimg.cn/29e98338871541eeb510fd10dc543f79.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_15,color_FFFFFF,t_70,g_se,x_16)

注意：  
INADDR_ANY 通配地址,值为0  
头文件：  
```
#include <sys/socket.h>
```

#### 2.4.8接收数据—recvfrom 函数

```c
ssize_t recvfrom(int sockfd, void *buf,size_t nbytes,int flags,
					struct sockaddr *from,socklen_t *addrlen);
功能：
接收UDP数据，并将源地址信息保存在from指向的结构中

参数：
sockfd:	套接字
buf：	接收数据缓冲区地址
nbytes: 接收数据缓冲区的大小
flags：	套接字标志(常为0)
from：	源地址结构体指针，用来保存数据的来源
addrlen: from所指内容的长度

注意：
通过from和addrlen参数存放数据来源信息
from和addrlen可以为NULL, 表示不保存数据来源
返回值：
成功:接收到的字符数
失败: -1
```

#### 2.4.9接收“网络调试助手”的数据

![在这里插入图片描述](https://img-blog.csdnimg.cn/a0578f112a484593842efffb12423c99.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/2a3d5872e6524a8b86a9e17c31ab9f83.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/85c1913a74bf4882bb41c5282fd27527.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_18,color_FFFFFF,t_70,g_se,x_16)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/f072024b20024960afa60035ba0e3d65.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_18,color_FFFFFF,t_70,g_se,x_16)

### 2.5 UDP编程代码编写-client、server

想一想：  
上节中的2个demo中发送数据端其实就是client；接收数据端就是server，那么client能否接收数据？server能否发送数据呢？  
答：  
其实在网络编程开发中client和server双方既可以有发送数据还可以接收数据；一般认为提供服务的一方为server；而接受服务的另一方为client

#### 服务端代码

```c
#include <stdio.h>
#include <arpa/inet.h>
#include <sys/socket.h>

int main(int argc, char *argv[])
{
	//创建套接字
	int sock_fd = socket(AF_INET,SOCK_DGRAM,0);
	if(sock_fd < 0)
		perror("");

	//绑定
	struct sockaddr_in addr;
	addr.sin_family = AF_INET;
	addr.sin_port = htons(9090);
	//inet_pton(AF_INET,"192.168.127.129",&addr.sin_addr.s_addr);
	addr.sin_addr.s_addr = 0;// 通配地址  主机上所有IP都绑定套接字
	int ret = bind(sock_fd,(struct sockaddr*)&addr,sizeof(addr));
	if(ret < 0)
		perror("");
	struct sockaddr_in cli_addr;
	socklen_t len = sizeof(cli_addr);
	while(1)
	{
		char buf[128]="";
													// 保存客户端的信息		   此处不能写&sizeof(cli_addr),常量不能取地址
		int n = recvfrom(sock_fd,buf,sizeof(buf),0,(struct sockaddr*)&cli_addr,&len);
		printf("%s\n",buf);
		sendto(sock_fd,buf,n,0,(struct sockaddr*)&cli_addr,sizeof(cli_addr));
	
	}

	close(sock_fd);



	return 0;
}
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/005ce29d3b834138ba429c5efe7d3ebd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

#### 客户端(无连接connect)

```c
#include <stdio.h>
#include <arpa/inet.h>
#include <sys/socket.h>
#include <string.h>
#include <unistd.h>
#include <sys/types.h>

int main(int argc, char *argv[])
{
	struct sockaddr_in server_addr;
	server_addr.sin_family = AF_INET;
	server_addr.sin_port = htons(9000);
	inet_pton(AF_INET,"192.168.127.1",&server_addr.sin_addr.s_addr);
	//创建套接字
	int sock_fd = socket(AF_INET,SOCK_DGRAM,0);
	if(sock_fd< 0)
		perror("");
	while(1)
	{
		char buf[128]="";
		fgets(buf,sizeof(buf),stdin);
		buf[strlen(buf)-1]=0;
		sendto(sock_fd,buf,strlen(buf),0,(struct sockaddr*) &server_addr,sizeof(server_addr));
		char read_buf[128]="";
		recvfrom(sock_fd,read_buf,sizeof(read_buf),0,NULL,NULL);
		printf("%s\n",read_buf);
	
	
	}
	return 0;
}
```

#### 抓包工具wireshark

视频里有讲

#### UDP客户端注意点

1、本地IP、本地端口（我是谁）  
2、目的IP、目的端口（发给谁）  
3、在客户端的代码中，我们只设置了目的IP、目的端口

客户端的本地ip、本地port是我们调用sendto的时候linux系统底层自动给客户端分配的；分配端口的方式为随机分配，即每次运行系统给的port不一样

#### UDP服务器注意点

1、服务器之所以要bind是因为它的本地port需要是固定，而不是随机的  
2、服务器也可以主动地给客户端发送数据  
3、客户端也可以用bind，这样客户端的本地端口就是固定的了，但一般不这样做

## 第三章：UDP编程 - TFTP、广播、多播

### 3.1 TFTP简介、通信过程

#### 3.1.1 TFTP概述

- TFTP：简单文件传送协议(默认端口号：69)  
    最初用于引导无盘系统，被设计用来传输小文件(一般用于==局域网内==上传下载文件，如嵌入式刷系统、网吧)
    
- 特点：  
    基于UDP协议实现(==在receive和send函数里加上TFTP协议头==)  
    不进行用户有效性认证
    
- 数据传输模式：  
    octet：**二进制模式**  
    netascii：文本模式  
    mail：已经不再支持
    

#### 3.1.2 TFTP通信过程

这是从服务端下载文件过程：

![在这里插入图片描述](https://img-blog.csdnimg.cn/91668d09152b4c409300960407b1a43f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_13,color_FFFFFF,t_70,g_se,x_16)

TFTP通信过程总结（无选项）  
1、服务器在**69**号端口等待客户端的请求  
2、服务器若批准此请求,则使用**临时端口**与客户端进行通信  
3、每个数据包的编号都有变化（从**1**开始）  
4、每个数据包都要得到ACK的确认，如果出现**超时**，则需要重新发送最后的包（数据或ACK）。本身UDP是不可靠协议，但是TFTP应用层协议里加入超时时间和重传次数，把数据传输变得可靠。  
5、数据的长度以512Byte传输(不包括协议长度)  
6、小于512Byte的数据意味着传输**结束**，如果最后一包正好是512，则再追加发送一个0字节长度数据包。

> TFTP服务器软件：  
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/455aeb84417b4b1ab036a98d418b4c90.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_12,color_FFFFFF,t_70,g_se,x_16)

想一想  
TFTP协议的格式是什么样子？

### 3.2 TFTP协议分析

![在这里插入图片描述](https://img-blog.csdnimg.cn/76da58472a144cbabfefff9efa73fcb1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

例如下载服务器的a.txt文件过程：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/70b244b3eedc413d8269f032450da3ad.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

注意：  
以上的0代表的是’\0’  
不同的差错码对应不同的错误信息

![在这里插入图片描述](https://img-blog.csdnimg.cn/372148a63cf74fb5a8ad68a4553d8415.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/eafaa7ec174b468f823a6100bc5d2603.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

错误码：  
0 未定义,参见错误信息  
1 File not found.  
2 Access violation.  
3 Disk full or allocation exceeded.  
4 illegal TFTP operation.  
5 Unknown transfer ID.  
6 File already exists.  
7 No such user.  
8 Unsupported option(s) requested.

想一想  
传输的数据的大小一定是512Byte吗？  
由于网络的原因,一方收不到另一方的数据怎么办？

#### TFTP带选项和OACK

读写请求中修改了选项  
![在这里插入图片描述](https://img-blog.csdnimg.cn/3b37748e051e4a56b5723ac9bef2b252.png)

如果发送带选项的读写请求  
![在这里插入图片描述](https://img-blog.csdnimg.cn/432297da2fe24225a88a8b51d78c5288.png)

- tsize选项  
    当读操作时，tsize选项的参数必须为“**0**”，服务器会返回待读取的文件的大小  
    当写操作时，tsize选项参数应为待写入文件的大小，服务器会回显该选项
- blksize选项  
    修改传输文件时使用的数据块的大小（范围：8～65464）
- timeout选项  
    修改默认的数据传输超时时间（单位：秒）

![在这里插入图片描述](https://img-blog.csdnimg.cn/62dc11863edc43cab45227855130518f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

TFTP通信过程总结（带选项）  
1、可以通过发送带选项的读/写请求发送给server  
2、如果server允许修改选项则发送选项修改确认包  
3、server发送的数据、选项修改确认包都是临时port  
4、server通过timeout来对丢失数据包的重新发送

### 3.3 编程练习 — TFTP客户端下载上传文件(不带选项)

练习要求  
使用TFTP协议，下载server上的文件到本地  
实现思路  
![在这里插入图片描述](https://img-blog.csdnimg.cn/f20c5eae506744e78c55639961037fda.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

1、构造请求报文，送至服务器(69号端口)  
2、等待服务器回应  
3、分析服务器回应  
4、接收数据,直到接收到的数据包小于规定数据长度

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <fcntl.h>
#include <string.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>
#include <signal.h>
#include <termios.h>

#define GREEN 	"\e[32m"   //shell打印显示绿色
#define RED     "\e[31m"   //shell打印显示红色
#define PRINT(X,Y) {	write(1,Y,5);  \
					printf(X);   \
					fflush(stdout); \
				    write(1,"\e[0m",4); \
				 }

static int sockfd;
static struct sockaddr_in dest_addr;

void sig_dispose(int sig)
{
	if(SIGINT == sig){
		close(sockfd);
		puts("\nclose!");
		system("stty sane");//回显
		exit(0);
	}
}

//上传
void tftp_upload(char *argv)
{
	int fd,read_len;
	unsigned short p_num = 0;
	unsigned char cmd = 0;
	char cmd_buf[512] = "";
	char recv_buf[516] = "";
	struct sockaddr_in client_addr;
	socklen_t cliaddr_len = sizeof(client_addr);

	if(dest_addr.sin_port == 0){
		dest_addr.sin_family = AF_INET;
		dest_addr.sin_port = htons(69); 
		puts("send to IP:");
		fgets(recv_buf,sizeof(recv_buf),stdin);
		*(strchr(recv_buf,'\n')) = '\0';
		inet_pton(AF_INET, recv_buf, &dest_addr.sin_addr);
	}
	
	//构造上传请求,argv为文件名
	int len = sprintf(cmd_buf, "%c%c%s%c%s%c", 0, 2, argv, 0, "octet", 0);	//发送读数据包请求
	sendto(sockfd, cmd_buf, len, 0, (struct sockaddr *)&dest_addr, sizeof(dest_addr));
	
	fd = open(argv, O_RDONLY);
	if(fd < 0 ){
		perror("open error");
		close(sockfd);
		exit(-1);
	}
	
	do{
		//接收服务器发送的内容
		len = recvfrom(sockfd, recv_buf, sizeof(recv_buf), 0, (struct sockaddr*)&client_addr, &cliaddr_len);
		
		cmd = recv_buf[1];
		if( cmd == 4 )	//是否为ACK
		{
			p_num = ntohs(*(unsigned short*)(recv_buf+2));
			read_len = read(fd, recv_buf+4, 512);
			printf("recv:%d\n", p_num);//十进制方式打印包编号		
			recv_buf[1] = 3;//构建数据包
			(*(unsigned short *)(recv_buf+2)) = htons(p_num + 1);
			//printf("%s\n", recv_buf+3);
			sendto(sockfd, recv_buf, read_len+4, 0, (struct sockaddr*)&client_addr, sizeof(client_addr));
		}
		else if( cmd == 5 ) //是否为错误应答
		{
			close(sockfd);
			close(fd);
			printf("error:%s\n", recv_buf+4);
			exit(-1);
		}		
	}while(read_len == 512); //读取的数据小于512则认为结束
	len = recvfrom(sockfd, recv_buf, sizeof(recv_buf), 0, (struct sockaddr*)&client_addr, &cliaddr_len);      //接收最后一个ACK确认包
	close(fd);
	PRINT("Download File is Successful\n", RED);
	return;
}


//下载
void tftp_down(char *argv)
{
	int fd;
	unsigned short p_num = 0;
	unsigned char cmd = 0;
	char cmd_buf[512] = "";
	char recv_buf[516] = "";
	struct sockaddr_in client_addr;
	socklen_t cliaddr_len = sizeof(client_addr);

	if(dest_addr.sin_port == 0){
		dest_addr.sin_family = AF_INET;
		dest_addr.sin_port = htons(69); 
		puts("send to IP:");
		fgets(recv_buf,sizeof(recv_buf),stdin);
		*(strchr(recv_buf,'\n')) = '\0';
		inet_pton(AF_INET, recv_buf, &dest_addr.sin_addr);
	}
	
	//构造下载请求报文,argv为文件名
	int len = sprintf(cmd_buf, "%c%c%s%c%s%c", 0, 1, argv, 0, "octet", 0);	//发送读数据包请求
	sendto(sockfd, cmd_buf, len, 0, (struct sockaddr *)&dest_addr, sizeof(dest_addr));
	
	fd = open(argv, O_WRONLY|O_CREAT, 0666);
	if(fd < 0 ){
		perror("open error");
		close(sockfd);
		exit(-1);
	}
	
	do{
		//接收服务器发送的内容			4字节报头+512字节数据			回数据的时候要用这个，上面那个端口是请求时用的，实际通讯服务器会开辟一个临时端口
		len = recvfrom(sockfd, recv_buf, sizeof(recv_buf), 0, (struct sockaddr*)&client_addr, &cliaddr_len);
		
		cmd = recv_buf[1];
		if( cmd == 3 )	//是否为数据包
		{
			//接收的包编号是否为上次包编号+1
			if((unsigned short)(p_num+1) == ntohs(*(unsigned short*)(recv_buf+2) ))
			{
				write(fd, recv_buf+4, len-4);
				p_num = ntohs(*(unsigned short*)(recv_buf+2));
				
				printf("recv:%d\n", p_num);//十进制方式打印包编号
			}			
			recv_buf[1] = 4;
			sendto(sockfd, recv_buf, 4, 0, (struct sockaddr*)&client_addr, sizeof(client_addr));
		}
		else if( cmd == 5 ) //是否为错误应答
		{
			close(sockfd);
			close(fd);
			unlink(argv);//删除文件
			printf("error:%s\n", recv_buf+4);
			exit(-1);
		}		
	}while((len == 516)||(cmd == 6)); //如果收到的数据小于516则认为出错
	close(fd);
	PRINT("Download File is Successful\n", RED);
	return;
}
void help_fun(int argc, char *argv[])
{
	printf("1  down\n");
	printf("2  upload\n");
	printf("3  exit\n");
	return;
}

char mygetch() 
{
    struct termios oldt, newt;
    char ch;
    tcgetattr( STDIN_FILENO, &oldt );
    newt = oldt;
    newt.c_lflag &= ~( ICANON | ECHO );
    tcsetattr( STDIN_FILENO, TCSANOW, &newt );
    ch = getchar();
    tcsetattr( STDIN_FILENO, TCSANOW, &oldt );
    return ch;
}

int main(int argc, char *argv[])
{
	char cmd_line[100];
	signal(SIGINT,sig_dispose);
	
	bzero(&dest_addr, sizeof(dest_addr));
	sockfd = socket(AF_INET, SOCK_DGRAM, 0);
	if(sockfd < 0){
		perror("socket error");
		exit(-1);
	}
	while(1){
		int cmd;
		help_fun(argc,argv);
		PRINT("send>", GREEN);
		cmd = mygetch();
		if(cmd == '1'){
			puts("input file name:");
			fgets(cmd_line,sizeof(cmd_line),stdin);
			*(strchr(cmd_line,'\n')) = '\0';
			tftp_down(cmd_line);
		}
		else if(cmd == '2')	{
			puts("input file name:");
			fgets(cmd_line,sizeof(cmd_line),stdin);
			*(strchr(cmd_line,'\n')) = '\0';
			tftp_upload(cmd_line);
		}			
		else if(cmd == '3')	{
			close(sockfd);			
			system("stty sane");//回显
			exit(0);
		}
	}
	
	close(sockfd);
	return 0;
}
```

### 3.4 UDP广播

![在这里插入图片描述](https://img-blog.csdnimg.cn/37d2dd1de0344cd1939066b4b40904d7.png)

广播：由一台主机向该主机所在子网内的所有主机发送数据的方式。  
广播只能用UDP或原始IP实现，==不能用TCP==。

#### 3.4.1广播的用途

单个服务器与多个客户主机通信时减少分组流通  
以下几个协议都用到广播

- 1、地址解析协议（ARP，寻找IP地址对应的MAC地址，链路层需要MAC地址组包）
- 2、动态主机配置协议（DHCP，向路由器里的DHCP服务器申请IP地址，发送广播谁是DHCP服务器请给我一个IP）
- 3、网络时间协议（NTP）

> 下图是ARP的表，记录了IP地址对应的MAC地址。如果表里有一个IP对应的MAC地址没有记录，则通过发广播去问，局域网里所有IP都收到，只有指定的那个IP回复自己的MAC地址，其它保持静默。  
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/9460595db97345fc98c5e44f9ae967ff.png)

#### 3.4.2 UDP广播的特点

1、处于同一子网的所有主机都必须处理数据  
2、UDP数据包会沿协议栈向上一直到UDP层  
3、运行音视频等较高速率工作的应用，会带来大负载  
4、局限于局域网内使用

#### 3.4.3 UDP广播地址

{网络ID，主机ID}  
网络ID表示由子网掩码中1覆盖的连续位  
主机ID表示由子网掩码中0覆盖的连续位

- 定向广播地址：主机ID全1  
    1、例：对于192.168.220.0/24，其定向广播地址为192.168.220.**255**(==所有主机必须无条件接收==)  
    2、通常路由器不转发该广播
    
- 受限广播地址：255.255.255.255  
    路由器从不转发该广播(否则全世界的人都接收了)
    

#### 3.4.4广播与单播的对比

单播：

一对一的  
![在这里插入图片描述](https://img-blog.csdnimg.cn/629704830c0046a0b82379bf71b4960f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_13,color_FFFFFF,t_70,g_se,x_16)

广播：

> 主机ID全1，即IP地址最后一位255，所有IP都要无条件接收  
> MAC地址全FF，所有链路层MAC都要接收

![在这里插入图片描述](https://img-blog.csdnimg.cn/795158ba73fe4a0785bbd709439d9350.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

#### 3.4.6 广播代码编程

套接口选项

```c
int setsockopt(int sockfd, int level, int optname, const void *optval, socklen_t optlen);
成功执行返回0，否则返回-1

此函数功能非常强大，除了设置广播还有很多其他功能。
```

参数说明：

> ![在这里插入图片描述](https://img-blog.csdnimg.cn/0400a4624cec4f22804259c832b393bd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16) ![在这里插入图片描述](https://img-blog.csdnimg.cn/b6748a3446d34bf18e90ffe3245838bc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/0f2ac83f0df749378c9dcf7d003bb393.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_17,color_FFFFFF,t_70,g_se,x_16)

第25行IP要写成XXX.XXX.XXX.255的形式。一旦发送出去，同一网段内所有电脑都能收到此包数据。

![在这里插入图片描述](https://img-blog.csdnimg.cn/d0c324ed091b4d4db3301bfa76843c07.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_17,color_FFFFFF,t_70,g_se,x_16)

### 3.5 UDP多播(部分)

#### 3.5.1多播概述

多播：  
数据的收发仅仅在同一分组中进行，将几个主机添加到这个分组内，==就可以同时给组内这几个节点发送==。

多播的特点：  
1、多播地址标示**一组**接口  
2、多播可以用于**广域网**使用  
3、在IPv4中，多播是可选的

![在这里插入图片描述](https://img-blog.csdnimg.cn/b7907ce11fdc49bc96cbd7293e08273a.png)

#### 3.5.2多播地址

IPv4的D类IP地址是**多播地址**  
十进制：224.0.0.1 ~ 239.255.255.254(里面任意一个都可以是多播地址)  
十六进制：E0.00.00.01 ~ EF.FF.FF.FE

多播地址向以太网MAC地址的映射(MAC地址不能直接全部设为FF，需要进行过滤)

![在这里插入图片描述](https://img-blog.csdnimg.cn/b17eecb4776e40a8930b6a459ca6f821.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

#### 3.5.3 UDP多播工作流程图

![在这里插入图片描述](https://img-blog.csdnimg.cn/55b001de360a48c8a8b0392c5456a847.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)  
备用的MAC地址就是多播地址向以太网MAC地址映射出来的。

![在这里插入图片描述](https://img-blog.csdnimg.cn/4e1509c2cdc34ab1b2d3bef704ee9877.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

#### 3.5.4加入多播组代码

**多播地址结构体**

在IPv4因特网域(AF_INET)中，多播地址结构体用如下结构体ip_mreq表示：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/cb17ea5b9e1c46e38608534de7ff46ae.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_18,color_FFFFFF,t_70,g_se,x_16)

结构体ip_mreq里面的两个IP分别表示 ：  
多播组的IP  
自己的IP

**多播套接口选项**

```c
int setsockopt(int sockfd, int level,int optname,   
                const void *optval, socklen_t optlen);
成功执行返回0，否则返回-1
```

![level	optname	说明	optval类型
IPPROTO_IP	IP_ADD_MEMBERSHIP	加入多播组	ip_mreq{}
IP_DROP_MEMBERSHIP	离开多播组	ip_mreq{}](https://img-blog.csdnimg.cn/b1e388ae9e3549e1a5dfdd20b19c0b3f.png)

**加入多播组示例**  
![在这里插入图片描述](https://img-blog.csdnimg.cn/0e73d542987d4b3fa52d7c3ce46cb854.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

MAC地址会根据自己的协议栈自己去映射。

## 第四章：TCP网络编程

### 4.1 TCP介绍、编程流程

TCP回顾

1、面向连接的流式协议;==可靠==、==出错重传==、且每收到一个数据都要给出相应的==确认==  
2、通信之前需要建立链接  
3、服务器被动连接，客户端是主动连接

TCP与UDP的差异

![在这里插入图片描述](https://img-blog.csdnimg.cn/4b093cbff57f4f87acf9d5577583e2fa.png)

**TCP C/S架构**

![在这里插入图片描述](https://img-blog.csdnimg.cn/76341c2fb2474d8ab8f4e0cb8c2ebe46.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

### 4.2 TCP编程-socket创建套接字

UDP套接字创建回顾

![在这里插入图片描述](https://img-blog.csdnimg.cn/ff69fcf2973a40d59094a3f3c4910fdd.png)

创建TCP套接字

![在这里插入图片描述](https://img-blog.csdnimg.cn/538adef3e89041ed8decb6f0eca78691.png)

做为客户端需要具备的条件  
1、知道“服务器”的ip、port  
2、主动连接“服务器”  
3、需要用到的函数

socket—创建“主动TCP套接字”  
connect—连接“服务器”  
send—发送数据到“服务器”  
recv—接受“服务器”的响应  
close—关闭连接

### 4.3 TCP客户端-connect、send、recv

**connect函数**

```c
int connect(int sockfd, const struct sockaddr *addr, socklen_t len);

功能：
主动跟服务器建立链接

参数：
sockfd：socket套接字
addr:   连接的服务器地址结构
len：	地址结构体长度

返回值：
成功：0    失败：其他
```

注意：  
1、connect建立连接之后不会产生新的套接字  
2、连接成功后才可以开始传输TCP数据  
3、头文件：#include <sys/socket.h>

**send函数**

```c
ssize_t send(int sockfd, const void* buf,size_t nbytes, int flags);

功能：
用于发送数据

参数：
sockfd： 已建立连接的套接字
buf：	   发送数据的地址
nbytes:  发送缓数据的大小(以字节为单位)
flags:	   套接字标志(常为0)

返回值：
成功发送的字节数

头文件：
#include <sys/socket.h>

注意：
不能用TCP协议发送0长度的数据包
```

发送数据如图所示

![在这里插入图片描述](https://img-blog.csdnimg.cn/41d63e38e3b1485e8b050539d812fdeb.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/86b2e2af67bd430083686c7f2a9dcb99.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_10,color_FFFFFF,t_70,g_se,x_16)

**recv函数**  
```
ssize_t recv(int sockfd, void *buf, size_t nbytes, int flags);  
功能：  
	用于接收网络数据  
参数：  
	sockfd：套接字  
	buf: 接收网络数据的缓冲区的地址  
	nbytes: 接收缓冲区的大小(以字节为单位)  
	flags: 套接字标志(常为0)  
返回值：  
	成功接收到字节数  
头文件：  
	#include <sys/socket.h>
```

接收数据如图所示：

![在这里插入图片描述](https://img-blog.csdnimg.cn/67644294cf8f4e92b4f8bcadd3394a38.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/00b4f2d87f44465a8050bf8997cfb204.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_10,color_FFFFFF,t_70,g_se,x_16)

#### 客户端代码编写案例

![在这里插入图片描述](https://img-blog.csdnimg.cn/a2407287f9344eaab8621bad6d32ac0a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_18,color_FFFFFF,t_70,g_se,x_16)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/6b25e8718c914db497ad45a0c49f9c61.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_18,color_FFFFFF,t_70,g_se,x_16)

```c
#include <stdio.h>
#include <sys/socket.h>
#include <unistd.h>
#include <arpa/inet.h>
#include <string.h>

int main(int argc, char *argv[])
{
	//创建流式套接字
	int s_fd = socket(AF_INET,SOCK_STREAM,0);
	if(s_fd < 0)
		perror("");
	//连接服务器
	struct sockaddr_in ser_addr;
	ser_addr.sin_family = AF_INET;
	ser_addr.sin_port = htons(8080);//服务器的端口
	ser_addr.sin_addr.s_addr = inet_addr("192.168.127.1");//服务器的ip
	int ret = connect(s_fd, (struct sockaddr*)&ser_addr,sizeof(ser_addr));
	if(ret<0)
		perror("");
	//收发数据
	while(1)
	{
		char buf[128]="";
		char r_buf[128]="";
		fgets(buf,sizeof(buf),stdin);
		buf[strlen(buf)-1]=0;//最后一个字符置零
		write(s_fd,buf,strlen(buf));
		int cont = read(s_fd,r_buf,sizeof(r_buf));
		if(cont == 0)
		{
			printf("server close\n");
			break;//对方关闭！！
		}
		printf("recv server = %s\n",r_buf);
	}
	//关闭套接字
	close(s_fd);
	return 0;
}
```

> ![在这里插入图片描述](https://img-blog.csdnimg.cn/1e60f0b590504582a298ad235fa0dede.png)

### 4.4 TCP服务器通信流程-bind、listen、accept、close

#### 流程图

![在这里插入图片描述](https://img-blog.csdnimg.cn/2afcde9c1f9e47d39e3dc793e4d8d623.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/f1555b6a234e4c5080450a7b1bcef5da.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/1a58f6a6f2564188bff8b560a23f60a9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

**做为TCP服务器需要具备的条件**

1、具备一个可以确知的地址  
2、让操作系统知道是一个服务器，而不是客户端  
3、等待连接的到来  
对于面向连接的TCP协议来说，连接的建立才真正意味着数据通信的开始

**bind示例**

![在这里插入图片描述](https://img-blog.csdnimg.cn/d0f448bcad594183a835409d754107ee.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_16,color_FFFFFF,t_70,g_se,x_16)

**listen 函数**

```c
int listen(int sockfd, int backlog);

功能：
将套接字由主动修改为被动
使操作系统为该套接字设置一个连接队列，用来记录所有连接到该套接字的连接

参数：
sockfd： socket监听套接字
backlog：已完成连接队列和未完成连接队列上连接数之和的最大值  一般写128

返回值：
成功：返回0
失败：其他

头文件：
#include <sys/socket.h>
```

**accept函数**

```c
int accept(int sockfd,struct sockaddr *cliaddr, socklen_t *addrlen);

功能：
从已连接队列中取出一个已经建立的连接，如果没有任何连接可用，则进入睡眠等待(阻塞)

参数：
sockfd： socket监听套接字
cliaddr: 用于存放客户端套接字地址结构
addrlen：套接字地址结构体长度的地址

返回值：
已连接套接字

头文件：
#include <sys/socket.h>
注意：
返回的是一个已连接套接字，这个套接字代表当前这个连接
```

**close 关闭套接字**

1、使用close函数即可关闭套接字  
关闭一个代表已连接套接字将导致另一端接收到一个0长度的数据包  
2、做服务器时  
1>关闭监听套接字将导致服务器无法接收新的连接，但不会影响已经建立的连接  
2>关闭accept返回的已连接套接字将导致它所代表的连接被关闭，但不会影响服务器的监听  
3、做客户端时  
关闭连接就是关闭连接，不意味着其他

#### TCP服务器例子代码编程

```c
#include <stdio.h>
#include <sys/socket.h>
#include <unistd.h>
#include <arpa/inet.h>
int main(int argc, char *argv[])
{
	//创建套接字
	int lfd = socket(AF_INET,SOCK_STREAM,0);
	if(lfd < 0)
		perror("");
	//绑定
	struct sockaddr_in myaddr;
	myaddr.sin_family = AF_INET;
	myaddr.sin_port = htons(9999);
	myaddr.sin_addr.s_addr = 0;//本机所有的ip都绑定
	bind(lfd,(struct sockaddr*)&myaddr,sizeof(myaddr));//绑定
	//监听
	listen(lfd,128);
	//提取
	struct sockaddr_in cliaddr;
	socklen_t len = sizeof(cliaddr);		// 客户端地址信息
	int cfd = accept(lfd,(struct sockaddr *)&cliaddr,&len);//从已连接队列提取新的连接，并创建一个新的已连接套接字
	if(cfd < 0 )
		perror("");
	char ip[16]="";
	printf("client ip=%s port=%d\n",inet_ntop(AF_INET,&cliaddr.sin_addr.s_addr,ip,16),ntohs(cliaddr.sin_port));
	//读写
	while(1)
	{
		char buf[256]="";
		//回射客户器
		int ret = read(cfd,buf,sizeof(buf));//阻塞
		if(ret == 0)
		{
			printf("client close\n");
			break;
		}
		printf("recv = [%s]\n",buf);
		write(cfd,buf,ret);
	
	}
	//关闭
	close(lfd);
	close(cfd);
	return 0;
}
```

> 注意：这个服务器是有问题的，强制退出再运行，客户端连接不上了！因为绑定的端口号没有被释放，后面章节会讲这个。

### 4.5 TCP三次握手、四次挥手

#### 三次握手

三次握手开始于客户端调用connect函数

![在这里插入图片描述](https://img-blog.csdnimg.cn/d2835f899d184dc2896506904fc1ee79.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_10,color_FFFFFF,t_70,g_se,x_16)

**SYN、ACK、序列号、确认号参照TCP报头协议。**  
![在这里插入图片描述](https://img-blog.csdnimg.cn/4e098622d78c4bb6a775d2cac64b9390.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

SYN泛洪攻击：客户端发送SYN后不回复ACK，故意消耗服务端未连接队列，伪装成千上万个客户端这么做就会造成服务端资源耗尽，不能正常连接。  
序列号其实不是从0开始的，防止被攻击，只是抓包工具处理了，给我们展示的从0开始。

> 抓包工具验证三次握手(客户端给服务端发送数据：world)  
> 里push字段含义：发送world时缓冲区没有满，此时执行发送就叫做push![在这里插入图片描述](https://img-blog.csdnimg.cn/ebe6affa77d2481d8def5e6176356272.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

#### 四次挥手

![在这里插入图片描述](https://img-blog.csdnimg.cn/b70809d11d804d698c9cf58a725796dc.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/dc779912afde4d819b9bf509cc7ee432.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

ctrl+c也会关闭套接字描述符，相当于主动调用close关闭连接(此时底层会发送一个0长度数据报文)，这就解释了项目上为什么我ctrl+c后AWS后台立马显示mqtt断连。

服务端ACK和FIN为什么不一起发？  
1、ACK是因为收到客户端的FIN立即返回；  
2、FIN是服务端判断收到0长度数据报文调用close后发的。  
如果服务端调用close的时机很快，那么ACK和FIN是有可能放到一个包里发送的，也就是3次挥手。

关于MSL：前面做实验ctrl+c将服务器关掉再启动的时候，服务器启动不了。因为发送完第四次挥手ACK协议栈底层并没有关闭，端口还在占用着。为什么这么做？因为第四次挥手发送ACK如果丢了，对方会给你重发FIN请求，如果你此时关闭就收不到了，所以必须等待ACK确保发送给了对方，也就是等待2MSL时间，==谁主动关闭谁就主动等待2MSL==。

> 抓包工具验证：发现只有3次，说明第三次和第四次挥手合成一次了。  
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/c5a50225e01148f4bee85d657be88d1b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

#### 滑动窗口

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/e65bb05127c546e187860db320255cbe.png)

#### TCP状态转换图(通过命令查看状态)

![在这里插入图片描述](https://img-blog.csdnimg.cn/1ab58a84af5a43fabd711959af236105.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

> 查看状态命令：netstat  
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/d14f2d8a66c34c52870291bd87c99f34.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)  
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/b6d9dc225fc840529e433516443e6148.png)  
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/410c848bbb774b87968e631a5177dd65.png)

#### 半关闭

TCP状态转换图里主动方的FIN_WAIT_2状态就是半关闭状态，此时主动方的应用层只能收数据，不能发数据。对方应用层还有什么数据没发送过来，可以继续发，底层协议栈自动实现的。

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/2788b5d0ca8d45ae92f8f48efb4e45f0.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/a32e0e2bb11340d0ae2d2e44d6197def.png)

文件描述符计数：文件描述符被多个进程打开，打开一个就要自增1，关闭一个就要自减1，当减到0的时候才算最终关闭。不涉及文件描述符计数就是调用立马关闭。

#### 端口复用

断开连接的时候，谁主动关闭谁就要等待2MSL，所以再次连接就会连接不上，因为端口被占用了。这里可以设置端口复用，被复用后之前那个同样的端口就失效了。

![在这里插入图片描述](https://img-blog.csdnimg.cn/99ba4e6085d04713b805f03a15be355d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/056ba59202ce4a41b8f7e8a2fa8ff843.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

#### 心跳包(少用)

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/a324d655913a42ab92d96efff76bc8ee.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/5cfa9f8a055248958df6b1d85ec4664c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

最小粒度：省流量吧，MQTT心跳就两个字节。

一般很少用这个函数，都是应用层自己写。

### 4.6 TCP并发服务器

> accept和read函数默认是带阻塞的，必须要多进程或多线程实现并发通信。  
> 比如阻塞在read的时候，来了一个新的客户端发起连接，==服务端没办法accept提取下个连接==。

#### 4.6.1多进程实现并发服务器

![在这里插入图片描述](https://img-blog.csdnimg.cn/35e22b55e4f44207bf8b00c504c13e66.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

> lfd：listen监听描述符  
> cfd：connect连接描述符

03_process_tcp_server.c

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <signal.h>
#include <sys/types.h>
#include <sys/wait.h>

void cath_child(int num)
{
	pid_t pid;
	while(1)
	{               //-1是任意子进程 NULL不接收子进程返回参数  WNOHANG子进程不退出不阻塞，不会等
		pid = waitpid(-1,NULL,WNOHANG);//循环回收子进程资源，不然浪费了
		if(pid <= 0)//都回收完了
		{
			break;
		}
		else if(pid > 0)//还有子进程没回收，继续回收
		{
			printf("child process %d\n",pid);
			continue;
		}
	}

}

int main(int argc, char *argv[])
{
	//将SIGCHLD 加入到阻塞集中     子进程退出需要回收资源
	sigset_t set;
	sigemptyset(&set);
	sigaddset(&set,SIGCHLD);
	sigprocmask(SIG_BLOCK,&set,NULL);//信号上锁
	
	// 创建套接字
	int lfd = socket(AF_INET,SOCK_STREAM,0);
	if(lfd <0)
		perror("");
	// 绑定
	struct sockaddr_in myaddr;
	myaddr.sin_family = AF_INET;
	myaddr.sin_port = htons(6666);
	myaddr.sin_addr.s_addr = 0;
	bind(lfd,(struct sockaddr *)&myaddr,sizeof(myaddr));
	// 监听
	listen(lfd,128);
	// 提取
	struct sockaddr_in cliaddr;
	socklen_t len = sizeof(cliaddr);
	while(1)
	{
		int cfd = accept(lfd,(struct sockaddr*)&cliaddr,&len);// 没有连接就阻塞在这里
		if(cfd < 0)
			perror("");
		char ip[16]="";
		printf("client ip=%s port=%d\n",
				inet_ntop(AF_INET,&cliaddr.sin_addr.s_addr,ip,16),ntohs(cliaddr.sin_port));
		
		pid_t pid;
		pid = fork();
		if(pid ==0 )		//子进程
		{
			close(lfd);
			while(1)
			{
				sleep(1);
				char buf[256]="";
				int n = read(cfd,buf,sizeof(buf));
				if(0==n)
				{
					printf("client close\n");
					break;
				}
				printf("[*********%s*********]\n",buf);
				// 回射过去
				write(cfd,buf,n);
			}
			//close(cfd);
			//exit(0);
			break;
		}
		else if(pid > 0)	//父进程
		{
			close(cfd);
			struct sigaction act;
			act.sa_handler = cath_child;//信号回调函数，当子进程退出调用此回调，回收子进程资源
			act.sa_flags = 0;//一般置零
			sigemptyset(&act.sa_mask);//请空mask(捕捉到信号后临时的一个屏蔽集)
			sigaction(SIGCHLD,&act,NULL);

			sigprocmask(SIG_UNBLOCK,&set,NULL);// 信号剔除
		}
	 }
	
	// 收尾

	return 0;
}
```

> 注意：上述代码中，accept函数默认阻塞等待新的连接，当客户端退出时触发信号，accept被信号中断和软件层次中断，后面接着运行了，进入到while循环会一直打印**********(while循环里cfd无效，所以read不会阻塞)，所以需要对accept返回值进行判断，如果accept被信号中断则继续accept。代码如下(使用视频里面的封装函数，对各个函数返回值进行判断处理)：

**封装accept等函数，主要是对返回值进行判断处理**

wrap.h

```c
#ifndef __WRAP_H_
#define __WRAP_H_
#include <stdlib.h>
#include <stdio.h>
#include <unistd.h>
#include <errno.h>
#include <string.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <strings.h>

void perr_exit(const char *s);
int Accept(int fd, struct sockaddr *sa, socklen_t *salenptr);
int Bind(int fd, const struct sockaddr *sa, socklen_t salen);
int Connect(int fd, const struct sockaddr *sa, socklen_t salen);
int Listen(int fd, int backlog);
int Socket(int family, int type, int protocol);
ssize_t Read(int fd, void *ptr, size_t nbytes);
ssize_t Write(int fd, const void *ptr, size_t nbytes);
int Close(int fd);
ssize_t Readn(int fd, void *vptr, size_t n);
ssize_t Writen(int fd, const void *vptr, size_t n);
ssize_t my_read(int fd, char *ptr);
ssize_t Readline(int fd, void *vptr, size_t maxlen);
int tcp4bind(short port,const char *IP);
#endif
```

wrap.c

```c
#include <stdlib.h>
#include <stdio.h>
#include <unistd.h>
#include <errno.h>
#include <string.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <strings.h>

void perr_exit(const char *s)
{
    perror(s);
    exit(-1);
}

int Accept(int fd, struct sockaddr *sa, socklen_t *salenptr)
{
    int n;

again:
    if ((n = accept(fd, sa, salenptr)) < 0)
    {
        if ((errno == ECONNABORTED) || (errno == EINTR)) // 如果是被信号中断和软件层次中断,不能退出!!
            goto again;                                  // 继续返回到accept阻塞 等待新的客户端接入
        else
            perr_exit("accept error");
    }
    return n;
}

int Bind(int fd, const struct sockaddr *sa, socklen_t salen)
{
    int n;

    if ((n = bind(fd, sa, salen)) < 0)
        perr_exit("bind error");

    return n;
}

int Connect(int fd, const struct sockaddr *sa, socklen_t salen)
{
    int n;

    if ((n = connect(fd, sa, salen)) < 0)
        perr_exit("connect error");

    return n;
}

int Listen(int fd, int backlog)
{
    int n;

    if ((n = listen(fd, backlog)) < 0)
        perr_exit("listen error");

    return n;
}

int Socket(int family, int type, int protocol)
{
    int n;

    if ((n = socket(family, type, protocol)) < 0)
        perr_exit("socket error");

    return n;
}

ssize_t Read(int fd, void *ptr, size_t nbytes)
{
    ssize_t n;

again:
    if ((n = read(fd, ptr, nbytes)) == -1)
    {                       // 默认是阻塞的
        if (errno == EINTR) // 如果是被信号中断,不应该退出
            goto again;     // 继续回去读
        else
            return -1;
    }
    return n;
}

ssize_t Write(int fd, const void *ptr, size_t nbytes)
{
    ssize_t n;

again:
    if ((n = write(fd, ptr, nbytes)) == -1)
    {
        if (errno == EINTR)
            goto again;
        else
            return -1;
    }
    return n;
}

int Close(int fd)
{
    int n;
    if ((n = close(fd)) == -1)
        perr_exit("close error");

    return n;
}

/*参三: 读取固定的字节数数据   如果一次读取的不够，则继续读取累加*/
ssize_t Readn(int fd, void *vptr, size_t n)
{
    size_t nleft;  // usigned int 剩余未读取的字节数
    ssize_t nread; // int 实际读到的字节数
    char *ptr;

    ptr = vptr;
    nleft = n;

    while (nleft > 0)
    {
        if ((nread = read(fd, ptr, nleft)) < 0)
        {
            if (errno == EINTR)
                nread = 0;
            else
                return -1;
        }
        else if (nread == 0)
            break;

        nleft -= nread;
        ptr += nread;
    }
    return n - nleft;
}
/*:写固定的字节数数据*/
ssize_t Writen(int fd, const void *vptr, size_t n)
{
    size_t nleft;
    ssize_t nwritten;
    const char *ptr;

    ptr = vptr;
    nleft = n;
    while (nleft > 0)
    {
        if ((nwritten = write(fd, ptr, nleft)) <= 0)
        {
            if (nwritten < 0 && errno == EINTR)
                nwritten = 0;
            else
                return -1;
        }

        nleft -= nwritten;
        ptr += nwritten;
    }
    return n;
}

static ssize_t my_read(int fd, char *ptr)
{
    static int read_cnt;
    static char *read_ptr;
    static char read_buf[100];

    if (read_cnt <= 0)
    {
again:
        if ((read_cnt = read(fd, read_buf, sizeof(read_buf))) < 0)
        {
            if (errno == EINTR)
                goto again;
            return -1;
        }
        else if (read_cnt == 0)
            return 0;
        read_ptr = read_buf;
    }
    read_cnt--;
    *ptr = *read_ptr++;

    return 1;
}

// 读一行，遇到\0就结束
ssize_t Readline(int fd, void *vptr, size_t maxlen)
{
    ssize_t n, rc;
    char c, *ptr;

    ptr = vptr;
    for (n = 1; n < maxlen; n++)
    {
        if ((rc = my_read(fd, &c)) == 1)
        {
            *ptr++ = c;
            if (c == '\n')
                break;
        }
        else if (rc == 0)
        {
            *ptr = 0;
            return n - 1;
        }
        else
            return -1;
    }
    *ptr = 0;

    return n;
}

// 传端口和IP参数即可，很方便
int tcp4bind(short port, const char *IP)
{
    struct sockaddr_in serv_addr;
    int lfd = Socket(AF_INET, SOCK_STREAM, 0);
    bzero(&serv_addr, sizeof(serv_addr));
    if (IP == NULL)
    {
        // 如果这样使用 0.0.0.0,任意ip将可以连接
        serv_addr.sin_addr.s_addr = INADDR_ANY;
    }
    else
    {
        if (inet_pton(AF_INET, IP, &serv_addr.sin_addr.s_addr) <= 0)
        {
            perror(IP); // 转换失败
            exit(1);
        }
    }
    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(port);
    // int opt = 1;
    // setsockopt(lfd, SOL_SOCKET, SO_REUSEADDR, &opt, sizeof(opt));

    Bind(lfd, (struct sockaddr *)&serv_addr, sizeof(serv_addr));
    return lfd;
}
```

#### 4.6.2多线程实现并发服务器

```c
#include <stdio.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <pthread.h>
#include <string.h>
#include <stdlib.h>
typedef struct _info
{
	int cfd;
	struct sockaddr_in client;

}INFO;

void* resq_client(void *arg)
{
	INFO *info = (INFO*)arg;
	char ip[16]="";
	printf("client ip=%s port=%d\n",
			inet_ntop(AF_INET,&(info->client.sin_addr.s_addr),ip,16),ntohs(info->client.sin_port));
	while(1)
	{
		char buf[1024]="";
		int n = read(info->cfd,buf,sizeof(buf));
		if(n == 0)
		{
			printf("cleint close\n");
			break;
		}
		printf("%s\n",buf);
		write(info->cfd,buf,n);
	}
	close(info->cfd);
	free(info);
}

int main(int argc, char *argv[])
{
	int lfd = socket(AF_INET,SOCK_STREAM,0);
	if(lfd <0)
		perror("");

	int opt=1;
	setsockopt(lfd,SOL_SOCKET,SO_REUSEADDR,&opt,sizeof(opt));

	struct sockaddr_in myaddr;
	myaddr.sin_family = AF_INET	;
	myaddr.sin_port= htons(8888);
	myaddr.sin_addr.s_addr = 0;
	if( bind(lfd,(struct sockaddr*)&myaddr,sizeof(myaddr))< 0)
		perror("bind:");

	listen(lfd,128);
	while(1)
	{
		struct sockaddr_in client;
		socklen_t len = sizeof(client);
		
		int cfd = accept(lfd,(struct sockaddr*)&client,&len);
		INFO *info = malloc(sizeof(INFO));//接入一个客户端线程就开辟一段内存，防止线程间相互干扰
		info->cfd = cfd;
		info->client = client;
		
		pthread_t pthid;                       //传参数 结构体的地址
		pthread_create(&pthid,NULL,resq_client,info);
		pthread_detach(pthid);//线程脱离
	}

	close(lfd);
	return 0;
}
```

### IO复用，多路IO转接技术(select、epoll)

#### 多路IO转接技术引入

![在这里插入图片描述](https://img-blog.csdnimg.cn/22db96d0e6924082b3a6767535d8bd03.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)  
就像是老板招了很多人，这些人大部分时间比较闲，躺着睡觉。这样非常浪费内存。

---

![在这里插入图片描述](https://img-blog.csdnimg.cn/c4d79c15e3a94cfcad3e6899b41970c8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)  
非阻塞忙轮询：不阻塞，一个进程疯狂循环轮询中间的各个fd套接字，查看是否有连接或数据到达，有就去处理。这样非常浪费CPU。

---

![在这里插入图片描述](https://img-blog.csdnimg.cn/6f7439a189e84c6da205b8664c4d6a29.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)  
招一个秘书(内核)，站在高处看哪个管子要滴水，指挥下面的人去接。  
如果只有下面一个人，要来回跑查看哪个水管要滴水，等同于上面那个非阻塞忙轮询，浪费CPU资源。

内核不断监听各fd里的读缓冲区(当然也能监听写缓冲区，所以叫监听多路IO)，如下图：

![在这里插入图片描述](https://img-blog.csdnimg.cn/edf1db92cd6546a787731c4f48e2bb02.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/19691d1014d8492b956a6037b4891ef7.png)

#### select的使用

功能：监听文件描述符的属性变化(读写缓冲区的变化)

```c
#include <sys/select.h>
/* According to earlier standards */
#include <sys/time.h>
#include <sys/types.h>
#include <unistd.h>

int select(int nfds, fd_set *readfds, fd_set *writefds, fd_set *exceptfds, struct timeval *timeout);
    函数功能: 监听多个文件描述符的属性变化(读写缓冲区的变化)
    参数:
            nfds: 	  监听的最大文件描述符+1  (遍历的文件描述符个数)
            readfds:  需要监听[读]属性变化的文件描述符集合
            writefds: 需要监听[写]属性变化的文件描述符集合
            exceptfds:需要监听[异常]属性变化的文件描述符集合
            timeout:  超时时间
                        NULL  	永久监听
                        >0      超时时间
                        =0		不等待，立即返回
    返回值: 监听到变化的文件描述符个数，同时fd_set里已经更新成对应变化的文件描述符(不然你只知道变化的个数，不知道对应哪一个变的)


void FD_SET(int fd, fd_set *set);	//将文件描述符fd加入到set集合中
void FD_CLR(int fd, fd_set *set);	//将文件描述符fd从set集合中清除
int  FD_ISSET(int fd, fd_set *set);	//判断文件描述符fd是否在set集合中
void FD_ZERO(fd_set *set);			//清空set文件描述符集合

struct timeval {
          long    tv_sec;         /* seconds */
          long    tv_usec;        /* microseconds */
 };
```

**05 select_tcp_server.c**

文件描述符0、1、2(前三个是标准描述符)、3…逐步递增的

select监听的流程  
创建套接字  
绑定  
监听  
while(1){  
select()//监听lfd

- 如果是lfd变化,提取新的连接,并且将新的套接字加入监听的集合
- 如果是cfd变化,直接读取,发送回去即可

}

```c
#include <stdio.h>
#include "wrap.h"	// 老师前面封装的函数
#include <sys/time.h>
#include <sys/select.h>
#include <sys/types.h>
int main(int argc, char *argv[])
{
    // 创建套接字
    // 绑定
    int lfd = tcp4bind(9999, NULL);
    // 监听
    listen(lfd, 128);
    int max_fd = lfd;

    fd_set r_set;   // 需要监听的文件描述符集合
    fd_set old_set; // 备份
    FD_ZERO(&old_set);
    FD_ZERO(&r_set);
    FD_SET(lfd, &old_set);
    int nready = 0;
    while (1)
    {
        r_set = old_set;//每次都要重新赋值一下  select会改变参数值
        nready = select(max_fd + 1, &r_set, NULL, NULL, NULL); // 监听变化的个数
        if (nready < 0)
        {
            perror("");
            break;
        }
        else if (nready >= 0)
        {
            // 判断lfd是否变化了(是否在set集合内)
            if (FD_ISSET(lfd, &r_set))
            {
                // 提取新的连接
                struct sockaddr_in cliaddr;
                socklen_t len = sizeof(cliaddr);
                char ip[16] = "";
                int cfd = Accept(lfd, (struct sockaddr *)&cliaddr, &len);
                printf("client ip=%s port=%d\n",
                       inet_ntop(AF_INET, &cliaddr.sin_addr.s_addr, ip, 16),
                       ntohs(cliaddr.sin_port));
                // 将cfd加入到old_set
                FD_SET(cfd, &old_set);
                // 更新最大值
                if (max_fd < cfd)
                    max_fd = cfd;
                // 如果只有lfd变化,执行下一次监听  下面循环不进行
                if (--nready == 0)
                    continue; 
            }
            
            // 判断cfd是否变化了(是否在set集合内)
            for (int i = lfd + 1; i <= max_fd; i++) // 遍历文件描述符 看哪个变化的  比较麻烦
            {
                // cfd变化了
                if (FD_ISSET(i, &r_set))
                {
                    char buf[1024] = "";
                    int n = Read(i, buf, sizeof(buf));
                    if (n == 0)
                    {
                        printf("client close\n");
                        close(i);
                        // 将i从old_set删除，不监听了
                        FD_CLR(i, &old_set);
                    }
                    else if (n < 0)
                    {
                        perror("");
                    }
                    else
                    {
                        printf("%s\n", buf);
                        Write(i, buf, n);
                    }
                }
            }
        }
    }
    return 0;
}
```

#### select的优缺点

- 优点:  
    跨平台 windows、linux下都可以使用(主要是windows平台用，linux不用)  
    并发量高  
    效率高  
    消耗资源少  
    消耗cpu也少
    
- 缺点:
    
- - 有最大文件描述符的个数限制 FD_SETSIZE = 1024(这个宏限定最大并发1024个，修改的话需要重新编译Linux内核)
- - 每次重新监听都需要将监听的文件描述符集合从用户态拷贝至内核态(让内核态去监听)消耗资源。  
        监听到文件描述符变化之后,需要用户自己遍历集合才知道具体哪个文件描述符变化了
- - 大量并发,少数活跃 select的效率低 无解的缺陷

问题:

- 假设监听3-1023个文件描述符,4-1001都关闭了，==还是挨个遍历==【**优化方法**：自定义一个数组，把关闭的文件描述符剔除】。
- 假设大量并发少数活跃， 3-1024都在监听，并且都没有退出,但是经常只有 5,6发来消息，目前这个无解，==还是要挨个遍历==，效率低。

#### select的优化

select未优化之前的代码:

```c
#include <stdio.h>
#include <sys/select.h>
#include <sys/types.h>
#include <unistd.h>
#include "wrap.h"
#include <sys/time.h>
#define PORT 8888
int main(int argc, char *argv[])
{
    // 创建套接字,绑定
    int lfd = tcp4bind(PORT, NULL);
    // 监听
    Listen(lfd, 128);
    int maxfd = lfd; // 最大的文件描述符
    fd_set oldset, rset;
    FD_ZERO(&oldset);
    FD_ZERO(&rset);
    // 将lfd添加到oldset集合中
    FD_SET(lfd, &oldset);
    while (1)
    {
        rset = oldset; // 将oldset赋值给需要监听的集合rset

        int n = select(maxfd + 1, &rset, NULL, NULL, NULL);
        if (n < 0)
        {
            perror("");
            break;
        }
        else if (n == 0)
        {
            continue; // 如果没有变化,重新监听
        }
        else // 监听到了文件描述符的变化
        {
            // lfd变化 代表有新的连接到来
            if (FD_ISSET(lfd, &rset))
            {
                struct sockaddr_in cliaddr;
                socklen_t len = sizeof(cliaddr);
                char ip[16] = "";
                // 提取新的连接
                int cfd = Accept(lfd, (struct sockaddr *)&cliaddr, &len);
                printf("new client ip=%s port=%d\n", inet_ntop(AF_INET, &cliaddr.sin_addr.s_addr, ip, 16),
                       ntohs(cliaddr.sin_port));
                // 将cfd添加至oldset集合中,以下次监听
                FD_SET(cfd, &oldset);
                // 更新maxfd
                if (cfd > maxfd)
                    maxfd = cfd;
                // 如果只有lfd变化,continue
                if (--n == 0)
                    continue;
            }

            // cfd  遍历lfd之后的文件描述符是否在rset集合中,如果在则cfd变化
            for (int i = lfd + 1; i <= maxfd; i++)
            {
                // 如果i文件描述符在rset集合中
                if (FD_ISSET(i, &rset))
                {
                    char buf[1500] = "";
                    int ret = Read(i, buf, sizeof(buf));
                    if (ret < 0) // 出错,将cfd关闭,从oldset中删除cfd
                    {
                        perror("");
                        close(i);
                        FD_CLR(i, &oldset);
                        continue;
                    }
                    else if (ret == 0)
                    {
                        printf("client close\n");
                        close(i);
                        FD_CLR(i, &oldset);
                    }
                    else
                    {
                        printf("%s\n", buf);
                        Write(i, buf, ret);
                    }
                }
            }
        }
    }

    return 0;
}
```

**优化select**  
两种方法:  
数组法优化:

```c
// 进阶版select，通过数组防止遍历1024个描述符
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <arpa/inet.h>
#include <ctype.h>

#include "wrap.h"

#define SERV_PORT 8888

int main(int argc, char *argv[])
{
    int i, j, n, maxi;

    int nready, client[FD_SETSIZE]; /* 自定义数组client, 防止遍历1024个文件描述符  FD_SETSIZE默认为1024 */
    int maxfd, listenfd, connfd, sockfd;
    char buf[BUFSIZ], str[INET_ADDRSTRLEN]; /* #define INET_ADDRSTRLEN 16 */
    struct sockaddr_in clie_addr, serv_addr;
    socklen_t clie_addr_len;
    fd_set rset, allset; /* rset 读事件文件描述符集合 allset用来暂存 */

    listenfd = Socket(AF_INET, SOCK_STREAM, 0);
    // 端口复用
    int opt = 1;
    setsockopt(listenfd, SOL_SOCKET, SO_REUSEADDR, &opt, sizeof(opt));

    bzero(&serv_addr, sizeof(serv_addr));
    serv_addr.sin_family = AF_INET;
    serv_addr.sin_addr.s_addr = htonl(INADDR_ANY);
    serv_addr.sin_port = htons(SERV_PORT);

    Bind(listenfd, (struct sockaddr *)&serv_addr, sizeof(serv_addr));
    Listen(listenfd, 128);

    maxfd = listenfd; /* 起初 listenfd 即为最大文件描述符 */

    maxi = -1; /* 将来用作client[]的下标, 初始值指向0个元素之前下标位置 */
    for (i = 0; i < FD_SETSIZE; i++)
        client[i] = -1; /* 用-1初始化client[] */

    FD_ZERO(&allset);
    FD_SET(listenfd, &allset); /* 构造select监控文件描述符集 */

    while (1)
    {
        rset = allset; /* 每次循环时都从新设置select监控信号集 */

        nready = select(maxfd + 1, &rset, NULL, NULL, NULL); // 2  1--lfd  1--connfd
        if (nready < 0)
            perr_exit("select error");

        if (FD_ISSET(listenfd, &rset))
        { /* 说明有新的客户端链接请求 */

            clie_addr_len = sizeof(clie_addr);
            connfd = Accept(listenfd, (struct sockaddr *)&clie_addr, &clie_addr_len); /* Accept 不会阻塞 */
            printf("received from %s at PORT %d\n",
                   inet_ntop(AF_INET, &clie_addr.sin_addr, str, sizeof(str)),
                   ntohs(clie_addr.sin_port));

            for (i = 0; i < FD_SETSIZE; i++)
                if (client[i] < 0)
                {                       /* 找到client[]中没有使用的位置 */
                    client[i] = connfd; /* 保存accept返回的文件描述符到client[]里 */
                    break;
                }

            if (i == FD_SETSIZE)
            { /* 达到select能监控的文件个数上限 1024 */
                fputs("too many clients\n", stderr);
                exit(1);
            }

            FD_SET(connfd, &allset); /* 向监控文件描述符集合allset添加新的文件描述符connfd */

            if (connfd > maxfd)
                maxfd = connfd; /* select第一个参数需要 */

            if (i > maxi)
                maxi = i; /* 保证maxi存的总是client[]最后一个元素下标 */

            if (--nready == 0)
                continue;
        }

        for (i = 0; i <= maxi; i++)			// 遍历还是要遍历的，只不过下面这一行将未发生变化的continue过滤了
        { 
            if ((sockfd = client[i]) < 0)
                continue; // 数组内的文件描述符如果被释放有可能变成-1
            if (FD_ISSET(sockfd, &rset))
            {
                if ((n = Read(sockfd, buf, sizeof(buf))) == 0)
                { /* 当client关闭链接时,服务器端也关闭对应链接 */
                    Close(sockfd);
                    FD_CLR(sockfd, &allset); /* 解除select对此文件描述符的监控 */
                    client[i] = -1;
                }
                else if (n > 0)
                {
                    for (j = 0; j < n; j++)
                        buf[j] = toupper(buf[j]);
                    Write(sockfd, buf, n);
                    Write(STDOUT_FILENO, buf, n);
                }
                if (--nready == 0)
                    break; /* 跳出for, 但还在while中 */
            }
        }
    }

    Close(listenfd);

    return 0;
}
```

第二种优化的方法:  
使用文件描述符的重定向  
将关闭的文件描述符指向maxfd，再将maxfd关闭。

下图第7个关了，数组置0，将7指向数组里最大文件描述符maxfd并关闭。然后更新maxfd。

![在这里插入图片描述](https://img-blog.csdnimg.cn/bb89b9f949374f9fb15408f42c4bc4c6.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

#### epoll原理(调用红黑树接口，返回变化的文件描述符，无需遍历)

![在这里插入图片描述](https://img-blog.csdnimg.cn/19b7708b8f474aa7939a53a53b230017.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/8390ccf6b2fb4664aa826c463d16007b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

> 阿里的服务器用的就是epoll，epoll的并发量和你的电脑的性能有关系，一般个人电脑并发量一万左右。再高的话需要服务器集群、集联。

#### epoll相关API

```c
1 创建红黑树,用于监听文件描述符
#include <sys/epoll.h>
int epoll_create(int size);
功能: 创建一棵红黑树
参数: 
    size : 写大于0即可(内核自动扩展节点个数)
返回值: 树的句柄 


 
2 节点上树、节点下树、修改树节点
#include <sys/epoll.h>
int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event);
功能:  节点上树 节点下树 修改树节点
    参数:
    epfd : 树的根节点(句柄)
    op: 
            EPOLL_CTL_ADD: 上树
            EPOLL_CTL_MOD: 修改(比如将读事件修改成写事件)
            EPOLL_CTL_DEL: 下树
    fd: 上树,下树,修改的文件描述符 
    event :
	    struct epoll_event {
	        uint32_t     events;      /* Epoll events */
	        epoll_data_t data;        /* User data variable */
	    };
	        events: 
                   EPOLLIN  	读事件
                   EPOLLOUT 	写事件
	
    	    typedef union epoll_data {
	               void        *ptr; // 反应堆，下一节讲
	               int          fd;  // 需要监听的文件描述符
	               uint32_t     u32;
	               uint64_t     u64;
            } epoll_data_t;

返回值:
        成功返回0 失败返回-1



3 监听
#include <sys/epoll.h>
int epoll_wait(int epfd, struct epoll_event *events,int maxevents, int timeout);
功能: 监听树上的节点属性变化
参数:
    epfd: 		树的根节点
    events: 	struct epoll_event数组首元素的地址
    maxevents: 	数组元素的个数
    timeout: 	-1 永久监听  >=0 限时等待
返回值: 变化文件描述符的个数
```

#### epoll代码实现

01epoll_tcp_server.c

就一个进程搞定了。

```c
#include "wrap.h"
#include <sys/epoll.h>

int main(int argc, char const *argv[])
{
	// 创建套接字
	// 绑定
	int lfd = tcp4bind(8888,NULL);
	// 监听
	listen(lfd,128);
	// 创建树
	int epfd = epoll_create(1);
	// 将lfd上树
	struct epoll_event ev,evs[1024];
	ev.events  = EPOLLIN;//读事件
	ev.data.fd = lfd;
	epoll_ctl(epfd,EPOLL_CTL_ADD,lfd,&ev);
	// 循环监听树
	while(1)
	{
		int n = epoll_wait(epfd,evs,1024,-1);
		if(n < 0)
		{
			perr_exit("epoll");// 包裹函数封装，错误退出
		}
		else if(n >= 0)
		{
			for(int i=0;i<n;i++)// 遍历evs数组
			{
				int fd = evs[i].data.fd;
				// 如果是lfd变化,并且是读事件变化  用位与判断，不用管其他位数据
				if(fd == lfd &&evs[i].events&EPOLLIN)
				{
					struct sockaddr_in cliaddr;
					socklen_t len = sizeof(cliaddr);
					char ip[16]="";
					int cfd = Accept(lfd,(struct sockaddr*)&cliaddr,&len);
					printf("client ip=%s port=%d\n",
						    inet_ntop(AF_INET,&cliaddr.sin_addr.s_addr,ip,16),
						    ntohs(cliaddr.sin_port));
					// 将cfd上树
					ev.data.fd = cfd;
					ev.events  = EPOLLIN;
					epoll_ctl(epfd,EPOLL_CTL_ADD,cfd,&ev);
				}
				// 是cfd变化 并且是读事件
				else if(evs[i].events&EPOLLIN)
				{
					char buf[1500]="";
					int count = Read(fd,buf,sizeof(buf));
					if(count <= 0)
					{
						printf("error or client close\n");
						close(fd);
						epoll_ctl(epfd,EPOLL_CTL_DEL,fd,&evs[i]);//下树
					}
					else
					{
						printf("%s\n",buf);
						Write(fd,buf,count);
					}
				}
			}
		}
	}
	return 0;
}
```

#### epoll的工作方式(水平触发、边沿触发)

水平触发 EPOLLLT  
边沿触发 EPOLLET

![在这里插入图片描述](https://img-blog.csdnimg.cn/b9fe5cd4aca544c2a3bebd4b313f8b12.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

读缓冲区水平触发：比如接收到10个字节数据，但是你代码里接收缓存只有5个，一次只读5个字节数据(单次没读干净，可以循环读干净)，那么epoll_wait就会被触发两次，然后你代码里正好两次读完。

注意: 因为监听写缓冲区时,如果时监听水平触发事件,只要可写(写缓冲区没满)就会被发出,那么这样一直触发(内核态和用户态来回切换浪费效率),所以我们**最好设置为边沿触发**(数据发送一次才触发一次)。

设置边沿触发时，需要一次读干净，所以==循环读==。

因为cfd是阻塞的，read读到最后缓冲区没数据会阻塞卡在那，所以需要设置cfd为==非阻塞==。同时当read的返回值为-1，并且errno的值被设置为EAGAIN时，代表缓冲区被读取干净，要跳出循环。

```c
#include "wrap.h"
#include <sys/epoll.h>
#include <fcntl.h>

int main(int argc, char *argv[])
{
    // 创建套接字
    // 绑定
    int lfd = tcp4bind(7777, NULL);
    // 监听
    listen(lfd, 128);
    // 创建树
    int epfd = epoll_create(1);
    // 将lfd上树
    struct epoll_event ev, evs[1024];
    ev.events = EPOLLIN; // 读事件
    ev.data.fd = lfd;
    epoll_ctl(epfd, EPOLL_CTL_ADD, lfd, &ev);
    // 循环监听树
    while (1)
    {
        int n = epoll_wait(epfd, evs, 1024, -1);
        printf("epoll_wait\n");
        if (n < 0)
        {
            perr_exit("epoll");
        }
        else if (n >= 0)
        {
            for (int i = 0; i < n; i++)
            {
                int fd = evs[i].data.fd;
                // 如果是lfd变化,并且是读事件变化
                if (fd == lfd && evs[i].events & EPOLLIN)
                {
                    struct sockaddr_in cliaddr;
                    socklen_t len = sizeof(cliaddr);
                    char ip[16] = "";
                    int cfd = Accept(lfd, (struct sockaddr *)&cliaddr, &len);
                    printf("client ip=%s port=%d\n",
                           inet_ntop(AF_INET, &cliaddr.sin_addr.s_addr, ip, 16),
                           ntohs(cliaddr.sin_port));
                   
                    // cfd设置为非阻塞
                    int flag = fcntl(cfd, F_GETFL);
                    flag |= O_NONBLOCK;
                    fcntl(cfd, F_SETFL, flag);

                    // 将cfd上树
                    ev.data.fd = cfd;
                    ev.events = EPOLLIN | EPOLLET; // 设置成 边沿触发|水平触发
                    epoll_ctl(epfd, EPOLL_CTL_ADD, cfd, &ev);
                }
                else if (evs[i].events & EPOLLIN) // 是cfd变化 并且是读事件
                {
                    while (1)
                    {
                        char buf[5] = "";
                        int count = Read(fd, buf, sizeof(buf));
                        if (count < 0)
                        {
                            if (errno == EAGAIN) // 缓冲区无数据，读干净了
                            {
                                break;
                            }
                            else // 出错
                            {
                                printf("error \n");
                                close(fd);
                                epoll_ctl(epfd, EPOLL_CTL_DEL, fd, &evs[i]);
                                break;
                            }
                        }
                        if (count == 0)
                        {
                            printf("client close\n");
                            close(fd);
                            epoll_ctl(epfd, EPOLL_CTL_DEL, fd, &evs[i]);
                            break;
                        }
                        else
                        {
                            // printf("%s\n",buf);
                            write(1, buf, count);
                            Write(fd, buf, count);
                        }
                    }
                }
            }
        }
    } // 处理
    return 0;
}
```

#### epoll+线程池

前面我们的代码很简单，就一个进程，收到什么就回复什么，如果某个cfd是要请求下载一个文件，传输需要消耗很多时间，单进程卡在传输里无法继续监听其他事件了。

可以使用多线程的方式，主线程只负责监听连接，有任务就创建子线程去处理。这样做来一个就要开辟一个线程，很多时间就消耗在创建线程和销毁线程上，有缺陷，所以要使用线程池，在线程池的任务回调里完成。

视频介绍了简单版、复杂版。

```c
#ifndef _THREADPOOL_H
#define _THREADPOOL_H


#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/types.h>
#include <unistd.h>
#include <pthread.h>
#include "sys/epoll.h"
#include "wrap.h"


typedef struct _PoolTask
{
    int tasknum;//模拟任务编号
    void *arg;//回调函数参数
    void (*task_func)(void *arg);//任务的回调函数
    int fd;
    int epfd;
    struct epoll_event *e;
}PoolTask ;


typedef struct _ThreadPool
{
    int max_job_num;//最大任务个数
    int job_num;//实际任务个数
    PoolTask *tasks;//任务队列数组
    int job_push;//入队位置
    int job_pop;// 出队位置


    int thr_num;//线程池内线程个数
    pthread_t *threads;//线程池内线程数组
    int shutdown;//是否关闭线程池
    pthread_mutex_t pool_lock;//线程池的锁
    pthread_cond_t empty_task;//任务队列为空的条件
    pthread_cond_t not_empty_task;//任务队列不为空的条件


}ThreadPool;


void create_threadpool(int thrnum,int maxtasknum);//创建线程池--thrnum  代表线程个数，maxtasknum 最大任务个数
void destroy_threadpool(ThreadPool *pool);//摧毁线程池
//void addtask(ThreadPool *pool);//添加任务到线程池
void addtask(ThreadPool *pool,int fd,struct epoll_event *evs);//
void taskRun(void *arg);//任务回调函数


#endif




//简易版线程池
#include "threadpoolsimple.h"
#include <unistd.h>
#include <fcntl.h>
ThreadPool *thrPool = NULL;


int beginnum = 1000;


void *thrRun(void *arg)
{
    //printf("begin call %s-----\n",__FUNCTION__);
    ThreadPool *pool = (ThreadPool*)arg;
    int taskpos = 0;//任务位置
    PoolTask *task = (PoolTask *)malloc(sizeof(PoolTask));


    while(1)
    {
        //获取任务，先要尝试加锁
        pthread_mutex_lock(&thrPool->pool_lock);


        //无任务并且线程池不是要摧毁
        while(thrPool->job_num <= 0 && !thrPool->shutdown )
        {
            //如果没有任务，线程会阻塞
            pthread_cond_wait(&thrPool->not_empty_task,&thrPool->pool_lock);
        }
        
        if(thrPool->job_num)
        {
            //有任务需要处理
            taskpos = (thrPool->job_pop++)%thrPool->max_job_num;
            //printf("task out %d...tasknum===%d tid=%lu\n",taskpos,thrPool->tasks[taskpos].tasknum,pthread_self());
            //为什么要拷贝？避免任务被修改，生产者会添加任务
            memcpy(task,&thrPool->tasks[taskpos],sizeof(PoolTask));
            task->arg = task;
            thrPool->job_num--;
            //task = &thrPool->tasks[taskpos];
            pthread_cond_signal(&thrPool->empty_task);//通知生产者
        }


        if(thrPool->shutdown)
        {
            //代表要摧毁线程池，此时线程退出即可
            //pthread_detach(pthread_self());//临死前分家
            pthread_mutex_unlock(&thrPool->pool_lock);
            free(task);
            pthread_exit(NULL);
        }


        //释放锁
        pthread_mutex_unlock(&thrPool->pool_lock);
       
        task->task_func(task->arg);//执行回调函数
      
    }
    
    //printf("end call %s-----\n",__FUNCTION__);
}


//创建线程池
void create_threadpool(int thrnum,int maxtasknum)
{
    printf("begin call %s-----\n",__FUNCTION__);
    thrPool = (ThreadPool*)malloc(sizeof(ThreadPool));


    thrPool->thr_num = thrnum;
    thrPool->max_job_num = maxtasknum;
    thrPool->shutdown = 0;//是否摧毁线程池，1代表摧毁
    thrPool->job_push = 0;//任务队列添加的位置
    thrPool->job_pop = 0;//任务队列出队的位置
    thrPool->job_num = 0;//初始化的任务个数为0


    thrPool->tasks = (PoolTask*)malloc((sizeof(PoolTask)*maxtasknum));//申请最大的任务队列


    //初始化锁和条件变量
    pthread_mutex_init(&thrPool->pool_lock,NULL);
    pthread_cond_init(&thrPool->empty_task,NULL);
    pthread_cond_init(&thrPool->not_empty_task,NULL);


    int i = 0;
    thrPool->threads = (pthread_t *)malloc(sizeof(pthread_t)*thrnum);//申请n个线程id的空间
    
    pthread_attr_t attr;
    pthread_attr_init(&attr);
    pthread_attr_setdetachstate(&attr, PTHREAD_CREATE_DETACHED);
    for(i = 0;i < thrnum;i++)
    {
        pthread_create(&thrPool->threads[i],&attr,thrRun,(void*)thrPool);//创建多个线程
    }
    //printf("end call %s-----\n",__FUNCTION__);
}
//摧毁线程池
void destroy_threadpool(ThreadPool *pool)
{
    pool->shutdown = 1;//开始自爆
    pthread_cond_broadcast(&pool->not_empty_task);//诱杀


    int i = 0;
    for(i = 0; i < pool->thr_num ; i++)
    {
        pthread_join(pool->threads[i],NULL);
    }


    pthread_cond_destroy(&pool->not_empty_task);
    pthread_cond_destroy(&pool->empty_task);
    pthread_mutex_destroy(&pool->pool_lock);


    free(pool->tasks);
    free(pool->threads);
    free(pool);
}


//添加任务到线程池
//addtask(thrPool,evs[i].data.fd,e);
void addtask(ThreadPool *pool,int fd,struct epoll_event *e,int epfd)
{
    printf("begin call %s-----\n",__FUNCTION__);
    pthread_mutex_lock(&pool->pool_lock);


    //实际任务总数大于最大任务个数则阻塞等待(等待任务被处理)
    while(pool->max_job_num <= pool->job_num)
    {
        pthread_cond_wait(&pool->empty_task,&pool->pool_lock);
    }


    int taskpos = (pool->job_push++)%pool->max_job_num;
    //printf("add task %d  tasknum===%d\n",taskpos,beginnum);
    //pool->tasks[taskpos].tasknum = beginnum++;
    pool->tasks[taskpos].arg = (void*)&pool->tasks[taskpos];
    pool->tasks[taskpos].task_func = taskRun;
    pool->tasks[taskpos].fd = fd;
    pool->tasks[taskpos].epfd =epfd;
     pool->tasks[taskpos].e = e;
    pool->job_num++;


    pthread_mutex_unlock(&pool->pool_lock);


    pthread_cond_signal(&pool->not_empty_task);//通知包身工
    printf("end call %s-----\n",__FUNCTION__);
}


//任务回调函数
void taskRun(void *arg)
{
   
    PoolTask *task = (PoolTask*)arg;
     char buf[1024]="";
    
     while(1)
          {
                        char buf[1024]="";
                        int n = Read(task->fd , buf,sizeof(buf));
                        if(n < 0 )
                        {
                            if(errno ==  EAGAIN)//缓冲区如果读干净了,read返回值小于0,errno设置成EAGAIN
                                break;
                            close(task->fd );//关闭cfd
                            //epoll_ctl(epfd,EPOLL_CTL_DEL,task->fd ,&evs[i]);//将cfd上树
                            epoll_ctl(task->epfd,EPOLL_CTL_DEL,task->fd,task->e);//将cfd
                            printf("client err\n");
                            break;
                        }
                        else if(n == 0)
                        {
                             close(task->fd );//关闭cfd
                            epoll_ctl(task->epfd,EPOLL_CTL_DEL,task->fd,task->e);//将cfd
                            printf("client close\n");
                            break;


                        }
                        else
                        {
                            printf("%s\n",buf );
                           Write(task->fd ,buf,n);


                        }
         }






        free(task->e);
   
}




int main()
{
    create_threadpool(3,20);
    int i = 0;
    //创建套接字,绑定
    int lfd = tcp4bind(8000,NULL);
    //监听
    listen(lfd,128);
    //创建树
    int epfd = epoll_create(1);
    struct epoll_event ev,evs[1024];
    ev.data.fd = lfd;
    ev.events = EPOLLIN;//监听读事件
    //将ev上树
    epoll_ctl(epfd,EPOLL_CTL_ADD,lfd,&ev);
    while(1)
    {
        int nready = epoll_wait(epfd,evs,1024,-1);
        if(nready < 0)
            perr_exit("err");
        else if(nready == 0)
            continue;
        else if(nready > 0 )
        {
            for(int i=0;i<nready;i++)
            {
                if(evs[i].data.fd == lfd && evs[i].events & EPOLLIN)//如果是lfd变化,并且是读事件
                {
                        struct sockaddr_in cliaddr;
                        char buf_ip[16]="";
                        socklen_t len  = sizeof(cliaddr);
                        int cfd = Accept(lfd,(struct sockaddr *)&cliaddr,&len);
                        printf("client ip=%s port=%d\n",inet_ntop(AF_INET,
                        &cliaddr.sin_addr.s_addr,buf_ip,sizeof(buf_ip)),
                        ntohs(cliaddr.sin_port));
                        ev.data.fd = cfd;//cfd上树
                        //ev.events = EPOLLIN;//监听读事件
                        ev.events = EPOLLIN | EPOLLET ;//监听读事件
                        //设置文件描述符cfd为非阻塞
                        int flag = fcntl(cfd,F_GETFL);
                        flag |= O_NONBLOCK;
                        fcntl(cfd,F_SETFL,flag);
                        
                        epoll_ctl(epfd,EPOLL_CTL_ADD,cfd,&ev);//将cfd上树


                }
                else if(evs[i].events & EPOLLIN)//普通的读事件
                {
                   
                    struct epoll_event *e = (struct epoll_event*)malloc(sizeof(struct epoll_event));
                    memcpy(e,&evs[i],sizeof(struct epoll_event));
                    addtask(thrPool,evs[i].data.fd,e,epfd);
                   
              }




            }






        }




    }
    close(lfd);


   
    destroy_threadpool(thrPool);


    return 0;
}
```

复杂版线程池代码：

多了一个管理者线程，管理者用来决定是否要增加线程或者减少线程。自己肯定写不出来，读懂了会用会改就可以了，很多时候工作就是这样子的。

```c
#ifndef __THREADPOOL_H_
#define __THREADPOOL_H_


typedef struct threadpool_t threadpool_t;


/**
* @function threadpool_create
* @descCreates a threadpool_t object.
* @param thr_num  thread num
* @param max_thr_num  max thread size
* @param queue_max_size   size of the queue.
* @return a newly created thread pool or NULL
*/
threadpool_t *threadpool_create(int min_thr_num, int max_thr_num, int queue_max_size);


/**
* @function threadpool_add
* @desc add a new task in the queue of a thread pool
* @param pool     Thread pool to which add the task.
* @param function Pointer to the function that will perform the task.
* @param argument Argument to be passed to the function.
* @return 0 if all goes well,else -1
*/
int threadpool_add(threadpool_t *pool, void*(*function)(void *arg), void *arg);


/**
* @function threadpool_destroy
* @desc Stops and destroys a thread pool.
* @param pool  Thread pool to destroy.
* @return 0 if destory success else -1
*/
int threadpool_destroy(threadpool_t *pool);


/**
* @desc get the thread num
* @pool pool threadpool
* @return # of the thread
*/
int threadpool_all_threadnum(threadpool_t *pool);


/**
* desc get the busy thread num
* @param pool threadpool
* return # of the busy thread
*/
int threadpool_busy_threadnum(threadpool_t *pool);


#endif



#include <stdlib.h>
#include <pthread.h>
#include <unistd.h>
#include <assert.h>
#include <stdio.h>
#include <string.h>
#include <signal.h>
#include <errno.h>
#include "threadpool.h"


#define DEFAULT_TIME 10                 /*10s检测一次*/
#define MIN_WAIT_TASK_NUM 10            /*如果queue_size > MIN_WAIT_TASK_NUM 添加新的线程到线程池*/
#define DEFAULT_THREAD_VARY 10          /*每次创建和销毁线程的个数*/
#define true 1
#define false 0


typedef struct
{
    void *(*function)(void *);          /* 函数指针，回调函数 */
    void *arg;                          /* 上面函数的参数 */
} threadpool_task_t;                    /* 各子线程任务结构体 */


/* 描述线程池相关信息 */
struct threadpool_t
{
    pthread_mutex_t lock;               /* 用于锁住本结构体 */    
    pthread_mutex_t thread_counter;     /* 记录忙状态线程个数de琐 -- busy_thr_num */


    pthread_cond_t queue_not_full;      /* 当任务队列满时，添加任务的线程阻塞，等待此条件变量 */
    pthread_cond_t queue_not_empty;     /* 任务队列里不为空时，通知等待任务的线程 */


    pthread_t *threads;                 /* 存放线程池中每个线程的tid。数组 */
    pthread_t adjust_tid;               /* 存管理线程tid */
    threadpool_task_t *task_queue;      /* 任务队列(数组首地址) */


    int min_thr_num;                    /* 线程池最小线程数 */
    int max_thr_num;                    /* 线程池最大线程数 */
    int live_thr_num;                   /* 当前存活线程个数 */
    int busy_thr_num;                   /* 忙状态线程个数 */
    int wait_exit_thr_num;              /* 要销毁的线程个数 */


    int queue_front;                    /* task_queue队头下标 */
    int queue_rear;                     /* task_queue队尾下标 */
    int queue_size;                     /* task_queue队中实际任务数 */
    int queue_max_size;                 /* task_queue队列可容纳任务数上限 */


    int shutdown;                       /* 标志位，线程池使用状态，true或false */
};


void *threadpool_thread(void *threadpool);


void *adjust_thread(void *threadpool);


int is_thread_alive(pthread_t tid);
int threadpool_free(threadpool_t *pool);


//threadpool_create(3,100,100);  
threadpool_t *threadpool_create(int min_thr_num, int max_thr_num, int queue_max_size)
{
    int i;
    threadpool_t *pool = NULL;
    do
    {
        if((pool = (threadpool_t *)malloc(sizeof(threadpool_t))) == NULL)
        {  
            printf("malloc threadpool fail");
            break;                                      /*跳出do while*/
        }


        pool->min_thr_num = min_thr_num;
        pool->max_thr_num = max_thr_num;
        pool->busy_thr_num = 0;
        pool->live_thr_num = min_thr_num;               /* 活着的线程数 初值=最小线程数 */
        pool->wait_exit_thr_num = 0;
        pool->queue_size = 0;                           /* 有0个产品 */
        pool->queue_max_size = queue_max_size;
        pool->queue_front = 0;
        pool->queue_rear = 0;
        pool->shutdown = false;                         /* 不关闭线程池 */


        /* 根据最大线程上限数， 给工作线程数组开辟空间, 并清零 */
        pool->threads = (pthread_t *)malloc(sizeof(pthread_t)*max_thr_num);
        if (pool->threads == NULL)
        {
            printf("malloc threads fail");
            break;
        }
        memset(pool->threads, 0, sizeof(pthread_t)*max_thr_num);


        /* 队列开辟空间 */
        pool->task_queue = (threadpool_task_t *)malloc(sizeof(threadpool_task_t)*queue_max_size);
        if (pool->task_queue == NULL)
        {
            printf("malloc task_queue fail\n");
            break;
        }


        /* 初始化互斥琐、条件变量 */
        if (pthread_mutex_init(&(pool->lock), NULL) != 0
                || pthread_mutex_init(&(pool->thread_counter), NULL) != 0
                || pthread_cond_init(&(pool->queue_not_empty), NULL) != 0
                || pthread_cond_init(&(pool->queue_not_full), NULL) != 0)
        {
            printf("init the lock or cond fail\n");
            break;
        }


        //启动工作线程
        pthread_attr_t attr;
        pthread_attr_init(&attr);
        pthread_attr_setdetachstate(&attr, PTHREAD_CREATE_DETACHED);
        for (i = 0; i < min_thr_num; i++)
        {
            pthread_create(&(pool->threads[i]), &attr, threadpool_thread, (void *)pool);/*pool指向当前线程池*/
            printf("start thread 0x%x...\n", (unsigned int)pool->threads[i]);
        }


        //创建管理者线程
        pthread_create(&(pool->adjust_tid), &attr, adjust_thread, (void *)pool);


        return pool;


    } while (0);


    /* 前面代码调用失败时,释放poll存储空间 */
    threadpool_free(pool);


    return NULL;
}


/* 向线程池中 添加一个任务 */
//threadpool_add(thp, process, (void*)&num[i]);   /* 向线程池中添加任务 process: 小写---->大写*/


int threadpool_add(threadpool_t *pool, void*(*function)(void *arg), void *arg)
{
    pthread_mutex_lock(&(pool->lock));


    /* ==为真，队列已经满， 调wait阻塞 */
    while ((pool->queue_size == pool->queue_max_size) && (!pool->shutdown))
    {
        pthread_cond_wait(&(pool->queue_not_full), &(pool->lock));
    }


    if (pool->shutdown)
    {
        pthread_cond_broadcast(&(pool->queue_not_empty));
        pthread_mutex_unlock(&(pool->lock));
        return 0;
    }


    /* 清空 工作线程 调用的回调函数 的参数arg */
    if (pool->task_queue[pool->queue_rear].arg != NULL)
    {
        pool->task_queue[pool->queue_rear].arg = NULL;
    }


    /*添加任务到任务队列里*/
    pool->task_queue[pool->queue_rear].function = function;
    pool->task_queue[pool->queue_rear].arg = arg;
    pool->queue_rear = (pool->queue_rear + 1) % pool->queue_max_size;       /* 队尾指针移动, 模拟环形 */
    pool->queue_size++;


    /*添加完任务后，队列不为空，唤醒线程池中 等待处理任务的线程*/
    pthread_cond_signal(&(pool->queue_not_empty));
    pthread_mutex_unlock(&(pool->lock));


    return 0;
}


/* 线程池中各个工作线程 */
void *threadpool_thread(void *threadpool)
{
    threadpool_t *pool = (threadpool_t *)threadpool;
    threadpool_task_t task;


    while (true)
    {
        /* Lock must be taken to wait on conditional variable */
        /*刚创建出线程，等待任务队列里有任务，否则阻塞等待任务队列里有任务后再唤醒接收任务*/
        pthread_mutex_lock(&(pool->lock));


        /*queue_size == 0 说明没有任务，调 wait 阻塞在条件变量上, 若有任务，跳过该while*/
        while ((pool->queue_size == 0) && (!pool->shutdown))
        {  
            printf("thread 0x%x is waiting\n", (unsigned int)pthread_self());
            pthread_cond_wait(&(pool->queue_not_empty), &(pool->lock));//暂停到这


            /*清除指定数目的空闲线程，如果要结束的线程个数大于0，结束线程*/
            if (pool->wait_exit_thr_num > 0)
            {
                pool->wait_exit_thr_num--;


                /*如果线程池里线程个数大于最小值时可以结束当前线程*/
                if (pool->live_thr_num > pool->min_thr_num)
                {
                    printf("thread 0x%x is exiting\n", (unsigned int)pthread_self());
                    pool->live_thr_num--;
                    pthread_mutex_unlock(&(pool->lock));
                    //pthread_detach(pthread_self());
                    pthread_exit(NULL);
                }
            }
        }


        /*如果指定了true，要关闭线程池里的每个线程，自行退出处理---销毁线程池*/
        if (pool->shutdown)
        {
            pthread_mutex_unlock(&(pool->lock));
            printf("thread 0x%x is exiting\n", (unsigned int)pthread_self());
            //pthread_detach(pthread_self());
            pthread_exit(NULL);     /* 线程自行结束 */
        }


        /*从任务队列里获取任务, 是一个出队操作*/
        task.function = pool->task_queue[pool->queue_front].function;
        task.arg = pool->task_queue[pool->queue_front].arg;


        pool->queue_front = (pool->queue_front + 1) % pool->queue_max_size;       /* 出队，模拟环形队列 */
        pool->queue_size--;


        /*通知可以有新的任务添加进来*/
        pthread_cond_broadcast(&(pool->queue_not_full));


        /*任务取出后，立即将 线程池琐 释放*/
        pthread_mutex_unlock(&(pool->lock));


        /*执行任务*/
        printf("thread 0x%x start working\n", (unsigned int)pthread_self());
        pthread_mutex_lock(&(pool->thread_counter));                            /*忙状态线程数变量琐*/
        pool->busy_thr_num++;                                                   /*忙状态线程数+1*/
        pthread_mutex_unlock(&(pool->thread_counter));


        (*(task.function))(task.arg);                                           /*执行回调函数任务*/
        //task.function(task.arg);                                              /*执行回调函数任务*/


        /*任务结束处理*/
        printf("thread 0x%x end working\n", (unsigned int)pthread_self());
        pthread_mutex_lock(&(pool->thread_counter));
        pool->busy_thr_num--;                                       /*处理掉一个任务，忙状态数线程数-1*/
        pthread_mutex_unlock(&(pool->thread_counter));
    }


    pthread_exit(NULL);
}


/* 管理线程 */
void *adjust_thread(void *threadpool)
{
    int i;
    threadpool_t *pool = (threadpool_t *)threadpool;
    while (!pool->shutdown)
    {


        sleep(DEFAULT_TIME);                                    /*定时 对线程池管理*/


        pthread_mutex_lock(&(pool->lock));
        int queue_size = pool->queue_size;                      /* 关注 任务数 */
        int live_thr_num = pool->live_thr_num;                  /* 存活 线程数 */
        pthread_mutex_unlock(&(pool->lock));


        pthread_mutex_lock(&(pool->thread_counter));
        int busy_thr_num = pool->busy_thr_num;                  /* 忙着的线程数 */
        pthread_mutex_unlock(&(pool->thread_counter));


        /* 创建新线程 算法： 任务数大于最小线程池个数, 且存活的线程数少于最大线程个数时 如：30>=10 && 40<100*/
        if (queue_size >= MIN_WAIT_TASK_NUM && live_thr_num < pool->max_thr_num)
        {
            pthread_mutex_lock(&(pool->lock));  
            int add = 0;


            /*一次增加 DEFAULT_THREAD 个线程*/
            for (i = 0; i < pool->max_thr_num && add < DEFAULT_THREAD_VARY
                    && pool->live_thr_num < pool->max_thr_num; i++)
            {
                if (pool->threads[i] == 0 || !is_thread_alive(pool->threads[i]))
                {
                    pthread_create(&(pool->threads[i]), NULL, threadpool_thread, (void *)pool);
                    add++;
                    pool->live_thr_num++;
                }
            }


            pthread_mutex_unlock(&(pool->lock));
        }


        /* 销毁多余的空闲线程 算法：忙线程X2 小于 存活的线程数 且 存活的线程数 大于 最小线程数时*/
        if ((busy_thr_num * 2) < live_thr_num  &&  live_thr_num > pool->min_thr_num)
        {
            /* 一次销毁DEFAULT_THREAD个线程, 隨機10個即可 */
            pthread_mutex_lock(&(pool->lock));
            pool->wait_exit_thr_num = DEFAULT_THREAD_VARY;      /* 要销毁的线程数 设置为10 */
            pthread_mutex_unlock(&(pool->lock));


            for (i = 0; i < DEFAULT_THREAD_VARY; i++)
            {
                /* 通知处在空闲状态的线程, 他们会自行终止*/
                pthread_cond_signal(&(pool->queue_not_empty));
            }
        }
    }


    return NULL;
}


int threadpool_destroy(threadpool_t *pool)
{
    int i;
    if (pool == NULL)
    {
        return -1;
    }
    pool->shutdown = true;


    /*先销毁管理线程*/
    //pthread_join(pool->adjust_tid, NULL);


    for (i = 0; i < pool->live_thr_num; i++)
    {
        /*通知所有的空闲线程*/
        pthread_cond_broadcast(&(pool->queue_not_empty));
    }


    /*for (i = 0; i < pool->live_thr_num; i++)
    {
        pthread_join(pool->threads[i], NULL);
    }*/


    threadpool_free(pool);


    return 0;
}


int threadpool_free(threadpool_t *pool)
{
    if (pool == NULL)
    {
        return -1;
    }


    if (pool->task_queue)
    {
        free(pool->task_queue);
    }


    if (pool->threads)
    {
        free(pool->threads);
        pthread_mutex_lock(&(pool->lock));
        pthread_mutex_destroy(&(pool->lock));
        pthread_mutex_lock(&(pool->thread_counter));
        pthread_mutex_destroy(&(pool->thread_counter));
        pthread_cond_destroy(&(pool->queue_not_empty));
        pthread_cond_destroy(&(pool->queue_not_full));
    }


    free(pool);
    pool = NULL;


    return 0;
}


int threadpool_all_threadnum(threadpool_t *pool)
{
    int all_threadnum = -1;


    pthread_mutex_lock(&(pool->lock));
    all_threadnum = pool->live_thr_num;
    pthread_mutex_unlock(&(pool->lock));


    return all_threadnum;
}


int threadpool_busy_threadnum(threadpool_t *pool)
{
    int busy_threadnum = -1;


    pthread_mutex_lock(&(pool->thread_counter));
    busy_threadnum = pool->busy_thr_num;
    pthread_mutex_unlock(&(pool->thread_counter));


    return busy_threadnum;
}


int is_thread_alive(pthread_t tid)
{
    int kill_rc = pthread_kill(tid, 0);     //发0号信号，测试线程是否存活
    if (kill_rc == ESRCH)
    {
        return false;
    }


    return true;
}


/*测试*/


#if 1
/* 线程池中的线程，模拟处理业务 */
void *process(void *arg)
{
    printf("thread 0x%x working on task %d\n ",(unsigned int)pthread_self(),*(int *)arg);
    sleep(1);
    printf("task %d is end\n", *(int *)arg);


    return NULL;
}


int main(void)
{
    /*threadpool_t *threadpool_create(int min_thr_num, int max_thr_num, int queue_max_size);*/
    threadpool_t *thp = threadpool_create(3,100,100);   /*创建线程池，池里最小3个线程，最大100，队列最大100*/
    printf("pool inited");


    //int *num = (int *)malloc(sizeof(int)*20);
    int num[20], i;
    for (i = 0; i < 20; i++)
    {
        num[i]=i;
        printf("add task %d\n",i);
        threadpool_add(thp, process, (void*)&num[i]);   /* 向线程池中添加任务 */
    }


    sleep(10);                                          /* 等子线程完成任务 */
    threadpool_destroy(thp);


    return 0;
}


#endif
```

#### epoll反应堆

之前的代码里文件描述符、事件、处理任务都是分开写的；  
反应堆借鉴了c++思想，将文件描述符、事件、处理任务的回调封装在一起了(结构体里)。这个就叫核反应堆了，只是一种编程思想。

libevent事件库（实际工作就是用的这个网络事件库，自己手写epoll反应堆这些容易出错也不稳定）里面的代码很好的实现了这个功能。下面例子是单线程的，体会一下反应堆的封装思想。

![在这里插入图片描述](https://img-blog.csdnimg.cn/5f4b2bbe5ee5484c9bfbdf43adcd8652.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

```c
//反应堆简单版
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <fcntl.h>
#include <string.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <sys/epoll.h>
#include "wrap.h"

#define _BUF_LEN_  1024
#define _EVENT_SIZE_ 1024

//全局epoll树的根
int gepfd = 0;

//事件驱动结构体   【反应堆】
typedef struct xx_event
{
    int 	fd;
    int 	events;
    void 	(*call_back)(int fd,int events,void *arg);
    void 	*arg;
    char 	buf[1024];
    int 	buflen;
    int 	epfd;
}xevent;

xevent myevents[_EVENT_SIZE_+1];

void readData(int fd,int events,void *arg);

// 添加事件并上树
// eventadd(lfd,EPOLLIN,initAccept,&myevents[_EVENT_SIZE_-1],&myevents[_EVENT_SIZE_-1]);
void eventadd(int fd,int events,void (*call_back)(int ,int ,void *),void *arg,xevent *ev)
{
    ev->fd = fd;
    ev->events = events;
    //ev->arg = arg;//代表结构体自己,可以通过arg得到结构体的所有信息
    ev->call_back = call_back;

    struct epoll_event epv;
    epv.events = events;
    epv.data.ptr = ev;//核心思想
    epoll_ctl(gepfd,EPOLL_CTL_ADD,fd,&epv);//上树
}

// 修改事件
// eventset(fd,EPOLLOUT,senddata,arg,ev);
void eventset(int fd,int events,void (*call_back)(int ,int ,void *),void *arg,xevent *ev)
{
    ev->fd = fd;
    ev->events = events;
    //ev->arg = arg;
    ev->call_back = call_back;

    struct epoll_event epv;
    epv.events = events;
    epv.data.ptr = ev;
    epoll_ctl(gepfd,EPOLL_CTL_MOD,fd,&epv);//修改
}

// 删除事件
void eventdel(xevent *ev,int fd,int events)
{
	printf("begin call %s\n",__FUNCTION__);

    ev->fd = 0;
    ev->events = 0;
    ev->call_back = NULL;
    memset(ev->buf,0x00,sizeof(ev->buf));
    ev->buflen = 0;

    struct epoll_event epv;
    epv.data.ptr = NULL;
    epv.events = events;
    epoll_ctl(gepfd,EPOLL_CTL_DEL,fd,&epv);//下树
}

// 发送数据
void senddata(int fd,int events,void *arg)
{
    printf("begin call %s\n",__FUNCTION__);

    xevent *ev = arg;
    Write(fd,ev->buf,ev->buflen);
    eventset(fd,EPOLLIN,readData,arg,ev);
}

// 读数据
void readData(int fd,int events,void *arg)
{
    printf("begin call %s\n",__FUNCTION__);
    xevent *ev = arg;

    ev->buflen = Read(fd,ev->buf,sizeof(ev->buf));
    if(ev->buflen>0) //读到数据
	{	
		// 以前我们这里收到数据直接再发出来  这里要修改事件，因为默认是水平触发，要判断什么时候能写(可能写缓冲区已满)  不判断直接调用write可能会阻塞，这样就不能监听新的连接到来了
		// void eventset(int fd,int events,void (*call_back)(int ,int ,void *),void *arg,xevent *ev)
        eventset(fd,EPOLLOUT,senddata,arg,ev);

    }
	else if(ev->buflen==0) //对方关闭连接
	{
        Close(fd);
        eventdel(ev,fd,EPOLLIN);// 删除  下树
    }

}
// 新连接处理
void initAccept(int fd,int events,void *arg)
{
    printf("begin call %s,gepfd =%d\n",__FUNCTION__,gepfd);// __FUNCTION__  函数名

    int i;
    struct sockaddr_in addr;
    socklen_t len = sizeof(addr);
    int cfd = Accept(fd,(struct sockaddr*)&addr,&len);//是否会阻塞？  不会 传过来的是变化的fd
	
	//查找 myevents 数组中可用的位置
    for(i = 0 ; i < _EVENT_SIZE_; i ++)
	{
        if(myevents[i].fd==0)
		{
            break;
        }
    }

    //设置读事件
    eventadd(cfd,EPOLLIN,readData,&myevents[i],&myevents[i]);
}

int main(int argc,char *argv[])
{
	//创建socket
    int lfd = Socket(AF_INET,SOCK_STREAM,0);

    //端口复用
    int opt = 1;
    setsockopt(lfd,SOL_SOCKET,SO_REUSEADDR,&opt,sizeof(opt));

	//绑定
    struct sockaddr_in servaddr;
    servaddr.sin_family = AF_INET;
    servaddr.sin_port = htons(8888);
    servaddr.sin_addr.s_addr = htonl(INADDR_ANY);
    Bind(lfd,(struct sockaddr*)&servaddr,sizeof(servaddr));
    
	//监听
    Listen(lfd,128);

	//创建epoll树根节点
    gepfd = epoll_create(1024);
    printf("gepfd === %d\n",gepfd);

    struct epoll_event events[1024];

    //添加最初始事件，将侦听的描述符上树
    eventadd(lfd,EPOLLIN,initAccept,&myevents[_EVENT_SIZE_],&myevents[_EVENT_SIZE_]);
    //void eventadd(int fd,int events,void (*call_back)(int ,int ,void *),void *arg,xevent *ev)

    while(1)
	{
        int nready = epoll_wait(gepfd,events,1024,-1);
		if(nready<0) //调用epoll_wait失败
		{
			perr_exit("epoll_wait error");
			
		}
        else if(nready>0) //调用epoll_wait成功,返回有事件发生的文件描述符的个数
		{
            int i = 0;
            for(i=0;i<nready; i++)
			{
                xevent *xe = events[i].data.ptr;//取ptr指向结构体地址
                printf("fd=%d\n",xe->fd);
				// 监听的事件等于返回的事件  此处不判断也行
                if(xe->events & events[i].events)
				{
                    xe->call_back(xe->fd,xe->events,xe);	//调用事件对应的回调
                }
            }
        }
    }

	//关闭监听文件描述符
	Close(lfd);

    return 0;
}
```

### 4.7 Web服务器介绍

#### 4.7.1 web服务器简介

Web服务器又称WWW服务器、网站服务器等  
特点  
使用HTTP协议与客户机浏览器进行信息交流  
不仅能存储信息，还能在用户通过web浏览器提供的信息的基础上运行脚本和程序  
该服务器需可安装在UNIX、Linux或Windows等操作系统上  
著名的服务器有Apache、Tomcat、 IIS等

#### 4.7.2 HTTP协议

Webserver—HTTP协议  
概念  
一种详细规定了浏览器和万维网服务器之间互相通信的规则，通过因特网传送万维网文档的数据传送协议  
特点  
1、支持C/S架构  
2、简单快速：客户向服务器请求服务时，只需传送请求方法和路径 ，常用方法:GET、POST  
3、无连接：限制每次连接只处理一个请求  
4、无状态：即如果后续处理需要前面的信息，它必须重传，这样可能导致每次连接传送的数据量会增大

#### 4.7.2 Webserver 通信过程

![在这里插入图片描述](https://img-blog.csdnimg.cn/415334dfc20b45e28e6d90a3da489f8f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_9,color_FFFFFF,t_70,g_se,x_16)

### 4.8 Web编程开发

网页浏览（使用GET方式）  
客户端浏览器请求：

![在这里插入图片描述](https://img-blog.csdnimg.cn/ae6ea864626e404c9cda9b34d242e306.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_8,color_FFFFFF,t_70,g_se,x_16)

服务器收到的数据：

![在这里插入图片描述](https://img-blog.csdnimg.cn/e64b996b013e4e3baec03f2a940c9b9f.png)

服务器应答的格式：请求成功  
![在这里插入图片描述](https://img-blog.csdnimg.cn/11528eeefdc94f17bcfb29a3cd8b1936.png)

服务器应答的格式：请求失败

![在这里插入图片描述](https://img-blog.csdnimg.cn/478e251f90e44ab5a4181289a0b719a0.png)

wrap.c  
wrap.h  
pub.h

```c
#ifndef _PUB_H
#define _PUB_H
#include <stdlib.h>
#include <stdio.h>
#include <unistd.h>
#include <errno.h>
#include <string.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <strings.h>
#include <ctype.h>
char *get_mime_type(char *name);
int get_line(int sock, char *buf, int size);
int hexit(char c);//16进制转10进制
void strencode(char* to, size_t tosize, const char* from);//编码
void strdecode(char *to, char *from);//解码
#endif
```

pub.c

```c
#include "pub.h"
//通过文件名字获得文件类型
char *get_mime_type(char *name)
{
    char* dot;

    dot = strrchr(name, '.');	//自右向左查找‘.’字符;如不存在返回NULL
    /*
     *charset=iso-8859-1	西欧的编码，说明网站采用的编码是英文；
     *charset=gb2312		说明网站采用的编码是简体中文；
     *charset=utf-8			代表世界通用的语言编码；
     *						可以用到中文、韩文、日文等世界上所有语言编码上
     *charset=euc-kr		说明网站采用的编码是韩文；
     *charset=big5			说明网站采用的编码是繁体中文；
     *
     *以下是依据传递进来的文件名，使用后缀判断是何种文件类型
     *将对应的文件类型按照http定义的关键字发送回去
     */
    if (dot == (char*)0)
        return "text/plain; charset=utf-8";
    if (strcmp(dot, ".html") == 0 || strcmp(dot, ".htm") == 0)
        return "text/html; charset=utf-8";
    if (strcmp(dot, ".jpg") == 0 || strcmp(dot, ".jpeg") == 0)
        return "image/jpeg";
    if (strcmp(dot, ".gif") == 0)
        return "image/gif";
    if (strcmp(dot, ".png") == 0)
        return "image/png";
    if (strcmp(dot, ".css") == 0)
        return "text/css";
    if (strcmp(dot, ".au") == 0)
        return "audio/basic";
    if (strcmp( dot, ".wav") == 0)
        return "audio/wav";
    if (strcmp(dot, ".avi") == 0)
        return "video/x-msvideo";
    if (strcmp(dot, ".mov") == 0 || strcmp(dot, ".qt") == 0)
        return "video/quicktime";
    if (strcmp(dot, ".mpeg") == 0 || strcmp(dot, ".mpe") == 0)
        return "video/mpeg";
    if (strcmp(dot, ".vrml") == 0 || strcmp(dot, ".wrl") == 0)
        return "model/vrml";
    if (strcmp(dot, ".midi") == 0 || strcmp(dot, ".mid") == 0)
        return "audio/midi";
    if (strcmp(dot, ".mp3") == 0)
        return "audio/mpeg";
    if (strcmp(dot, ".ogg") == 0)
        return "application/ogg";
    if (strcmp(dot, ".pac") == 0)
        return "application/x-ns-proxy-autoconfig";

    return "text/plain; charset=utf-8";
}
/**********************************************************************/
/* Get a line from a socket, whether the line ends in a newline,
 * carriage return, or a CRLF combination.  Terminates the string read
 * with a null character.  If no newline indicator is found before the
 * end of the buffer, the string is terminated with a null.  If any of
 * the above three line terminators is read, the last character of the
 * string will be a linefeed and the string will be terminated with a
 * null character.
 * Parameters: the socket descriptor
 *             the buffer to save the data in
 *             the size of the buffer
 * Returns: the number of bytes stored (excluding null) */
/**********************************************************************/
//获得一行数据，每行以\r\n作为结束标记
int get_line(int sock, char *buf, int size)
{
    int i = 0;
    char c = '\0';
    int n;

    while ((i < size - 1) && (c != '\n'))
    {
        n = recv(sock, &c, 1, 0);
        /* DEBUG printf("%02X\n", c); */
        if (n > 0)
        {
            if (c == '\r')
            {
                n = recv(sock, &c, 1, MSG_PEEK);//MSG_PEEK 从缓冲区读数据，但是数据不从缓冲区清除
                /* DEBUG printf("%02X\n", c); */
                if ((n > 0) && (c == '\n'))
                    recv(sock, &c, 1, 0);
                else
                    c = '\n';
            }
            buf[i] = c;
            i++;
        }
        else
            c = '\n';
    }
    buf[i] = '\0';

    return(i);
}

//下面的函数第二天使用
/*
 * 这里的内容是处理%20之类的东西！是"解码"过程。
 * %20 URL编码中的‘ ’(space)
 * %21 '!' %22 '"' %23 '#' %24 '$'
 * %25 '%' %26 '&' %27 ''' %28 '('......
 * 相关知识html中的‘ ’(space)是&nbsp
 */
 // %E8%8B%A6%E7%93%9C.txt
void strdecode(char *to, char *from)
{
    for ( ; *from != '\0'; ++to, ++from) {

        if (from[0] == '%' && isxdigit(from[1]) && isxdigit(from[2])) { //依次判断from中 %20 三个字符

            *to = hexit(from[1])*16 + hexit(from[2]);//字符串E8变成了真正的16进制的E8
            from += 2;                      //移过已经处理的两个字符(%21指针指向1),表达式3的++from还会再向后移一个字符
        } else
            *to = *from;
    }
    *to = '\0';
}

//16进制数转化为10进制, return 0不会出现
int hexit(char c)
{
    if (c >= '0' && c <= '9')
        return c - '0';
    if (c >= 'a' && c <= 'f')
        return c - 'a' + 10;
    if (c >= 'A' && c <= 'F')
        return c - 'A' + 10;

    return 0;
}

//"编码"，用作回写浏览器的时候，将除字母数字及/_.-~以外的字符转义后回写。
//strencode(encoded_name, sizeof(encoded_name), name);
void strencode(char* to, size_t tosize, const char* from)
{
    int tolen;

    for (tolen = 0; *from != '\0' && tolen + 4 < tosize; ++from) {
        if (isalnum(*from) || strchr("/_.-~", *from) != (char*)0) {
            *to = *from;
            ++to;
            ++tolen;
        } else {
            sprintf(to, "%%%02x", (int) *from & 0xff);
            to += 3;
            tolen += 3;
        }
    }
    *to = '\0';
}
```

server.c

```c
#include <stdio.h>
#include "wrap.h"
#include <sys/epoll.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <fcntl.h>
#include <sys/stat.h>
#include "pub.h"
#include "dirent.h"
#include <signal.h>
#define PORT 8000

// 发送头
void send_header(int cfd, int code, char *info, char *filetype, int length)
{
    // 状态行
    char buf[1024] = "";
    int len = 0;
    len = sprintf(buf, "HTTP/1.1 %d %s\r\n", code, info);
    send(cfd, buf, len, 0);                                 // 发送
    // 消息头
    len = sprintf(buf, "Content-Type:%s\r\n", filetype);
    send(cfd, buf, len, 0);                                 // 发送     分开发送，也可以放到一起发送   前面已经建立tcp连接了
    if (length > 0)
    {
        len = sprintf(buf, "Content-Length:%d\r\n", length);
        send(cfd, buf, len, 0);                             // 发送
    }
    // 空行
    send(cfd, "\r\n", 2, 0);                                // 发送
}
// 发送文件
void send_file(int cfd, char *filepath, int close_flag, struct epoll_event *ev, int epfd)
{
    int fd = open(filepath, O_RDONLY);
    if (fd < 0)
    {
        perror("");
        return;
    }
    char buf[1024] = "";
    while (1)
    {
        int count = read(fd, buf, sizeof(buf));
        if (count <= 0)
        {
            break;
        }
        int n = write(cfd, buf, count);
        printf("write = %d\n", n);
    }
    close(fd);
    if (close_flag == 1)// 发送完了 关闭连接
    {

        epoll_ctl(epfd, EPOLL_CTL_DEL, cfd, ev);
        close(cfd);
    }
}
// 请求回复
void request_http(char *msg, struct epoll_event *ev, int epfd)
{
    signal(SIGPIPE, SIG_IGN);
    int cfd = ev->data.fd;
    // printf("[%s]\n",msg);
    char method[256];
    char content[256];
    // GET /abc HTTP/1.1
    sscanf(msg, "%[^ ] %[^ ]", method, content);
    printf("[%s]   [%s]\n", method, content);
    if (strcasecmp(method, "get") == 0)
    {
        //  GET  /a.txt
        char *strfile = content + 1;
        strdecode(strfile, strfile); // 转码
        if (*strfile == 0)           // 如果没有请求 ，默认请求当前目录
        {
            strfile = "./";
        }
        struct stat s;
        // 文件不存在
        if (stat(strfile, &s) == -1) 
        {
            printf("文件不存在\n");
            // 发送错误信息头部
            send_header(cfd, 404, "NOT FOUND", get_mime_type("*.html"), 0);
            // 发送error.html
            send_file(cfd, "error.html", 1, ev, epfd);
        }
        // 文件存在
        else 
        {
            // 普通文件
            if (S_ISREG(s.st_mode))
            {
                printf("普通文件\n");
                // 发送信息头部
                send_header(cfd, 200, "OK", get_mime_type(strfile), s.st_size);
                // 发送文件
                send_file(cfd, strfile, 1, ev, epfd);
            }
            else if (S_ISDIR(s.st_mode)) // 目录
            {
                printf("目录\n");
                // 发送信息头部
                send_header(cfd, 200, "OK", get_mime_type("*.html"), 0);
                // 发送dir_header.html
                send_file(cfd, "dir_header.html", 0, ev, epfd);
                // 发送列表  组包
                struct dirent **list = NULL;
                int ndir = scandir(strfile, &list, NULL, alphasort);
                int i = 0;
                printf("001\n");
                for (i = 0; i < ndir; i++)
                {
                    printf("002  \n");
                    printf("%p\n", list[i]);
                    printf("[%s]\n", list[i]->d_name);
                    char listbuf[256] = "";
                    int n = 0;
                    if (list[i]->d_type == DT_REG)
                    {
                        n = sprintf(listbuf, "<li><a href=%s>%s</a></li>", list[i]->d_name, list[i]->d_name);
                    }
                    else if (list[i]->d_type == DT_DIR)
                    {

                        n = sprintf(listbuf, "<li><a href=%s/>%s/</a></li>", list[i]->d_name, list[i]->d_name);
                    }
                    send(cfd, listbuf, n, 0);
                    printf("0021\n");
                    free(list[i]);

                    printf("003\n");
                }
                free(list);

                // 发送dir_tail.html
                send_file(cfd, "dir_tail.html", 1, ev, epfd);
            }
        }
    }
}
int main(int argc, char const *argv[])
{
    // 切换工作目录
    char *curdir = getenv("PWD");// 获取当前工作目录
    char mydir[256] = "";
    strcpy(mydir, curdir);
    strcat(mydir, "/web-http");
    chdir(mydir);

    int lfd = tcp4bind(PORT, NULL);
    Listen(lfd, 128);
    int epfd = epoll_create(1);
    if (epfd < 0)
    {
        perror("");
        exit(0);
    }
    struct epoll_event ev, evs[1024];
    ev.events = EPOLLIN;
    ev.data.fd = lfd;
    epoll_ctl(epfd, EPOLL_CTL_ADD, lfd, &ev);
    while (1)
    {
        int n = epoll_wait(epfd, evs, 1024, -1);
        if (n < 0)
        {
            perror("");
            exit(0);
        }
        else
        {
            for (int i = 0; i < n; i++)
            {
                if (evs[i].data.fd == lfd && evs[i].events & EPOLLIN)
                {
                    struct sockaddr_in cliaddr;
                    socklen_t len = sizeof(cliaddr);
                    char ip[16] = "";
                    int cfd = Accept(lfd, (struct sockaddr *)&cliaddr, &len);
                    printf("client ip=%s port=%d\n", inet_ntop(AF_INET, &cliaddr.sin_addr.s_addr, ip, 16),
                           ntohs(cliaddr.sin_port));
                    int flag = fcntl(cfd, F_GETFL);
                    flag |= O_NONBLOCK;
                    fcntl(cfd, F_SETFL, flag);// 设置成非阻塞,否则write大文件时阻塞不能接受新的连接了

                    ev.events = EPOLLIN;
                    ev.data.fd = cfd;
                    epoll_ctl(epfd, EPOLL_CTL_ADD, cfd, &ev);
                }
                else if (evs[i].events & EPOLLIN)
                {
                    char msg[1500] = "";
                    int count = Read(evs[i].data.fd, msg, sizeof(msg));
                    if (count < 0)
                    {
                        perror("");
                        close(evs[i].data.fd);
                        epoll_ctl(epfd, EPOLL_CTL_DEL, evs[i].data.fd, &evs[i]);
                    }
                    else if (count == 0)
                    {
                        printf("client close\n");
                        close(evs[i].data.fd);
                        epoll_ctl(epfd, EPOLL_CTL_DEL, evs[i].data.fd, &evs[i]);
                    }
                    else
                    {
                        request_http(msg, &evs[i], epfd);
                    }
                }
            }
        }
    }

    return 0;
}
```

## 第五章：网络通信过程

### 5.1网络通信概述

通过对TCP、UDP的编程学习，能够完成对实际项目需求中网络功能的开发，为了提高程序的稳定性以及效率等等，通常会使用多线程、多进程开发；根据功能需求的不同，可以利用C/S、B/S模式进行开发

作为嵌入式工程师，需要对整个网络通信的过程进行掌握，从一个整体的角度来开发出更加稳定、效率的网络程序

想一想  
PC0怎样才能够访问到www.qfedu.com

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/1e7ebc0690404309a303ae174e50ef25.png)

SWITCH：交换机

router：路由器

**网络通信过程分析软件**

Packet Tracer 是由Cisco公司发布的一个辅助学习工具，提供了设计、配置、排除网络故障网络模拟环境

可以直接使用拖曳方法建立网络拓扑，并可提供数据包在网络中行进的详细处理过程，观察网络实时运行情况

![在这里插入图片描述](https://img-blog.csdnimg.cn/6b2e185e11ad488382e3240514979026.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_11,color_FFFFFF,t_70,g_se,x_16)

### 5.2通信过程（PC+switch）

#### 5.2.1交换机介绍

网络交换机（又称“网络交换器”），是一个**扩大网络的器材**，可以把更多的计算机等网络设备连接到当前的网络中。

具有性价比高、高度灵活、相对简单、易于实现等特点  
以太网技术已成为当今最重要的一种局域网组网技术，网络交换机也就成为了最普及的交换机

家用级

![在这里插入图片描述](https://img-blog.csdnimg.cn/a603ce8b3c3a4ff892ac7b765c81c468.png)

企业级(好几千元一个)，常见品牌：华为(最好但最贵)、TP-LINK(大部分选择)、思科、腾达

![在这里插入图片描述](https://img-blog.csdnimg.cn/33f55eb5120e4754bfec5093f587ffc0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_8,color_FFFFFF,t_70,g_se,x_16)

#### 5.2.2交换机功能

1、转发过滤：当一个数据帧的目的地址在MAC地址表中有映射时，它被转发到连接目的节点的端口而不是所有端口（如该数据帧为广播/组播帧则转发至所有端口）

2、学习功能：以太网交换机了解每一端口相连设备的MAC地址，并将地址同相应的端口映射起来存放在交换机缓存中的MAC地址表中

3、目前交换机还具备了一些新的功能，如对VLAN（虚拟局域网）的支持、对链路汇聚的支持，甚至有的还具有防火墙的功能

4、交换机只有lan口，路由器既有wan口又有lan口。

![在这里插入图片描述](https://img-blog.csdnimg.cn/1228316eca754077b087a1b4e1f7862f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

> 左边是路由表，交换机端口1对应pc1的MAC地址；端口6对应pc2的MAC地址；端口7连接的路由器，路由器至少有2个网卡。比如pc2给pc1发送数据携带目的MAC和源MAC，交换机通过查表转发以太网帧数据。

#### 5.2.3通信过程（PC+switch）

通过交换机可以组成一个简单的网络

![在这里插入图片描述](https://img-blog.csdnimg.cn/85b7d59d58444bc7899cb15d9867b2d1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_9,color_FFFFFF,t_70,g_se,x_16)

举例  
每台PC必须手动设置ip、netmask  
192.168.1.1/255.255.255.0  
……  
192.168.1.8/255.255.255.0

总结：  
如果PC不知道目标IP所对应的MAC，那么可以看出，PC会先发送ARP广播，得到对方的MAC地址，然后再进行数据的传送。  
当switch第一次收到ARP广播数据，会把ARP广播数据包转发给所有端口（除来源端口）；如果以后还有PC询问此IP的MAC，那么只是向目标的端口进行转发数据(如果是HUB集线器，则每次都广播)

每台PC都会有一个ARP缓存表，用来记录IP所对应的的MAC。

![在这里插入图片描述](https://img-blog.csdnimg.cn/62fc1e7201be452ab24ce52f455e66e8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_17,color_FFFFFF,t_70,g_se,x_16)

### 5.3通信过程（PC+switch+router）

#### 路由器介绍

每个路由器至少==有两个网卡==，两边分别与两个局域网连接(路由器的网卡充当网关，其IP地址最后一位一般设置为1或者254)

路由器（Router）又称网关设备（Gateway）是用于连接多个逻辑上分开的网络  
所谓逻辑网络是代表一个单独的网络或者一个子网。当数据从一个子网传输到另一个子网时，可通过路由器的路由功能来完成  
具有判断网络地址和选择IP路径的功能

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/082c7ad8afaa4e94b72de0001cdeadaf.png)

家用级

![在这里插入图片描述](https://img-blog.csdnimg.cn/7bf7bfafdd2d4d4589326cb3b525bde9.png)

企业级

![在这里插入图片描述](https://img-blog.csdnimg.cn/e0f19ac3a7be48c4b9de406690c26536.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_8,color_FFFFFF,t_70,g_se,x_16)

通过2个router，2个switch，4台PC组成的网络

![在这里插入图片描述](https://img-blog.csdnimg.cn/2d3de8a00c3e4bd6bb309645534cf2dd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_17,color_FFFFFF,t_70,g_se,x_16)

举例  
配置所有PC的IP、netmask  
PC0（192.168.1.1/24）  
PC1（192.168.1.2/24）  
PC2（192.168.3.1/24）  
PC3（192.168.3.2/24）

配置router（**每个router有2个IP**，两个网卡）  
router0(192.168.1.254/24,192.168.2.1/24)  
router1(192.168.3.254/24,192.168.2.2/24)

配置后的网络信息如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/1657d4822a8b4294841661726661a769.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_17,color_FFFFFF,t_70,g_se,x_16)

参考演示demo-2(PC0 ping PC3)

总结：  
不在同一网段的PC，需要设置默认网关才能把数据传送过去，通常情况下，都会把路由器设为默认网关。  
当路由器收到一个其它网段的数据包时，会根据“路由表”来决定把此数据包发送到哪个端口(网卡)；路由表的设定有静态和动态方法。

在dos控制端下，可通过输入命令查看路由表。

![在这里插入图片描述](https://img-blog.csdnimg.cn/5678b8f5fbeb4b31b5a6d16fd93b251e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_17,color_FFFFFF,t_70,g_se,x_16)

#### 路由器报文分析

详细看视频

### 5.4通信过程（浏览器跨网访问Web服务器）

网络通信过程（复杂）  
以PC0访问www.helloworld.com举例：

![在这里插入图片描述](https://img-blog.csdnimg.cn/cb64f00e2a1744d394b4adb6e485e6f9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_17,color_FFFFFF,t_70,g_se,x_16)

配置网络设备  
PC：IP、NETMASK、DFGATEWAY、DNS  
ROUTER：IP、NETMASK、路由表  
Server：右上是DNS Server、右下是Web Server

![在这里插入图片描述](https://img-blog.csdnimg.cn/36a1d82d4915426486848a17aec10532.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_17,color_FFFFFF,t_70,g_se,x_16)

总结  
1、DNS服务器的作用是解析出IP，即域名解析  
2、DFGATEWAY指定发往其它网段的数据包转发的路径  
3、在路由器中路由表指定数据包的“下一跳”的地址  
4、公有IP、私有IP

> pc里要先设置好DNS服务器的IP，当用户直接用域名访问Web Server的时候，pc会先通过ip访问DNS域名服务器，让DNS服务器告诉pc要访问的Web Server的ip。![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/97e6b20f6e154787a27410d60e05af32.png)

## 第六章：Linux防火墙(视频教程里删掉此部分)

### 6.1认识防火墙

防火墙的定义  
防火墙被定义成一个或一组设备，它在网络之间执行访问控制策略  
![在这里插入图片描述](https://img-blog.csdnimg.cn/c28bee8eac5c42dfb3d430f64493649d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_15,color_FFFFFF,t_70,g_se,x_16)

防火墙的分类  
硬件防火墙、软件防火墙

防火墙最重要的任务  
1、切割被信任(如子域)与不被信任(如 Internet)的网段  
2、划分出可提供Internet的服务与必须受保护的服务  
3、分析出可接受与不可接受的数据包状态  
你需不需要防火墙？  
理论上需要，但你必须知道系统哪些数据与服务需要保护、针对需要受保护的服务来设置防火墙规则

### 6.2防火墙的一般网络布线示意

单一网络，仅有一个路由器  
只要管理这一台防火墙主机就可以很轻易的将来自Internet的不良网络数据包阻挡掉  
![在这里插入图片描述](https://img-blog.csdnimg.cn/705eed77f75647e9a0e6fed74628a9f4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_15,color_FFFFFF,t_70,g_se,x_16)

如果入侵从LAN进入，那咋办？  
内部网络包含安全性更高的子网，需要内部防火墙切开子网  
![在这里插入图片描述](https://img-blog.csdnimg.cn/dd8c19d9bce84e5d84d216dd44791dff.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_15,color_FFFFFF,t_70,g_se,x_16)

架设在防火墙后端的主机服务器

![在这里插入图片描述](https://img-blog.csdnimg.cn/a4ba25c9ffba4319be211f26de118841.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_15,color_FFFFFF,t_70,g_se,x_16)

### 6.3防火墙的使用限制

设置了防火墙也不能保证网络就一定安全  
防火墙不能有效阻止病毒或木马程序

![在这里插入图片描述](https://img-blog.csdnimg.cn/73dde7f64750402f8362ae48b3a3a96a.png)

防火墙对于来自内部LAN的攻击无能为力

![在这里插入图片描述](https://img-blog.csdnimg.cn/55589b4e1c8d4338800e4f858146a135.png)

### 6.4Linux的数据包过滤软件:iptables

#### 6.4.1规则顺序的重要性

iptables根据数据包的分析资料“对比”预先定义的规则内容  
对比结果符合Rule1，此时这个网络数据包就会进行Action1的动作，而不会理会后续的Rule2、Rule3等规则了

![在这里插入图片描述](https://img-blog.csdnimg.cn/37e9574c9a7442b1aa173dac1de03840.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_8,color_FFFFFF,t_70,g_se,x_16)

#### 6.4.2 iptables的表格与链

iptables有多个表格(table)。而每个表格又有多个链(chain)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/6666da7711304458a22e72a633ee8f44.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_13,color_FFFFFF,t_70,g_se,x_16)

1、Filter(过滤器):与本机数据有关  
INPUT:主要与想要进入Linux本机的数据包有关  
OUTPUT:主要与Linux本机所要送出的数据包有关  
FORWARD:与本机无关，传送数据到后端的计算机中  
2、NAT(地址转换)：主要用来进行来源和目的地的ip或port的转换  
PREROUTING:在进行路由判断之前所要进行的规则  
POSTROUTING:在进行路由判断之后所要进行的规则  
OUTPUT:与发出去的数据包有关  
3、Mangle(破坏者):主要与特殊的数据包的路由标志有关(很少使用)

1.规则的查看  
iptables [-t tables] [-L] [-nv]  
-t:后面接table，例如nat或filter,若省略则使用filter  
-L:列出目前的table的规则  
-n:不进行IP与HOSTNAME的反查，这样显示速度快  
-v:列出更多的信息(数据包的位数、相关的网络接口)  
iptables-save会列出完整的防火墙规则(推荐)

![在这里插入图片描述](https://img-blog.csdnimg.cn/c2e8a18b927f40cfae798a73ee3bf93c.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/4aef2c4d9ae845a78cb1f4937e17bdd5.png)

2.规则的清除  
iptables [-t tables] [-FXZ]  
-F:清除所有已定制的规则  
-X:除掉所有用户"自定义"的chain  
-Z:将所有的chain的计数与流量统计都归零  
例：清除本机防火墙(filter)的所有规则  
![在这里插入图片描述](https://img-blog.csdnimg.cn/abeb642bd0ac4c7f8052d32ab83e80bb.png)

3.定义默认策略(policy)  
iptables [-t nat] -p [INPUT,OUTPUT,FORWARD] [ACCEPT,DROP]  
-P:定义策略(Policy),P为大写  
ACCEPT:该数据包可接受  
DROP：该数据包直接丢弃，不会让client知道为何丢弃  
例：将本机的INPUT设置为DROP，其他设置为ACCEPT注意先清除所有规则  
![在这里插入图片描述](https://img-blog.csdnimg.cn/f690217c42c84622b6561525f3eaedb0.png)

4.IP、网络及接口设备的防火墙设置  
![在这里插入图片描述](https://img-blog.csdnimg.cn/0968183febf04c319ed7b188864c2e67.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_15,color_FFFFFF,t_70,g_se,x_16)

例1：设置lo成为受信任的设备，亦即进出lo的数据包都予以接受  
![在这里插入图片描述](https://img-blog.csdnimg.cn/19b35172f13740039bd975d0c961ce05.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/016bf1dc1dd1466d825ed51f1a853cd8.png)

例2：只要来自内网的(172.20.223.0/24)的数据包都接受  
![在这里插入图片描述](https://img-blog.csdnimg.cn/d485e96e527f4ea39072c0fd902513c4.png)

例3：只要是来自172.20.223.32就接受，但是来自172.20.223.91的数据包就丢弃  
![在这里插入图片描述](https://img-blog.csdnimg.cn/3e50d548b0b24f5187eb09613c1b2e02.png)

5. 针对端口的防火墙设置  
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/46d0312a01b34ebf86f0d803a54e82f0.png)

例1：想连接到本机的udp port 137,138 tcp port 139,445就放行

![在这里插入图片描述](https://img-blog.csdnimg.cn/2e0be4d0b724465a9bf0aa995ee46375.png)

6. 对mac与state的防火墙设置  
    ![在这里插入图片描述](https://img-blog.csdnimg.cn/ab5c22f7302047f7a228c7bf4a0ca46e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_15,color_FFFFFF,t_70,g_se,x_16)

例1：只要已建立或相关封包就予以通过，只要是不合法封包就丢弃  
![在这里插入图片描述](https://img-blog.csdnimg.cn/c8b5d25004d94eeaa160f3196a4c399e.png)

例2：针对局域网络内的 aa:bb:cc:dd:ee:ff 主机放行  
![在这里插入图片描述](https://img-blog.csdnimg.cn/80f6c03ed2494131855d0c7fc669331c.png)

### 6.5设置单机防火墙实例

超简单的客户端防火墙（实例）  
1、规则归零：清除所有已经存在的规则  
2、默认策略：除了 INPUT 这个自定义链设为 DROP 外，其他为预设 ACCEPT  
3、信任本机：由于 lo 对本机来说是相当重要的，因此 lo 必须设定为信任装置  
4、回应数据包：让本机主动向外要求而响应的封包可以进入本机 (ESTABLISHED,RELATED)  
5、信任用户：这是非必要的，如果你想要让区网的来源可用你的主机资源时

## 第七章：原始套接字(自己组底层的数据包 收一帧完整的数据包)

前面TCP、UDP通信我们只组了应用层的包，直接调用sendto send write等函数发送，调用recvfrom、recv、read接收。三次握手四次挥手以及底层数据报文这些接口都帮我们实现了。

下面我们要自己去写，比如写一个路由器。

### 7.1 TCP、UDP开发回顾

数据报式套接字（SOCK_DGRAM）  
1、无连接的socket,针对无连接的 UDP 服务  
2、可通过邮件模型来进行对比

![在这里插入图片描述](https://img-blog.csdnimg.cn/dd565cbd85834b5a8789db8c9116ea05.png)

流式套接字（SOCK_STREAM）  
1、面向连接的socket,针对面向连接的 TCP 服务  
2、可通过电话模型来进行对比

这两类套接字似乎涵盖了TCP/IP 应用的全部

TCP与UDP各自有独立的port互不影响  
一个进程可同时拥有多个port  
不必关心tcp/ip协议实现的过程  
![在这里插入图片描述](https://img-blog.csdnimg.cn/a937643fca4c4d54a5471892126d1461.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_13,color_FFFFFF,t_70,g_se,x_16)

#### 7.1.1 UDP 编程回顾

client  
1、创建socket接口  
2、定义sockaddr_in变量，其中ip、port为目的主机的信息  
3、可发送0长度的数据包  
server  
1、bind本地主机的ip、port等信息  
2、接收到的数据包中包含来源主机的ip、port信息

![在这里插入图片描述](https://img-blog.csdnimg.cn/18b0a0552e154719994f6e62d3184971.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_12,color_FFFFFF,t_70,g_se,x_16)

#### 7.1.2 TCP 编程回顾

client  
connect来建立连接  
write、read收发数据  
用户不可发送0长度的数据，调用close底层自动发的。

server  
bind本地主机的ip、port等信息  
listen把主动套接字变为被动  
accept会有新的返回值  
多进程、线程完成并发  
![在这里插入图片描述](https://img-blog.csdnimg.cn/e8c429b06cfe422b93188d395b4ca909.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_12,color_FFFFFF,t_70,g_se,x_16)

想一想？  
1、能否截获网络中的数据？  
2、怎样发送一个自定义的IP包？  
3、怎样伪装本地的IP、MAC？  
4、网络攻击是怎么回事？  
5、路由器、交换机怎样实现？

方向：  
TCP/IP协议栈  
原始套接字  
原始套接字网络开发工具包libpcap(组包)/libnet(拆包)，libevent是应用层套接字网络开发库。  
书籍：  
《TCP/IP详解 卷一》★  
《UNIX网络编程 卷一》第三版★

### 7.2原始套接字概述、创建

#### 7.2.1原始套接字概述

原始套接字（SOCK_RAW）  
1、一种不同于SOCK_STREAM(流式)、SOCK_DGRAM(报式)的套接字，它实现于系统核心  
2、可以接收本机网卡上所有的数据帧（数据包）,对于监听网络流量和分析网络数据很有作用  
3、开发人员可发送自己组装的数据包到网络上  
4、广泛应用于高级网络编程  
5、网络专家、黑客通常会用此来编写奇特的网络程序

**流式套接字只能收发**  
TCP协议的数据  
**数据报套接字只能收发**  
UDP协议的数据  
**原始套接字可以收发**  
1、内核没有处理的数据包，因此要访问其他协议  
2、发送的数据需要使用，原始套接字(SOCK_RAW)

![在这里插入图片描述](https://img-blog.csdnimg.cn/14b30868e3e74143a27ede8aceafd9fd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_9,color_FFFFFF,t_70,g_se,x_16)

#### 7.2.2创建原始套接字

```c
int socket(PF_PACKET, SOCK_RAW, protocol)
功能：
	创建链路层的原始套接字
参数：
	protocol：指定可以接收或发送的数据包类型
		ETH_P_IP:IPV4数据包
		ETH_P_ARP:ARP数据包
		ETH_P_ALL:任何协议类型的数据包
返回值：
	成功(>0):链路层套接字
	失败(<0):出错
```

#### 7.2.3创建链路层的原始套接字

```c
sock_raw_fd = socket(PF_PACKET,SOCK_RAW,htons(ETH_P_ALL));
sock_raw_fd = socket(AF_INET, SOCK_PACKET, htons(ETH_P_ALL)); //已过时，不再使用
头文件：
#include <sys/socket.h>
#include <netinet/ether.h>
```

### 7.3数据包详解

使用原始套接字进行编程开发时,首先要对不同协议的数据包进行学习,需要手动对IP、TCP、UDP、ICMP等包头进行组装或者拆解。  
ubuntu12.04中描述网络协议结构的文件如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/4ee656369ea6455bad915d28bf724688.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/d157ddf8db8640878b29f84b50128ec1.png)

在TCP/IP协议栈中的每一层为了能够正确解析出上层的数据包，从而使用一些“协议类型”来标记，详细如下图

![在这里插入图片描述](https://img-blog.csdnimg.cn/806451644a0f459db71a56302b7ac044.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_12,color_FFFFFF,t_70,g_se,x_16)

#### 7.3.1组装/拆解udp数据包流程

![在这里插入图片描述](https://img-blog.csdnimg.cn/48ffd13f7aff460990c18b58a57ee415.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_18,color_FFFFFF,t_70,g_se,x_16)

#### 7.3.2 UDP封包格式

![在这里插入图片描述](https://img-blog.csdnimg.cn/40430046853545f482458c1cf153a08d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_18,color_FFFFFF,t_70,g_se,x_16)

#### 7.3.3 IP封包格式

![在这里插入图片描述](https://img-blog.csdnimg.cn/69b9edaf7cb44ce4922fdbf7902f35e1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_19,color_FFFFFF,t_70,g_se,x_16)

#### 7.3.4 Ethernet封包格式

![在这里插入图片描述](https://img-blog.csdnimg.cn/2a572bd310ad4a50ad64159a8ba13a11.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_15,color_FFFFFF,t_70,g_se,x_16)

> 此图上半部分是无线网卡封装格式，下半部分是以太网数据封装格式。我们自己组的话直接组下面的以太网格式，无线网卡会帮我们自动转化的。

#### 7.3.5 TCP封包格式

![在这里插入图片描述](https://img-blog.csdnimg.cn/4ea87a4710c04aa9aab2d8ea48752fbe.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_17,color_FFFFFF,t_70,g_se,x_16)

#### 7.3.6 ICMP封包格式(ping命令)

![在这里插入图片描述](https://img-blog.csdnimg.cn/c7752b1eea3d489eb861f3f3ac73f68f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_17,color_FFFFFF,t_70,g_se,x_16)

ICMP回显请求和回显应答格式  
注意：  
不同的类型值以及代码值，代表不同的功能

### 7.4编程实例—分析MAC数据包

#### 7.4.1链路层数据格式

![在这里插入图片描述](https://img-blog.csdnimg.cn/1698ba1da9b349cf85dfebba5950c145.png)

#### 7.4.2示例效果

![在这里插入图片描述](https://img-blog.csdnimg.cn/7f3eb6c7a7b44f0e9cb8cba8688955b9.png)

#### 7.4.3参考代码

analysis_mac.c  
![在这里插入图片描述](https://img-blog.csdnimg.cn/8eac82a811e940eb829e8556f1c30f9d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_18,color_FFFFFF,t_70,g_se,x_16)

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <sys/socket.h>
#include <string.h>
#include <netinet/in.h>
#include <netinet/ether.h>
int main(int argc, char *argv[])
{
    int fd = socket(PF_PACKET, SOCK_RAW, htons(ETH_P_ALL));
    if (fd < 0)
        perror("");
    unsigned char buf[1500] = "";
    unsigned char src_mac[18] = "";
    unsigned char dst_mac[18] = "";
    unsigned char dst_ip[16] = "";
    unsigned char src_ip[16] = "";

    while (1)
    {
        bzero(buf, sizeof(buf));
        bzero(src_mac, sizeof(src_mac));
        bzero(dst_mac, sizeof(dst_mac));
        recvfrom(fd, buf, sizeof(buf), 0, NULL, NULL);
        // buf
        sprintf(dst_mac, "%x:%x:%x:%x:%x:%x", buf[0], buf[1], buf[2], buf[3], buf[4], buf[5]);
        sprintf(src_mac, "%x:%x:%x:%x:%x:%x", buf[6], buf[7], buf[8], buf[9], buf[10], buf[11]);
        printf("src_mac=%s --> dst_mac=%s\n", src_mac, dst_mac);
        
        if (buf[12] == 0x08 && buf[13] == 0x00)
        {
            printf("IP\n");
            sprintf(src_ip, "%d.%d.%d.%d", buf[26], buf[27], buf[28], buf[29]);
            sprintf(dst_ip, "%d.%d.%d.%d", buf[30], buf[31], buf[32], buf[33]);
            printf("src_ip=%s --> dst_ip=%s\n", src_ip, dst_ip);
            if (buf[23] == 6)
            {
                printf("TCP\n");
                printf("src_port=%d\n", ntohs(*(unsigned short *)(buf + 34)));
                printf("dst_port=%d\n", ntohs(*(unsigned short *)(buf + 36)));
            }
            else if (buf[23] == 17)
            {
                printf("UDP\n");
            }
        }
        else if (buf[12] == 0x08 && buf[13] == 0x06)
        {
            printf("ARP\n");
        }
        else if (buf[12] == 0x80 && buf[13] == 0x35)
        {
            printf("RARP\n");
        }
    }

    return 0;
}
```

> 上面代码运行时要加sudo权限，因为要操作内核层。  
> 视频中运行会循环打印，因为开启了ssh服务。

### 7.5练习—网络数据分析器

在很多时候需要对网络上的数据进行抓取，然后进行分析，此“网络数据分析器”就是模仿现实开发中的抓包工具而进行的。  
运行demo现象如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/5ea1150f50d44c11a778c98971644c71.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_16,color_FFFFFF,t_70,g_se,x_16)

#### 7.5.1网络数据分析图

![在这里插入图片描述](https://img-blog.csdnimg.cn/74b48063119740379986a49644e62f89.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_10,color_FFFFFF,t_70,g_se,x_16)

#### 7.5.2 ARP数据解析图

![在这里插入图片描述](https://img-blog.csdnimg.cn/84f2e8abdfcf4cea8e4e683a51e47903.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_12,color_FFFFFF,t_70,g_se,x_16)

说明:  
1、ARP的TYPE为0x0806  
2、buf为unsinged char  
3、所有数据均为大端

#### 7.5.3 IP数据解析图

![在这里插入图片描述](https://img-blog.csdnimg.cn/496434f769484dcc9e2c8f33b23bf6cf.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_11,color_FFFFFF,t_70,g_se,x_16)

说明：  
1、IP的TYPE为0x0800  
2、buf为unsinged char  
3、所有数据均为大端

如下图所示，是网上的数据包的组包过程；其解包过程正好相反，首先分析以太网得到MAC然后再依次分析，比如IP、PORT等等

![在这里插入图片描述](https://img-blog.csdnimg.cn/a5a6401bff2f43809372c7b10709c4ce.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_12,color_FFFFFF,t_70,g_se,x_16)

#### 7.5.4要求

要求：  
1、分析出ARP/IP/RARP  
2、分析出MAC  
扩展：  
在完成基本要求的前提下，分析PORT  
提示：  
以root权限运行  
想一想：  
如何捕捉途经网卡的数据?  
06-01-data-analysis.c

#### 7.5.5混杂模式

混杂模式  
1、指一台机器的网卡能够接收所有经过它的数据包(目的MAC并不是你自己)，而不论其目的地址是否是它。  
2、一般计算机网卡都工作在非混杂模式下，如果设置网卡为混杂模式需要root权限

linux下设置  
1、设置混杂模式：ifconfig eth0 promisc  
2、取消混杂模式：ifconfig eth0 -promisc  
windos下通过特定的软件实现

linux下通过程序设置网卡混杂模式：

![在这里插入图片描述](https://img-blog.csdnimg.cn/285a6de0143e4e5b9488759cab2559d7.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_19,color_FFFFFF,t_70,g_se,x_16)

> ifreq结构体里还记录了网卡的IP地址、MAC地址等信息，可以通过接口获取。

### 7.6 sendto发送数据

#### 7.6.1使用sendto发送原始套接字数据

sendto(sock_raw_fd, msg, msg_len, 0,(struct sockaddr*)&sll, sizeof(sll));  
注意：  
1、sock_raw_fd：原始套接字  
2、msg:发送的消息（封装好的协议数据）  
3、sll:本机网络接口，指发送的数据应该从本机的哪个网卡出去，而不是以前的目的地址，在7.6.2和7.6.4里介绍。  
想一想：  
如何定义sll?

#### 7.6.2本机网络接口

```cpp
struct sockaddr_ll sll;  
#include <netpacket/packet.h>  
struct sockaddr_ll  
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/ba72c20b299a40e8bccfa1a8b761f875.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_16,color_FFFFFF,t_70,g_se,x_16)

只需要对sll.sll_ifindex赋值，就可使用

#### 7.6.3发送数据demo

![在这里插入图片描述](https://img-blog.csdnimg.cn/93686697e1414d5bb009153cadc2943a.png)

#### 7.6.4通过ioctl来获取网络接口地址

```cpp
int ioctl(int fd, int request,void *)  
#include <sys/ioctl.h>  
```
ioctl获取接口示例

![在这里插入图片描述](https://img-blog.csdnimg.cn/19247e358d3d4c379e1c53228af093ae.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_17,color_FFFFFF,t_70,g_se,x_16)

想一想：  
ioctl的参数、struct ifreq结构类型

ioctl参数对照表  
![在这里插入图片描述](https://img-blog.csdnimg.cn/8c970c6ae48e41b4b6a1f24b89054379.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/ab9f5f404a724906afa047c592e6b4d4.png)

struct ifreq：#include <net/if.h>  
IFNAMSIZ 16  
sendto发送数据的整体过程

![在这里插入图片描述](https://img-blog.csdnimg.cn/6d1c75f2bc7c473b8022217bbe850bd0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_18,color_FFFFFF,t_70,g_se,x_16)

### 7.7MAC地址扫描器（ARP）

AC地址扫描器：把同一网段所有主机MAC地址都获取到。

#### 7.7.1 ARP概述

想一想：  
如果A(192.168.1.1)向B(192.168.1.2)发送一个数据包，那么需要的条件有ip、port、使用的协议（TCP/UDP）之外还需要MAC地址，因为在以太网数据包中MAC地址是必须要有的；问怎样才能知道对方的MAC地址？使用什么协议呢？

ARP（Address Resolution Protocol，地址解析协议）  
1、是TCP/IP协议族中的一个  
2、主要用于查询指定ip所对应的的MAC  
3、请求方使用广播来发送请求  
4、应答方使用单播来回送数据  
5、为了在发送数据的时候提高效率在计算中会有一个**ARP缓存表**，用来暂时存放ip所对应的MAC，在linux中使用ARP即可查看,在xp中使用ARP -a。  
主机3~5分钟会更新这个表，防止中途变化。我调试的时候就会修改网卡的IP地址。

#### 7.7.2在linux与xp系统下查看ARP的方式

![在这里插入图片描述](https://img-blog.csdnimg.cn/80544ad4d071430e8111b4a2a8575a5d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_13,color_FFFFFF,t_70,g_se,x_16)

以机器A获取机器B的MAC为例，发送ARP请求报文，报文中MAC地址全写0xFF，目的IP写上需要请求那一台主机的IP。  
![在这里插入图片描述](https://img-blog.csdnimg.cn/c714f883800d430f9ac55ca6dc9864ee.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_15,color_FFFFFF,t_70,g_se,x_16)

#### 7.7.3 ARP协议格式

![在这里插入图片描述](https://img-blog.csdnimg.cn/63daccfc4c0d4bc3a6b1ea4dd580e82a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_15,color_FFFFFF,t_70,g_se,x_16)

#### 7.7.4向指定IP发送ARP请求(demo)

获取172.20.226.11的MAC地址

![在这里插入图片描述](https://img-blog.csdnimg.cn/1ef9c8102666416aa53359c7a8273b2b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

![在这里插入图片描述](https://img-blog.csdnimg.cn/f1c15692eaa0446096590b51823b4adf.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <net/if.h>    //struct ifreq
#include <sys/ioctl.h> //ioctl、SIOCGIFADDR
#include <sys/socket.h>
#include <netinet/ether.h>    //ETH_P_ALL
#include <netpacket/packet.h> //struct sockaddr_ll
#include <pthread.h>
#include <netinet/in.h>
#include <signal.h>
#include <unistd.h>

static int sockfd;

static void sig_dispose(int sig)
{
    if (SIGINT == sig)
    {
        close(sockfd);
        puts("\nclose!");
        exit(0);
    }
}

void *send_arp_ask(void *arg)
{
    int i = 0;
    int sockfd = *(int *)arg;
    // 1.根据各种协议首部格式构建发送数据报
    unsigned char send_msg[1024] = {
        //--------------组MAC--------14------
        0xff, 0xff, 0xff, 0xff, 0xff, 0xff, // dst_mac: FF:FF:FF:FF:FF:FF
        0x00, 0x53, 0x50, 0x00, 0x7f, 0xa5, // src_mac: 00:0c:29:75:a6:51
        0x08, 0x06,                         // 类型：0x0806 ARP协议

        //--------------组ARP--------28-----
        0x00, 0x01, 0x08, 0x00,             // 硬件类型1(以太网地址),协议类型0x0800(IP)
        0x06, 0x04, 0x00, 0x01,             // 硬件、协议地址分别是6、4，op:(1：arp请求，2：arp应答)
        0x00, 0x53, 0x50, 0x00, 0x7f, 0xa5, // 发送端的MAC地址
        10, 0, 108, 127,                    // 发送端的IP地址
        0x00, 0x00, 0x00, 0x00, 0x00, 0x00, // 目的MAC地址（由于要获取对方的MAC,所以目的MAC置零）
        10, 0, 108, 213                     // 目的IP地址
    };

    // 2.数据初始化
    struct sockaddr_ll sll;                     // 原始套接字地址结构
    struct ifreq ethreq;                        // 网络接口地址
    strncpy(ethreq.ifr_name, "eth0", IFNAMSIZ); // 指定网卡名称

    // 3.获取指定网卡对应的接口地址索引存入ethreq.ifr_ifindex用于sendto()
    ioctl(sockfd, SIOCGIFINDEX, (char *)&ethreq);
    bzero(&sll, sizeof(sll));
    sll.sll_ifindex = ethreq.ifr_ifindex;

    // 4.获取本地机的IP
    if (!(ioctl(sockfd, SIOCGIFADDR, (char *)&ethreq)))
    {
        int num = ntohl(((struct sockaddr_in *)(&ethreq.ifr_addr))->sin_addr.s_addr);
        for (i = 0; i < 4; i++)
        {
            send_msg[31 - i] = num >> 8 * i & 0xff; // 将发送端的IP地址组包
        }
    }

    // 5.获取本地机(eth0)的MAC
    if (!(ioctl(sockfd, SIOCGIFHWADDR, (char *)&ethreq)))
    {
        for (i = 0; i < 6; i++)
        {
            // 将src_mac、发送端的MAC地址组包
            send_msg[22 + i] = send_msg[6 + i] = (unsigned char)ethreq.ifr_hwaddr.sa_data[i];
        }
    }

    while (1)
    {
        int i = 0;
        int num[4] = {0};
        unsigned char input_buf[1024] = "";

        // 6.获取将要扫描的网段（10.0.13.0）
        printf("input_dst_Network:10.0.13.0\n");
        fgets(input_buf, sizeof(input_buf), stdin);
        sscanf(input_buf, "%d.%d.%d.", &num[0], &num[1], &num[2]); // 目的IP地址

        // 7.将键盘输入的信息组包
        for (i = 0; i < 4; i++)
            send_msg[38 + i] = num[i]; // 将目的IP地址组包

        // 8.给1~254的IP发送ARP请求
        for (i = 1; i < 255; i++)
        {
            send_msg[41] = i;
            int len = sendto(sockfd, send_msg, 42, 0, (struct sockaddr *)&sll, sizeof(sll));
            if (len == -1)
            {
                perror("sendto");
            }
        }
        sleep(1);
    }
    return NULL;
}


int main(int argc, char *argv[])
{
    pthread_t tid;
    signal(SIGINT, sig_dispose);

    // 1.创建通信用的原始套接字
    sockfd = socket(PF_PACKET, SOCK_RAW, htons(ETH_P_ALL));
    // 2.创建发送线程
    pthread_create(&tid, NULL, (void *)send_arp_ask, (void *)&sockfd);

    while (1)
    {
        // 3.接收对方的ARP应答
        unsigned char recv_buf[1024] = "";
        recvfrom(sockfd, recv_buf, sizeof(recv_buf), 0, NULL, NULL);
        if (recv_buf[21] == 2) // ARP应答
        {
            char resp_mac[18] = ""; // arp响应的MAC
            char resp_ip[16] = "";  // arp响应的IP

            sprintf(resp_mac, "%02x:%02x:%02x:%02x:%02x:%02x",
                    recv_buf[22], recv_buf[23], recv_buf[24], recv_buf[25], recv_buf[26], recv_buf[27]);
            sprintf(resp_ip, "%d.%d.%03d.%03d", recv_buf[28], recv_buf[29], recv_buf[30], recv_buf[31]);
            printf("IP:%s - MAC:%s\n", resp_ip, resp_mac);
        }
    }

    return 0;
}
```

要求  
获取到当前网段中所有机器的MAC地址  
提示  
1、每次指定一个机器发送MAC请求(交换机一次只能通信一组)，通过发送多次ARP，即可得到所有的机器的MAC  
2、ARP的发送和接收各使用一个线程

MAC地址扫描器运行现象如下图所示 06-02-sock_raw_arp.c

![在这里插入图片描述](https://img-blog.csdnimg.cn/01403463068e4809bcf86b649ed1c0bb.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_17,color_FFFFFF,t_70,g_se,x_16)

### 7.8飞鸽欺骗（UDP）

飞鸽报文格式(应用层)：

```c
版本:用户名:主机名:命令字:附加消息
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/ec146f7a95b54fe9b8dd940b99ab2289.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/bc2a1e361341410e9469289cc1bfb3a6.png)

---

目的：挑拨离间

组包过程：

![在这里插入图片描述](https://img-blog.csdnimg.cn/5c6f919079764b9cbee43a0893baed4c.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/ac84ef17c63d427ab37a5036862b78aa.png)

飞鸽消息格式：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/1ee5953e78094468a50a7a644368ee0d.png)

注意：  
msg指的是udp报文头中的数据

MAC、IP、UDP报文头参考前面的数据包详解  
1、在对UDP校验的时候需要在UDP报文之间加上伪头部  
2、IP校验的时候不需要伪头部

![在这里插入图片描述](https://img-blog.csdnimg.cn/fee4a78dcfd448368e5a49b82ee4613f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_17,color_FFFFFF,t_70,g_se,x_16)

raw_udp_fq.c

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <net/if.h>	   			//struct ifreq
#include <sys/ioctl.h> 			//ioctl、SIOCGIFADDR
#include <sys/socket.h>
#include <netinet/ether.h>	  	//ETH_P_ALL
#include <netpacket/packet.h> 	//struct sockaddr_ll

#define IPMSG_BR_ENTRY 1
#define IPMSG_BR_EXIT 2
#define IPMSG_SENDMSG 32
#define IPMSG_RECVMSG 33
// 网络头部校验算法
unsigned short checksum(unsigned short *buf, int len)
{
	int nword = len / 2;
	unsigned long sum;

	if (len % 2 == 1)
		nword++;
	for (sum = 0; nword > 0; nword--)
	{
		sum += *buf;
		buf++;
	}
	sum = (sum >> 16) + (sum & 0xffff);
	sum += (sum >> 16);
	return ~sum;
}

int main(int argc, char *argv[])
{
	// 1.创建通信用的原始套接字
	int sock_raw_fd = socket(PF_PACKET, SOCK_RAW, htons(ETH_P_ALL));

	// 2.根据各种协议首部格式构建发送数据报
	unsigned char send_msg[1024] = {
		//--------------组MAC--------14------
		0x00, 0x21, 0xcc, 0x5f, 0x7e, 0xf6, 	// dst_mac
		0x00, 0x53, 0x50, 0x00, 0x4a, 0x44, 	// src_mac
		0x08, 0x00,								// 类型：0x0800 IP协议
		//--------------组IP---------20------
		0x45, 0x00, 0x00, 0x00, 				// 版本号：4, 首部长度：20字节, TOS:0, 总长度(不知道先写0)：
		0x00, 0x00, 0x00, 0x00, 				// 16位标识、3位标志、13位片偏移都设置0
		0x80, 17, 0x00, 0x00,					// TTL：128、协议：UDP（17）、16位首部校验和(不知道先写0)
		10, 0, 31, 17,							// src_ip
		10, 0, 31, 39,							// dst_ip
		//--------------组UDP--------8+len------
		0x09, 0x79, 0x09, 0x79, 				// src_port:0x0979(2425), dst_port:0x0979(2425)
		0x00, 0x00, 0x00, 0x00, 				// 16位UDP长度(不知道先写0)30个字节  16位校验和(不知道先写0)
	};

	int len = sprintf(send_msg + 42,
					  "1:%d:%s:%s:%d:%s", /* 版本头像 */
					  123,				  /* 飞秋数据包编号 */
					  "usernam",		  /* 发送者姓名 */
					  "hostname",		  /* 发送者主机名 */
					  IPMSG_SENDMSG,	  /* 表示发送消息的命令 */
					  "hello");			  /* 发送内容 */
	if (len % 2 == 1)					  // 判断len是否为奇数
		len++;							  // 如果是奇数，len就应该加1(因为UDP的数据部分如果不为偶数需要用0填补)

	*((unsigned short *)&send_msg[16]) = htons(20 + 8 + len);	  // IP总长度 = 20 + 8 + len
	*((unsigned short *)&send_msg[14 + 20 + 4]) = htons(8 + len); // udp总长度 = 8 + len
	// 3.UDP伪头部
	unsigned char pseudo_head[1024] = {
		//------------UDP伪头部--------12--
		10, 0, 31, 17,		  // src_ip
		10, 0, 31, 39,		  // dst_ip
		0x00, 17, 0x00, 0x00, // 0,17,#--16位UDP长度--8个字节
	};

	*((unsigned short *)&pseudo_head[10]) = htons(8 + len); // 伪头部中的udp长度（和真实udp长度是同一个值）
	// 4.构建udp校验和需要的数据报 = udp伪头部 + udp数据报
	memcpy(pseudo_head + 12, send_msg + 34, 8 + len); //--计算udp校验和时需要加上伪头部--
	// 5.对IP首部进行校验
	*((unsigned short *)&send_msg[24]) = checksum((unsigned short *)(send_msg + 14), 20);
	// 6.--对UDP数据进行校验--
	*((unsigned short *)&send_msg[40]) = checksum((unsigned short *)pseudo_head, 12 + 8 + len);

	// 6.发送数据
	struct sockaddr_ll sll; // 原始套接字地址结构
	struct ifreq ethreq;	// 网络接口地址

	strncpy(ethreq.ifr_name, "eth0", IFNAMSIZ);			 // 指定网卡名称
	if (-1 == ioctl(sock_raw_fd, SIOCGIFINDEX, &ethreq)) // 获取网络接口
	{
		perror("ioctl");
		close(sock_raw_fd);
		exit(-1);
	}

	/*将网络接口赋值给原始套接字地址结构*/
	bzero(&sll, sizeof(sll));
	sll.sll_ifindex = ethreq.ifr_ifindex;
	len = sendto(sock_raw_fd, send_msg, 14 + 20 + 8 + len, 0, (struct sockaddr *)&sll, sizeof(sll));
	if (len == -1)
	{
		perror("sendto");
	}
	close(sock_raw_fd);
	return 0;
}
```

### 7.9练习—三次握手连接器（TCP报头 SYN数据包）

TCP数据包（发送一个SYN数据包）

![在这里插入图片描述](https://img-blog.csdnimg.cn/83798ea5c6894befa39ec38164163171.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_14,color_FFFFFF,t_70,g_se,x_16)

发送一个SYN数据包  
![在这里插入图片描述](https://img-blog.csdnimg.cn/bf81b4e892a8426b82d6f6a63411b558.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/6edc912a6bd74069b1ab1c4aac1cfc77.png)

06-04-sock_raw_tcp_syn.c

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <net/if.h> //struct ifreq
#include <sys/socket.h>
#include <netinet/ether.h>	  //ETH_P_ALL
#include <netpacket/packet.h> //struct sockaddr_ll
#include <sys/ioctl.h>		  //ioctl、SIOCGIFADDR
#include <arpa/inet.h>
#include <unistd.h>

unsigned short checksum(unsigned short *buf, int len)
{
	int nword = len / 2;
	unsigned long sum;

	if (len % 2 == 1)
		nword++;
	for (sum = 0; nword > 0; nword--)
	{
		sum += *buf;
		buf++;
	}
	sum = (sum >> 16) + (sum & 0xffff);
	sum += (sum >> 16);
	return ~sum;
}
/*
	1、首先通过NetAssist建立TCP服务器连接
	2、打开wireshark抓取tcp.port == 8000端口数据
	3、在相同局域网开发板或虚拟机编译运行该程序
	3、抓取后停止观察现象
*/
int main(int argc, char *argv[])
{
	// 1.创建通信用的原始套接字
	int sockfd = socket(PF_PACKET, SOCK_RAW, htons(ETH_P_ALL));

	// 2.根据各种协议首部格式构建发送数据报
	unsigned char send_msg[1024] = {
		//--------------组MAC--------14------
		0x00, 0x21, 0xcc, 0x5f, 0x7e, 0xf6, // dst_mac: c8:9c:dc:b7:0f:19
		0x00, 0x53, 0x50, 0x00, 0x13, 0x10, // src_mac: 00:0c:29:75:a6:51
		0x08, 0x00,							// 类型：0x0800 IP协议
		//--------------组IP---------20------
		0x45, 0x00, 0x00, 40,	// 版本号：4, 首部长度：20字节, TOS:0, --总长度--：
		0x00, 0x00, 0x00, 0x00, // 16位标识、3位标志、13位片偏移都设置0
		0x80, 6, 0x00, 0x00,	// TTL：128、协议：TCP（6）、--16位首部校验和--
		10, 0, 31, 17,			// src_ip: 10,  0,   13,  252
		10, 0, 31, 39,			// dst_ip: 10,  0,   31,  39
		//--------------组TCP--------20------
		0x1f, 0x40, 0x1f, 0x40, // src_port:0x1f40(8000), dst_port:0x1f40(8000)
		0x00, 0x00, 0x00, 0x01, // 序号
		0x00, 0x00, 0x00, 0x00, // 确认序号
		0x50, 0x02, 0x17, 0x70, // 数据偏（首部长度）移在4位为5*4=20，
								// 保留位占6位，0x02就是将SYN位置1、窗口尺寸0x1770(6000)
		0x00, 0x00, 0x00, 0x00 // 校验和（TCP伪首部12B+TCP首部20B+TCP数据部分0B）(靠左2B)

	};

	// 3.TCP伪头部
	unsigned char pseudo_head[1024] = {
		//------------TCP伪头部-------12--
		10, 0, 31, 17,	   // src_ip: 10,  0,   13,  252
		10, 0, 31, 39,	   // dst_ip: 10,  0,   31,  39
		0x00, 6, 0x00, 20, // 0,6（TCP）,#16位TCP长度20个字节
	};

	// 4.构建tcp校验和需要的数据报 = tcp伪头部 + tcp数据报
	memcpy(pseudo_head + 12, send_msg + 34, 20); // 计算udp校验和时需要加上伪头部

	// 5.对tcp数据进行校验
	*((unsigned short *)&send_msg[50]) = checksum((unsigned short *)pseudo_head, 32);
	*((unsigned short *)&send_msg[24]) = checksum((unsigned short *)(send_msg + 14), 20);

	// 6.发送数据
	struct sockaddr_ll sll; // 原始套接字地址结构
	struct ifreq ethreq;	// 网络接口地址

	strncpy(ethreq.ifr_name, "eth0", IFNAMSIZ);		// 指定网卡名称
	if (-1 == ioctl(sockfd, SIOCGIFINDEX, &ethreq)) // 获取网络接口
	{
		perror("ioctl");
		close(sockfd);
		exit(-1);
	}

	/*将网络接口赋值给原始套接字地址结构*/
	bzero(&sll, sizeof(sll));
	sll.sll_ifindex = ethreq.ifr_ifindex;
	int len = sendto(sockfd, send_msg, 54, 0, (struct sockaddr *)&sll, sizeof(sll));
	if (len == -1)
	{
		perror("sendto");
	}
	return 0;
}
```

## 第八章：网络开发工具包库-libpcap、libnet(方便组包、拆包等)

### 8.1 socket原始套接字回顾

原始套接字  
1、开发者可发送任意的数据包到网上  
2、开发网络攻击等特殊软件  
3、需要开发者手动组织数据、各种协议包头、校验和计算等等  
创建方法:  
![在这里插入图片描述](https://img-blog.csdnimg.cn/7cb29cc57c024bdf87b5c75d69ea201e.png)

### 8.2 libpcap介绍、安装

#### 8.2.1 Libpcap概念

1、是一个网络数据捕获开发包  
2、平台独立具有强大功能  
3、是一套高层的编程接口的集合；其隐藏了操作系统的细节，可以捕获网上的所有,包括到达其他主机的数据包  
4、使用非常广泛，几乎只要涉及到网络数据包的捕获的功能，都可以用它开发，==如wireshark==  
5、开发语言为C语言,但是也有基于其它语言的开发包,如Python语言的开发包pycap

#### 8.2.2 Libpcap主要的作用

Libpcap主要的作用如下：  
1、捕获各种数据包  
列如：网络流量统计  
2、过滤网络数据包  
列如：过滤掉本地上的一些数据，类似防火墙  
3、分析网络数据包  
列如：分析网络协议，数据的采集  
4、存储网络数据包  
列如：保存捕获的数据以为将来进行分析

#### 8.2.3 Libpcap的安装：

apt-get方式：

![在这里插入图片描述](https://img-blog.csdnimg.cn/aa47a87aeae8460a865e267e1b76d49a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_17,color_FFFFFF,t_70,g_se,x_16)

### 8.3 libpcap开发实例

利用libpcap函数库开发应用程序的基本步骤：  
1、打开网络设备  
2、设置过滤规则  
3、捕获数据  
4、关闭网络设备

捕获网络数据包常用函数  
1、pcap_lookupdev( )  
2、pcap_open_live( )  
3、pcap_lookupnet( )  
4、pcap_compile( )、 pcap_setfilter( )  
5、pcap_next( )、pcap_loop( )  
6、pcap_close( )

```c
char *pcap_lookupdev(char *errbuf)
功能：
得到可用的网络设备名(网卡名称)指针
参数:
 errbuf: 存放相关的错误信息
返回值：
成功返回设备名指针；失败返回NULL
```

例如：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/aa43d07b11714e629c0cc6a706b9e711.png)

```c
pcap_t *pcap_open_live(const char *device,
						int snaplen,
						int promisc,
						int to_ms,
						char *ebuf)
功能：
打开一个用于捕获数据的网络接口(打开这个网卡，用于捕获数据包抓包)
返回值：
返回一个Libpcap句柄

参数：
device：网络接口的名字
snaplen：捕获数据包的长度
promise：1代表混杂模式,其它非混杂模式
to_ms：等待时间
ebuf：存储错误信息
```

例如  
![在这里插入图片描述](https://img-blog.csdnimg.cn/477b535d998640c28f811282e20001af.png)

```c
int pcap_lookupnet(char *device,bpf_u_int32 *netp, 
bpf_u_int32 *maskp,
char *errbuf)
功能：
获得指定网络设备的网段地址和子网掩码
参数：
device：网络设备名
netp：存放网络号的指针
maskp：存放掩码的指针
errbuf：存放出错信息
返回值：
成功返回0，失败返回-1
```

例如：

![在这里插入图片描述](https://img-blog.csdnimg.cn/62989e81ebff43b1b178782ecbecde29.png)

```c
int pcap_compile(pcap_t *p,
				struct bpf_program *program,
				char *buf,int optimize,
				bpf_u_int32 mask)
功能：
编译BPF过滤规则
返回值：
成功返回0，失败返回-1

参数:
p：Libpcap句柄
program：bpf过滤规则
buf：过滤规则字符串
optimize：优化
mask：掩码
```

例如:

![在这里插入图片描述](https://img-blog.csdnimg.cn/d0eaafa4445b44959abbd97460588be3.png)

```c
int pcap_setfilter(pcap *p,
					struct bpf_program*fp)
功能：
设置BPF过滤规则
返回值：
成功返回0，失败返回-1
参数:
p：Libpcap句柄
fp：BPF过滤规则
```

例如：

![在这里插入图片描述](https://img-blog.csdnimg.cn/3a9ae18d6a244fec894b9f6a3a3bcd90.png)

```c
const u_char *pcap_next(pcap_t *p,
					struct pcap_pkthdr *h)
功能：
捕获[一次]网络数据包
参数：
p：Libpcap句柄
h：数据包头
返回值：
捕获的数据包的地址
```

例如：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/7eb35c95330a4a01b13cdd3165774daf.png)

```c
int pcap_loop(pcap_t *p,int cnt,
				pcap_handler callback,
				u_char *user)
功能:
[循环]捕获网络数据包，直到遇到错误或者满足退出条件；每次捕获一个数据包就会调用callback指示的回调函数，所以，可以在回调函数中进行数据包的处理操作
返回值：
成功返回0，失败返回负数
参数:
p：Libpcap句柄
cnt：指定捕获数据包的个数，如果是-1，就
     会永无休止的捕获
callback：回调函数
user：向回调函数中传递的参数
```

例如：

![在这里插入图片描述](https://img-blog.csdnimg.cn/d034bfe197bb43a8aac111af2222cbb2.png)

```c
void pcap_close(pcap_t *p)
功能：
关闭Libpcap操作，并销毁相应的资源
参数
   p:需要关闭的Libpcap句柄
返回值：
无
```

例如  
![在这里插入图片描述](https://img-blog.csdnimg.cn/769a47edebfa4976b8de4bfafb376056.png)

捕获第一个网络数据包（01-Libpcap-demo）  
1、gcc编译时需要加上-lpcap  
2、运行时需要使用超级权限

![在这里插入图片描述](https://img-blog.csdnimg.cn/9c2e79ca408d41e4a82acc82e058e981.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_17,color_FFFFFF,t_70,g_se,x_16)

捕获指定类型数据包,如ARP（02-Libpcap-demo）

![在这里插入图片描述](https://img-blog.csdnimg.cn/af548a017af443439c1bea572fde7ca2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_17,color_FFFFFF,t_70,g_se,x_16)

注意：为了更具有说明请多次执行此程序

捕获多个网络数据包（03-Libpcap-demo）

![在这里插入图片描述](https://img-blog.csdnimg.cn/b3af2972683e4189a9e349f4ba7f19dc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_17,color_FFFFFF,t_70,g_se,x_16)

### 8.4 libpcap练习——网络数据分析器

![在这里插入图片描述](https://img-blog.csdnimg.cn/281d3af436234f55abe58436d6b8808d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_10,color_FFFFFF,t_70,g_se,x_16)

项目要求  
判断数据包的类型ARP/RARP/IP/TCP/UDP  
分析  
MAC(src、dst)  
IP(src、dst)  
PORT(src、dst)  
分析出的UDP/TCP应用程序的通信数据

### 8.5 Libnet介绍、安装(构造和发送网络数据包)

#### 8.5.1 Libnet概念

专业的构造和发送网络数据包的开发工具包  
是个高层次的API函数库，允许开发者自己构造和发送网络数据包

#### 8.5.2 Libnet特点

Libnet特点  
1、隐藏了很多底层细节，省去了很多麻烦；如缓冲区管理、字节流顺序、校验（不用自己计算校验了）和计算等问题，使开发者把重心放到程序的开发中  
2、可以轻松、快捷的构造任何形式的网络数据包，从而开发各种各样的网络程序  
3、使用非常广泛，例如著名的软件Ettercap、Firewalk、Snort、Tcpreplay等  
4、在1998年就出现了，但那时还有很多缺陷，比如计算校验和非常繁琐等；从2001年开始Libnet的作者Mike Schiffman对其进行了完善，而且功能更加强大。至此，可以说Libnet开发包已经非常完美了，使用Libnet开发包的人越来越多

Libnet的安装

下载完直接使用，不用额外编译安装。

![在这里插入图片描述](https://img-blog.csdnimg.cn/f3cebeb07808458db4a1440031e85131.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_17,color_FFFFFF,t_70,g_se,x_16)

### 8.6 Libnet开发实例

#### 8.6.1 Libnet开发流程

利用libnet函数库开发应用程序的基本步骤：  
1、数据包内存初始化  
2、构造数据包  
3、发送数据  
4、释放资源

libnet库主要功能：  
1、内存管理  
2、地址解析  
3、包处理

![在这里插入图片描述](https://img-blog.csdnimg.cn/0c8343d71dc04cf28ae804018834efcf.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_9,color_FFFFFF,t_70,g_se,x_16)

内存管理相关函数：

```c
libnet_t *libnet_init(int injection_type, char *device, char *err_buf)
功能：
数据包内存初始化及环境建立
参数：							链路层		网络层(mac层不用自己组包)		高级链路层
	injection_type: 构造的类型(LIBNET_LINK, LIBNET_RAW4,    				LIBNET_LINK_ADV, LIBNET_RAW4_ADV)
	device：网络接口，如"eth0",或IP地址，亦可为NULL(自动查询搜索)
	err_buf: 存放出错的信息
返回值：
成功返回一个libnet句柄；失败返回NULL
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2cddcb140c904bf8a8d783ee8f90f352.png)

内存管理相关函数：

```c
  void libnet_destroy(libnet_t *l);
功能：
释放资源
参数：
  l: libnet句柄
返回值：
无
```

例如：  
![在这里插入图片描述](https://img-blog.csdnimg.cn/38f314d09c8341a9ae3655e6d4fe263c.png)

```c
libnet_ptag_t libnet_build_udp(
		u_int16_t sp,u_int16_t dp,
		u_int16_t len,u_int16_t sum,
		u_int8_t *payload,u_int32_t payload_s,
		libnet_t *l,libnet_ptag_t ptag)
功能：
构造udp数据包
返回值：
成功返回协议标记；失败返回-1
参数：
sp: 源端口号
dp：目的端口号
len：udp包总长度
sum：校验和，设为0，libnet自动填充
payload：负载，可设置为NULL
payload_s：负载长度，或为0
l: libnet句柄
ptag：协议标记  第一次写0，代表申请一块内存，以后再次组UDP报文时，写成功返回协议标记
```

```c
libnet_ptag_t libnet_build_ipv4(
		u_int16_t ip_len,u_int8_t tos,
		u_int16_t id,u_int16_t flag,
		u_int8_t ttl,u_int8_t prot,
		u_int16 sum,u_int32_t src,
		u_int32_t dst,u_int8_t *payload,
		u_int32_t payload_s,
		libnet_t *l,libnet_ptag_t ptag)
功能：
构造一个IPv4数据包
参数：
ip_len：ip包总长
tos：服务类型
id：ip标识
flag：片偏移
ttl：生存时间
prot：上层协议  UDP：17 TCP：6
sum：校验和，设为0，libnet自动填充
src: 源ip地址
dst：目的ip地址
payload：负载，可设置为NULL
payload_s：负载长度，或为0
l: libnet句柄
ptag：协议标记

返回值：
成功返回协议标记；失败返回-1
```

```c
libnet_ptag_t libnet_build_ethernet(
			u_int8_t *dst,u_int8_t *src,
			u_int16_t type,
			u_int8_t *payload,
			u_int32_t payload_s,
			libnet_t *l,libnet_ptag_t ptag)
功能：
构造一个以太网数据包

参数：
dst：目的mac
src：源mac
type：上层协议类型
payload：负载，即附带的数据
payload_s：负载长度
l：libnet句柄
ptag：协议标记

返回值：
成功返回协议标记；失败返回-1
```

```c
int libnet_write(libnet_t * l)
功能：
发送数据到网络
参数：
  l：libnet句柄
返回值：
失败返回-1，成功返回其他
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/c61054eb251344a7aec962c02e453129.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_20,color_FFFFFF,t_70,g_se,x_16)

代码中的问题:  
构造udp和ip,eth头部报文时,返回值不要使用一个,因为使用一个变量接返回值,会将上一次的协议标记覆盖,那么以后找不到  
构造ip报文,ttl不能写10,应该写64或128

更多处理函数请参见附录  
如：  
libnet_build_tcp( );  
libnet_build_tcp_options( );  
libnet_build_ipv4_options( );  
libnet_build_arp( );

### 8.7 libnet练习——ARP欺骗(窜改ARP表)

#### 8.7.1项目分析：

局域网中的网络流通是按照MAC地址进行传输的，可以通过欺骗通信双方的MAC地址，即可达到干扰通信的目的  
![在这里插入图片描述](https://img-blog.csdnimg.cn/fb5123b41faa462b8bc6e0516ecc81ed.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6KGM56iz5pa56IO96LWw6L-c,size_14,color_FFFFFF,t_70,g_se,x_16)

#### 8.7.2项目要求

1、用户可选择要欺骗的ip  
2、使用ARP方式来进行欺骗  
3、被欺骗方不能正常通信

#### 8.7.3项目步骤

1、发送ARP请求  
2、获得ARP表(并存储)  
3、选择欺骗目标ip  
4、根据目标ip找到其MAC  
5、使用库函数构造ARP回应包  
6、发送

## libevent库

前面自己写的web服务器在传输大文件时有问题(缓冲区的问题),优化的话比较复杂,可以使用现成的三方库解决。

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/cb4e192ed1a943ae9bacc991388f460b.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/6955e9078a56430bbc6cc5807e83cdf3.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/4eb3d380b3614d01832e36adfc80fb13.png)

libevent_tcp_server.c

```c
#include "wrap.h"
#include <event.h>
#include <stdio.h>
#define PORT 8888

void cfdcb(evutil_socket_t fd, short events, void *arg)
{
    struct event_base *base = (struct event_base *)arg;
    char buf[1024] = "";
    int n = Read(fd, buf, sizeof(buf));
    if (n <= 0)
    {
        // event_del();
        // close(fd);
        printf("client close\n");
    }
    else
    {
        printf("%s\n", buf);
        write(fd, buf, n);
    }
}

void lfdcb(evutil_socket_t fd, short events, void *arg)
{

    struct event_base *base = (struct event_base *)arg;
    struct sockaddr_in cliaddr;
    socklen_t len = sizeof(cliaddr);
    char ip[16] = "";
    int cfd = Accept(fd, (struct sockaddr *)&cliaddr, &len);
    printf("ip=%s port=%d\n", inet_ntop(AF_INET, &cliaddr.sin_addr.s_addr, ip, 16),
           ntohs(cliaddr.sin_port));
    struct event *new_node = event_new(base, cfd, EV_READ | EV_PERSIST, cfdcb, base);
    event_add(new_node, NULL);
}


int main(int argc, char const *argv[])
{

    int lfd = tcp4bind(PORT, NULL);
    Listen(lfd, 128);
    // 创建event_base根节点
    struct event_base *base = event_base_new();
    // 初始化(上树)
    struct event *new_node = event_new(base, lfd, EV_READ | EV_PERSIST, lfdcb, base);
    event_add(new_node, NULL);
    // 循环监听
    event_base_dispatch(base);

    event_base_free(base);
    event_free(new_node);

    return 0;
}
```

**高级的bufferevent事件**

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/61626c50b89e4f869e038a29303f3b56.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/62143637a36945eeb790ba21af985b04.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/aedf7439b0a14f8f8bdb409741da74ec.png)

  

 

显示推荐内容

[![](https://profile-avatar.csdnimg.cn/598d40612ef94982b3ffc0d198623e7d_zhuguanlin121.jpg!1)行稳方能走远](https://blog.csdn.net/zhuguanlin121)

- ![](https://csdnimg.cn/release/blogv2/dist/pc/img/newHeart2021Black.png)10
- ![](https://csdnimg.cn/release/blogv2/dist/pc/img/newUnHeart2021Black.png)
- ![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCollectBlack.png)54
- ![打赏](https://csdnimg.cn/release/blogv2/dist/pc/img/newRewardBlack.png)
- ![](https://csdnimg.cn/release/blogv2/dist/pc/img/newComment2021Black.png)0

- ![](https://csdnimg.cn/release/blogv2/dist/pc/img/newShareBlack.png)
    
    ![](https://profile-avatar.csdnimg.cn/598d40612ef94982b3ffc0d198623e7d_zhuguanlin121.jpg!1)
    
    Linux网络编程——千峰物联网笔记
    
    B站视频：千峰物联网学科linux网络编程网址：https://www.bilibili.com/video/BV1RJ411B761?p=1目录第一章：计算机网络概述1.1计算机网络发展简史1.1.1最早的广域网1.1.2电路交换网特点1.1.3计算机网络的要求1.1.4分组交换1.1.5交换方式1.1.6因特网发展史1.2 TCP/IP协议简介1.2.1分层结构1.2.2 IP协议简介1.2.3 TCP协议简介1.2.4 UDP协议简介1.3 MAC地址、IP地址、Netmask、端口1.3.1.
    
    复制链接
    
    ![](data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAEEAAABBCAYAAACO98lFAAAAAXNSR0IArs4c6QAAA0hJREFUeF7tm9GWgjAMROH/PxqPLnAgMN5JW13F7uNiazqdTCYtjsMwTEPyb5r+hozjuBu5/H/5Z3yuviaOKx3vfl+M476KSQVx+PC8aAqansd5Faju3sTx2fV0EO6M3jJB0YmQXsYRA1S6EBNoXEwfd74l7g5CLRMiA5RQqs8Rw2o1yWV2FRM6CK50b0qp2nmlKcQgYspbmODiQIv5KBBoURTsMp5yPVtN6POqOrjraeITXEF0gyXmqMW5YMU4HiAQYuTw3FynxbV6nl1PB+Eu3HbjkIDXVWXy+G6DRvNQ6B2EyAQyP1n1z2pJnN8VQBJEqmo7Jvw0CFkkKVfVoUjWP1DpzTJTdZcPJvw8CFufQMdmlC7Zvt/dSTJZpYxZ5+0ghJMl1ZWpHVNq7vqE0uqhqobrF2J8T3uHLM2UIBKIlBYKVAUimqNwSn4Kgpv7VH9pcS5oShNKwTms7+zI/adBcBefdXKtGEHxlWrM6RkjlcrLgXBmlghx1w9QzpOwlTpTEsZDFewgzD6BujfXVlPuk3/IVgH3xgvjP3OMyjTRl341CJTjtIPkF9z5s2eVlPuuwBeZJdpxt+HJLpouWqlESpu/PWOk016l9lnPTosnm03MUuksQeogzKfNpJ5Eb1fVswyg76UGTzHmwDTnZImC+XoQnr2zRI7N1ZBWWiKFTbxARlVrjauDEDRB1d1sLhNDXIZlfQCZObmOrSZ0EDYI0P1AbY67dZw0IBunde/gOsHLgeAII3lwUmFyeORT6DltCp2PWKfNlwfhTBjdRVPOlqaX2yhR1aJqsTKogyBuoCiHiCmUo+5zVUVcplBrvc7T4t4hmxYfB0LNO0t0A0R13HWOlNvucxVv1TtLlwGh5j1Gl9auj8h+jqoP+YvledV7jJcCofaM0FXhbM9QKri0OdattOomqXQSPb8ChOz9AuWwYgidE8SdJPWn+eissekvXxQNFa0peFfYaJ6XguDuGKVBaQ9A308nXOv4d/wk8NIgZDWEVJueZ80ZpeHOJ1BDQnTNlkjXLqtqozTAbewi2E1//uMG4TIo6xOKwW3xg3HyD6TOtOM0vhT8XStNJYbqvXsnSDvllliah+I5CPUrG6gsQ/4LhBtw7g0ofj1/QwAAAABJRU5ErkJggg==)
    
    扫一扫
    
    热门
    
    VIP
    

专栏目录

![](https://csdnimg.cn/release/blogv2/dist/pc/img/iconHideSide.png)![](https://csdnimg.cn/release/blogv2/dist/pc/img/iconSideBeta.png)举报![](https://g.csdnimg.cn/side-toolbar/3.4/images/fanhuidingbucopy.png)

去广告

 

自动做题

 

搜题

 

快进1.5倍

 

设置

 

更新

 

delete键隐藏