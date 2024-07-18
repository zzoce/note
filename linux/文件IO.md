# 系统调用

什么是系统调用：

由操作系统实现并提供给外部应用程序的编程接口。(Application Programming Interface，API)。是应用程序同系统之间数据交互的桥梁。

C标准函数和系统函数调用关系。一个helloworld如何打印到屏幕。

![[wps1.jpg]]
### 为什么要使用系统函数？

OS 为了安全的管理计算机软硬件资源，不允许程序员直接操作系统资源，比如（进程、内存、I/O、文件），

但是用户可以通过系统调用向 OS 请求相关资源的服务，比如：I/O 的请求和释放、设备启动、文件的创建、读写、删除、进程的创建、撤销、阻塞、唤醒

进程间的消息传递、内存的配备和回收等。

***总结：系统调用就是程序员给 OS 发送请求服务的方法或函数。***,系统函数中功能的实现与系统调用几乎一致，可看作只是换了一个封装而已

# 系统函数帮助文档
### 方法一
```bash
man 2 函数名
//例如：
man 2 open
```
![[Pasted image 20240706152759.png]]


### 方法二
在vim编写代码时，每次都返回bash来查看函数太麻烦了，可以将光标放在函数处，使用快捷键`Shift+k`来查看帮助文档

# 错误处理函数
```
error
strerror
perror
```

# 文件描述符
![[Pasted image 20240708001803.png]]

**结构体PCB进程控制块**的成员变量file_struct \*file 指向文件描述符表。从应用程序使用角度，该指针可理解记忆成一个字符指针数组，下标0/1/2/3/4...找到文件结构体。本质是一个键值对0、1、2...都分别对应具体地址。但键值对使用的特性是自动映射，我们只操作键不直接使用值。

**==新打开文件返回文件描述符表中未使用的最小文件描述符。==**
```
0 STDIN_FILENO   //标准输入
1 STDOUT_FILENO   //标准输出
2 STDERR_FILENO   // 标准错误
```

### 最大打开文件数

一个进程默认打开文件的个数1024。命令查看`ulimit -a` 查看`open files` 对应值。默认为1024  

![[Pasted image 20240708002432.png]]
     
可以使用`ulimit -n 4096` 修改
![[Pasted image 20240708002605.png]]
当然也可以通过修改系统配置文件永久修改该值，但是不建议这样操作。`cat /proc/sys/fs/file-max`可以查看该电脑最大可以打开的文件个数。受内存大小影响。

# open函数以及close

```cpp
#include <unistd.h>
#include <fcntl.h>
int open(const char *pathname, int flags);
int open(const char *pathname, int flags, mode_t mode);
int close(int fd);
```
***返回值为文件描述符***
### 常用参数
**如果使用多个参数，参数之间使用`|`隔开**
O_RDONLY：只读、O_WRONLY：只写、O_RDWR：读写、O_APPEND：追加，在文件末尾追加
###### O_CREAT：创建文件夹
如果文件不存在就创建，**==此时就需要第三个参数（一个八进制的三位数）来设置权限==**
```cpp
 fd=open ("./text1.txt",O_RDONLY|O_CREAT,0777);
//以只读模式打开，如果文件不存在就创建，文件权限为777
```
创建文件时，指定文件访问权限。权限同时受[[Linux基础命令#umask产看文件夹权限|umask]]影响。
```
文件权限 = mode & ~umask
文件权限=min（mode，文件夹权限）
```

![[Pasted image 20240706155853.png]]


######  O_EXCL：检查文件是否存在
- 如果同时指定了`O_CREAT`，则在文件存在时导致`open`调用失败。
- 作用：与`O_CREAT`配合使用，确保不会覆盖已有文件。这在避免竞态条件（race conditions）时很有用。
防止使用`O_CREAT`将已有的文件覆盖
###### O_TRUNC：截断
也就是清空文件内容
```
#include <unistd.h>
#include <iostream>
#include <fcntl.h>
using namespace std;
int main ()
{
        int fd,fd1;
        fd=open ("./test.txt",O_RDONLY);
        fd1=open ("./text1.txt",O_RDONLY|O_CREAT|O_TRUNC,0777);
        cout<<fd<<" "<<fd1<<endl;
        close(fd);
        close(fd1);

        return 0;
}
```

![[Pasted image 20240706160203.png]]
###### O_NONBLOCK：以非阻塞模式打开文件。
- 作用：操作不会因为I/O操作阻塞。这对于需要处理非阻塞I/O的应用程序（如需要处理异步I/O的程序）非常有用。


### errno
系统变量，捕获出现的错误，是一个数字代表错误类型，使用`strerror`函数找到具体的错误类型
**==以打开不存在文件为例==**
```
#include <unistd.h>
#include <iostream>
#include <fcntl.h>
#include <cstring>
#include <errno.h>
using namespace std;
int main ()
{
        int fd,fd1;
        fd=open ("./test4.txt",O_RDONLY);
        cout<<fd<<endl;
        cout<<errno<<":"<<strerror(errno)<<endl;
        close(fd);

        return 0;
}
```
![[Pasted image 20240706161009.png]]

### open常见错误 
1. 打开文件不存在
2. 以写方式打开只读文件(打开文件没有对应权限)
3. 以只写方式打开目录

# read/write函数

```
ssize_t read(int fd, void \*buf, size_t count);
ssize_t write(int fd, const void \*buf, size_t count);

//参数
	//fd： 文件描述符
	//buf：存放读取或写入的字符，char数组
	//count：read表示字符数组buf的长度，wirte表示要写入的字符长度，不一定等于buf的长度
	//返回值：read成功返回读取的长度，
		errno != EAGAIN (或!= EWOULDBLOCK)  read出错
		errno == EAGAIN (或== EWOULDBLOCK)  设置了非阻塞读，并且没有数据到达。失败返回-1，
	wirte失败返回-1
```

使用这两个函数来编写一个拷贝函数`mycp`
```cpp
#include <iostream>
#include <cstring>
#include <unistd.h>
#include <fcntl.h>
using namespace std;
int main(int argc, char *argv[])
{

    int fd1 = open(argv[1], O_RDONLY);
    if (fd1 == -1)
    {
        perror("open argv2 error");
        exit(1);
    }

    int fd2 = open(argv[2], O_WRONLY | O_CREAT | O_TRUNC, 0777);
    if (fd2 == -1)
    {
        perror("open argv2 error");
        exit(1);
    }

    int n = 0;
    char buf[1025];
    while ((n = read(fd1, buf, 1024)) != 0)
    {
        if (n == -1)
        {
            perror("read error");
            exit(1);
        }
        int w = write(fd2, buf, n);
        if (w == -1)
        {
            perror("write error");
            exit(1);
        }
    }

    close(fd1);
    close(fd2);
    return 0;
}
```

![[Pasted image 20240707192855.png]]

### strace命令、
```
strace ./mycp test.txt  test1.txt
```
![[Pasted image 20240708000042.png]]
### 缓冲区
**==read、write函数常常被称为Unbuffered I/O。指的是无用户及缓冲区。但不保证不使用内核缓冲区。==**
![[Pasted image 20240707234710.png]]

### 预读入换输出

![[Pasted image 20240707235459.png]]

# 阻塞与非阻塞

产生阻塞的场景： 读设备文件，都网络文件（都常规文件无阻塞概念），例如终端文件`/dev/tty`，编写一个非阻塞读终端等待超时的代码
```cpp
#include <iostream>
#include <cstring>
#include <unistd.h>
#include <error.h>
#include <fcntl.h>
using namespace std;

#define MSG_TRy "try again\n"
#define MSG_TIMEOUT "time out\n"

int main()
{
    char buf[10];
    int fd, n, i;
    fd = open("/dev/tty", O_RDONLY | O_NONBLOCK);

    if (fd < 0)
    {
        perror("open /dev/tty");
        exit(1);
    }
    for (i = 0; i < 5; i++)
    {
        n = read(fd, buf, 10);
        if (n >= 0)
        {
            break;//证明读到东西了
        }
        if (errno != EAGAIN)
        {
            perror("read /dev/tty");//读出错
            exit(1);
        }
        else//阻塞
        {
            write(STDOUT_FILENO, MSG_TRy, strlen(MSG_TRy));//在终端输出
            sleep(2);
        }
    }

    if (i == 5)
    {
        write(STDOUT_FILENO, MSG_TIMEOUT, strlen(MSG_TIMEOUT));
    }
    else
    {
        write(STDOUT_FILENO, buf, n);//将读到的数据输出
    }
    close(fd);
    return 0;
}
```

![[Pasted image 20240708021103.png]]

读常规文件是不会阻塞的，不管读多少字节，read一定会在有限的时间内返回。从终端设备或网络读则不一定，如果从终端输入的数据没有换行符，调用read读终端设备就会阻塞，如果网络上没有接收到数据包，调用read从网络读就会阻塞，至于会阻塞多长时间也是不确定的，如果一直没有数据到达就一直阻塞在那里。同样，写常规文件是不会阻塞的，而向终端设备或网络写则不一定。

现在明确一下阻塞（Block）这个概念。当进程调用一个阻塞的系统函数时，该进程被置于睡眠（Sleep）状态，这时内核调度其它进程运行，直到该进程等待的事件发生了（比如网络上接收到数据包，或者调用sleep指定的睡眠时间到了）它才有可能继续运行。与睡眠状态相对的是运行（Running）状态，在Linux内核中，处于运行状态的进程分为两种情况：

正在被调度执行。CPU处于该进程的上下文环境中，程序计数器（eip）里保存着该进程的指令地址，通用寄存器里保存着该进程运算过程的中间结果，正在执行该进程的指令，正在读写该进程的地址空间。

就绪状态。该进程不需要等待什么事件发生，随时都可以执行，但CPU暂时还在执行另一个进程，所以该进程在一个就绪队列中等待被内核调度。系统中可能同时有多个就绪的进程，那么该调度谁执行呢？内核的调度算法是基于优先级和时间片的，而且会根据每个进程的运行情况动态调整它的优先级和时间片，让每个进程都能比较公平地得到机会执行，同时要兼顾用户体验，不能让和用户交互的进程响应太慢。

# fcntl改变文件控制属性

```cpp
#include <fcntl.h>
int fcntl(int fd, int op, ... /* arg */ )
	//第二个参数
		获取文件状态：F_GETFL
		设置文件状态：F_DETFL
```

### 改变文件是否阻塞
```cpp
int flags=fcntl(STDIN_FILENO,F_GETFL);
flags|=O_NONBLOCK;//按位或
int ret=fcntl(STDIN_FILENO,F_sETFL,flags);
```

### 位图
![[Pasted image 20240708023905.png]]

# ioctl函数

对设备的I/O通道进行管理，控制设备特性。(主要应用于设备驱动程序中)。
通常用来获取文件的【物理特性】（该特性，不同文件类型所含有的值各不相同） 【ioctl.c】

# lseek函数

### 文件偏移

Linux中可使用系统函数lseek来 **==修改文件偏移量(读写位置)==**

每个打开的文件都记录着当前读写位置，打开文件时读写位置是0，表示文件开头，通常读写多少个字节就会将读写位置往后移多少个字节。但是有一个例外，如果以O_APPEND方式打开，每次写操作都会在文件末尾追加数据，然后将读写位置移到新的文件末尾。lseek和标准I/O库的fseek函数类似，可以移动当前读写位置（或者叫偏移量）。

 回忆fseek的作用及常用参数。 SEEK_SET、SEEK_CUR、SEEK_END
```cpp
int fseek(FILE *stream, long offset, int whence);
成功返回0；失败返回-1
特别的：超出文件末尾位置返回0；往回超出文件头位置，返回-1
```


```cpp
 #include <unistd.h>
off_t lseek(int fd, off_t offset, int whence);
//参数
//fd:文件描述符
//offset：偏移字节数
//whence：偏移参考位置
	//SEEK_SET:起始位置
	//SEEK_CUR：当前位置
	//SEEK_END：末尾位置
//返回值：
	//成功：以起始位置位参考的偏移长度，
	//失败：-1 errno           
```

**==特别的：lseek允许超过文件结尾设置偏移量，文件会因此被拓展==**。
***注意文件“读”和“写”使用同一偏移位置。*** 

### lseek常用应用：

###### 文件的‘’读‘’、‘’写‘’使用同一偏移位置。
###### 使用lseek获取文件大小
```cpp
cout<<lseek(fd, 0, SEEK_END);<<endl;
```

###### 使用lseek扩展文件大小。
```cpp
int lenth =lseek(fd, 99, SEEK_END);//扩展99个字节，用"\0"填充，称为文件空洞
//必须进行IO操作才能保存扩展
write(fd, "\0", 1);//一共扩展100个字节
```
###### truncate拓展文件大小
```cpp
#include <unistd.h>
int truncate(const char *path, off_t length);//必须时已经存在的文件
//参数
//path：文件名称，及路径"./text.txt"
//length:需要扩展的长度
//返回值：成功返回0
```

# 不同进制查看文件
```cpp
od -tcx filename  //查看文件的16进制表示形式
```
![[Pasted image 20240708155618.png]]


```cpp
od -tcd filename  //查看文件的10进制表示形式
```
![[Pasted image 20240708155701.png]]

# 传入传出参数
### 传入参数
1. 指针作为函数参数
2. 通常有const关键字修饰
3. 指针指向有效区域，在函数内部做读操作
```cpp
 #include <string.h>

char *stpcpy(char *restrict dst, const char *restrict src);//第二个参数
```
### 传出参数
1. 指针作为函数参数
2. 在函数调用之前，指针指向的空间可以无意义，但必须有效
3. 在函数内部，做写操作
4. 函数调用结束后，充当函数返回值
```cpp
 #include <string.h>
char *stpcpy(char *restrict dst, const char *restrict src);//第一个参数
```
### 传入传出操作
1. 指针作为函数参数
2. 在函数调用之前，指针指向的空间有实际意义
3. 在函数内部，先做读操作，再做写操作
4. 函数调用结束后，充当函数返回值
```cpp
#include <string.h>
char *strtok(char *restrict str, const char *restrict delim);//第三个参数
```