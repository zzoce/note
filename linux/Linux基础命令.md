# Linux基础命令



## Linux的目录结构

![image-20221027214128453](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027214128.png)

- `/`，根目录是最顶级的目录了
- Linux只有一个顶级目录：`/`
- 路径描述的层次关系同样适用`/`来表示
- /home/itheima/a.txt，表示根目录下的home文件夹内有itheima文件夹，内有a.txt

## 系统目录
bin：存放二进制可执行文件，sleep date等
boot：存放开机启动程序
dev：存放设备文件： 显示器，鼠标，内存，硬盘，等等
home：存放普通用户
etc：用户信息和系统配置文件 passwd、group
lib：库文件：比如C++库啊等等
root：管理员宿主目录（家目录）
usr：用户资源管理目录 ，自己装的软件等


## 常用快捷键

| 快捷键        | 功能      |
| ---------- | ------- |
| CTRL p     | 上一条命令   |
| CTRL n     | 下一条命令   |
| ctrl b     | 左       |
| ctrl f     | 右       |
| ctrl d     | 删除后一个字符 |
| ctrl a     | home    |
| ctrl e     | end     |
| ctrl u     | 删除整行    |
| ctrl k     | 删除光标到行末 |
| ctrl pgup  | 显示上滚    |
| ctrl pgdn  | 显示下滚    |
| ctrl +/-   | 字体大/小   |
| ctrl art t | 新打开一个终端 |
| ctrl l     | 清屏      |


## shell

####   查看当前可使用的shell
```linux
cat /etc/shells
```
![[Pasted image 20240626194525.png]]

#### 查看当前的shell
```linux
echo $SHELL
```

![[Pasted image 20240626194631.png]]


## 快速切换目录

```linux
cd -
```

![[Pasted image 20240626194721.png]]

## ls命令

功能：列出文件夹信息

语法：`ls [-l -h -a] [参数]`

- 参数：被查看的文件夹，不提供参数，表示查看当前工作目录
- -l，以列表形式查看
- -h，配合-l，以更加人性化的方式显示文件大小，必须配合-l使用
- -a，显示隐藏文件
- -R，递归进入文件下的目录

```linux
ll = ls -la 
显示文件大小 ls -lh

ls -l  //显示文件夹下的文件信息
ls -dl  //显示该文件的文件信息
```

![[Pasted image 20240617145716.png]]

## 文件类型
```linux
普通文件：-      /文件夹
目录文件：d      文件夹
字符设备文件：c   // /dev文件下
块设备文件：b     // /dev文件下，磁盘
软连接：l        //类似于快捷方式
管道文件：p
套接字：s
未知文件
```

### 隐藏文件、文件夹

在Linux中以`.`开头的，均是隐藏的。

默认不显示出来，需要`-a`选项才可查看到。

## 查看文件类型，后缀
```
file 文件名
```
<mark style="background: #FF5582A6;">因为</mark>：在linux系统中文件加的后缀并不会影响一个文件夹的类型，所以在搞不清究竟是什么文件时就可以使用该命令查看

## pwd查看当前所在目录

功能：展示当前工作目录

语法：`pwd`

![[Pasted image 20240617150636.png]]

## cd命令

功能：切换工作目录

语法：`cd [目标目录]`

参数：目标目录，要切换去的地方，不提供默认切换到`当前登录用户HOME目录`

![[Pasted image 20240617150643.png]]

## HOME目录

每一个用户在Linux系统中都有自己的专属工作目录，称之为HOME目录。

- 普通用户的HOME目录，默认在：`/home/用户名`

- root用户的HOME目录，在：`/root`



FinalShell登陆终端后，默认的工作目录就是用户的HOME目录



## 相对路径、绝对路径

- 相对路径，==非==`/`开头的称之为相对路径

  相对路径表示以`当前目录`作为起点，去描述路径，如`test/a.txt`，表示当前工作目录内的test文件夹内的a.txt文件

- 绝对路径，==以==`/`开头的称之为绝对路径

  绝对路径从`根`开始描述路径

![[Pasted image 20240617150915.png]]

## 特殊路径符

- `.`，表示当前，比如./a.txt，表示当前文件夹内的`a.txt`文件
- `..`，表示上级目录，比如`../`表示上级目录，`../../`表示上级的上级目录
- `~`，表示用户的HOME目录，比如`cd ~`，即可切回用户HOME目录



## mkdir创建文件夹

功能：创建文件夹

语法：`mkdir [-p] 参数`

- 参数：<mark style="background: #FF5582A6;">必填</mark>，被创建文件夹的路径
- 选项：-p，可选，表示多个层级的目录

```linux
mkdir itheima
mkdir ~/itheima/test
```
![[Pasted image 20240617151447.png]]


### mkdir -p 选项
如果想要一次性创建多个层级的目录，如下图
![[Pasted image 20240617151703.png]]
会报错，因为上级目录itcast和good并不存在，所以无法创建666目录可以通过-p选项，将一整个链条都创建完成。
![[Pasted image 20240617151700.png]]


## touch创建文件

功能：创建文件

语法：`touch 参数`

- 参数：被创建的文件路径

```linux
touch test.txt
```

## 区分文件和文件夹
![[Pasted image 20240617152410.png]]

## cat查看文件内容

功能：查看文件内容

语法：`cat 参数`

- 参数：被查看的文件路径
![[Pasted image 20240617152502.png]]

## tac逆序查看文件内容

![[Pasted image 20240626202531.png]]
## more翻页查看文件内容

功能：查看文件，***可以支持翻页查看***

语法：`more 参数`

- 参数：被查看的文件路径
- 在查看过程中：
  - `空格`键翻页
  - `回车`下一行
  - `q`退出查看

## less翻页查看文件内容

按`CTRL C`或`q`终止显示

## head显示文件前n行
```linux
head -n file
```
显示文件前n行


## cp拷贝命令

功能：复制文件、文件夹

语法：`cp [-r] 参数1 参数2`

- 参数1，被复制的
- 参数2，要复制去的地方
- 选项：-r，***可选，复制文件夹使用***，-a 全部拷贝，包括子目录文件

示例：

- cp a.txt b.txt，复制当前目录下a.txt为b.txt
- cp a.txt test/，复制当前目录a.txt到test文件夹内
- cp -r test test2，复制文件夹test到当前文件夹内为test2存在



## mv移动

功能：移动文件、文件夹

语法：`mv 参数1 参数2`

- 参数1：被移动的
- 参数2：要移动去的地方，参数2如果不存在，则会进行改名



## rm删除

功能：删除文件、文件夹

语法：`rm [-r -f] 参数...参数`

- 参数：支持多个，每一个表示被删除的，**==空格进行分隔==**
- 选项：-r，删除文件夹使用
- 选项：-f，强制删除，不会给出确认提示，一般root用户会用到


> [!NOTE] 注意
> rm命令很危险，一定要注意，特别是切换到root用户的时候。

### rm删除文件、文件夹 - 通配符
rm命令支持通配符 *，用来做模糊匹配

•符号\* 表示通配符，即匹配任意内容（包含空），示例：

•test\*，表示匹配任何以test开头的内容

•\*test，表示匹配任何以test结尾的内容

•\*test\*，表示匹配任何包含test的内容

演示：

•删除所有以test开头的文件或文件夹
![[Pasted image 20240617153259.png]]



> [!NOTE] 知识小贴士
> rm是一个危险的命令，特别是在处于root（超级管理员）用户的时候。请谨慎使用。
> 如下命令，请千万千万不要在root管理员用户下执行：
> rm -rf /
> rm -rf /*
> 


### 强制删除
演示强制删除，-f选项

可以通过 su - root，并输入密码123456（和普通用户默认一样）临时切换到root用户体验

通过输入***exit命令，退回普通用户***。（临时用root，用完记得退出，不要一直用，关于root我们后面会讲解）

## rmdir删除空目录

非空目录删不掉

## which 查看程序所在路径

功能：查看***命令的程序本体***文件路径

语法：`which 参数`

- 参数：被查看的命令
![[Pasted image 20240617154039.png]]


## find查找文件


功能：搜索文件

语法1按文件名搜索：`find 路径 -name 参数`

- 路径，搜索的起始路径，便是在哪个文件夹下搜索
- 参数，搜索的关键字，支持通配符*， 比如：`*`test表示搜索任意以test结尾的文件

![[Pasted image 20240617154504.png]]


##### find按文件大小搜索

语法：
```linux
find 起始路径 -size [+/-]n[kMG]

find ./ -size +20M -size -50M
当前目录下大于20M小于50M的文件
```

1. +、- 表示大于和小于
2. n表示大小数字
3. kMG表示大小单位，k(小写字母)表示kb，M表示MB，G表示GB

> [!NOTE] 注意
> 默认单位为b 并不是字节，而是块。一个块512个字节，硬盘存取的最小单位


示例：

1. 查找小于10KB的文件： find / -size -10k
2. 查找大于100MB的文件：find / -size +100M
3. 查找大于1GB的文件：find / -size +1G

##### find按类型查找

```
find 路径 -type '类型'

find ./ -type 'l'
查找当前目录下的软连接
```

![[Pasted image 20240628150404.png]]

文件类型表示符[[Linux基础命令#Linux基础命令#文件类型|点这里]]


##### find命令控制查找目录层数
```
find 路径 -maxdepth n 搜索类型 参数
```
**==n表示搜索层数==**


##### find帮助文档
```
man find
```
![[Pasted image 20240628171657.png]]
##### -exec执行命令 
```linux
 find ./ -maxdepth 1 -type f -exec ls -l {} \;
```
![[Pasted image 20240628170440.png]]
```linux
 find ./ -maxdepth 1 -name "*.cpp" -exec rm {} \;
```

![[Pasted image 20240628170843.png]]

##### xargs执行命令 
```linux
find ./ -maxdepth 1 -name "*.cpp" | xargs ls -l
```
![[Pasted image 20240628182541.png]]
***相比exec 当数据较多时分片处理，效率更高***
![[Pasted image 20240628182646.png]]
  <mark style="background: #FF5582A6;">注意：</mark>
  无法查询带有空格的文件，**==解决方法xargs不以空格为拆分依据==**
  ```linux
  find ./ -maxdepth 1 -name "*.cpp" -print0| xargs -print0 ls -l
```
以0（null  ）作为拆分依据
## grep查找内容

功能：过滤关键字,从文件中通过关键字过滤文件行。

语法：`grep [-n] 关键字 文件路径`

- 选项-n，可选，表示在结果中 **==显示匹配的行的行号。==**
- 选项-r，可选，表示允许路径递归
- 参数，关键字，必填，表示过滤的关键字，***带有空格或其它特殊符号，建议使用””将关键字包围起来***
- 参数，文件路径，必填，表示要过滤内容的文件路径，可作为内容输入端口

![[Pasted image 20240617155417.png]]


#### 递归查找
```linux
grep -r "#include" ./
```

![[Pasted image 20240628173133.png]]

#### grep和管道符、
```linux
ps aux |grep zzoce
```
![[Pasted image 20240628180926.png]]
```
find ./ -name "*.cpp" | grep Thread
```
![[Pasted image 20240628181239.png]]


## wc统计命令

功能：统计,可以通过wc命令统计文件的行数、单词数量等

语法：`wc [-c -m -l -w] 文件路径`

- 选项，-c，统计bytes数量
- 选项，-m，统计字符数量
- 选项，-l，统计行数
- 选项，-w，统计单词数量
- 参数，文件路径，被统计的文件，可作为内容输入端口



> 参数文件路径，可作为管道符的输入



## 管道符|

写法：`|`

功能：将符号左边的结果，作为符号右边的输入

示例：

`cat a.txt | grep itheima`，将cat a.txt的结果，作为grep命令的输入，用来过滤`itheima`关键字



可以支持嵌套：

`cat a.txt | grep itheima | grep itcast`



## echo输出内容

功能：输出内容

语法：`echo 参数`

- 参数：被输出的内容



### \`反引号

功能：被两个反引号包围的内容，会作为命令执行

示例：

- echo \`pwd\`，会输出当前工作目录




## tail显示文件尾部n行

功能：**查看文件尾部内容**，跟踪文件的最新更改，语法如下：

语法：`tail [-f -num] 参数`

- 参数：被查看的文件
- 选项：-f，持续跟踪文件修改
- 选项, -num，表示，查看尾部多少行，不填默认10行
![[Pasted image 20240617161602.png]]


## head显示文件前n行

功能：查看文件头部内容

语法：`head [-n] 参数`

- 参数：被查看的文件
- 选项：-n，查看的行数



## 重定向符

功能：将符号左边的结果，输出到右边指定的文件中去

- `>`，表示覆盖输出
- `>>`，表示追加输出

```bash
ls / > test.txt
```

![[Pasted image 20240617161509.png]]

## vi编辑器

### 命令模式快捷键

![image-20221027215841573](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027215841.png)

![image-20221027215846581](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027215846.png)

![image-20221027215849668](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027215849.png)

### 底线命令快捷键

![image-20221027215858967](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027215858.png)



## 命令的选项

我们学习的一系列Linux命令，它们所拥有的选项都是非常多的。

比如，简单的ls命令就有：-a -A -b -c -C -d -D -f -F -g -G -h -H -i -I -k -l -L -m -n -N -o -p -q -Q -r-R -s -S -t -T -u -U -v -w -x -X -1等选项，可以发现选项是极其多的。

课程中， 并不会将全部的选项都进行讲解，否则，一个ls命令就可能讲解2小时之久。

课程中，会对常见的选项进行讲解， 足够满足绝大多数的学习、工作场景。



### 查看命令的帮助

可以通过：`命令 --help`查看命令的帮助手册

![image-20221027220005610](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027220005.png)

### 查看命令的详细手册

可以通过：`man 命令`查看某命令的详细手册

![image-20221027220009949](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027220010.png)



## su命令

切换用户

语法：`sudo su [-] [用户]`

切换用户后，可以通过exit命令退回上一个用户，也可以使用快捷键：ctrl + d
![image-20221027222021619](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027222021.png)



## sudo命令

![image-20221027222035337](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027222035.png)



比如：

```shell
sudo visudo
//在打开的文件插入下面的语句
zzoce ALL=(ALL)       NOPASSWD: ALL
```

在visudo内配置如上内容，可以让zzoce用户，***无需密码***直接使用`sudo`


## 权限管理

![[Pasted image 20240618150650.png]]
1. 序号1，表示文件、文件夹的权限控制信息
2. 序号2，表示文件、文件夹所属用户
3. 序号3，表示文件、文件夹所属用户组

![[Pasted image 20240618150702.png]]
举例：drwxr-xr-x，表示：

1. 这是一个文件夹，首字母d表示
2. 所属用户(右上角图序号2)的权限是：有r有w有x，rwx
3. 所属用户组(右上角图序号3)的权限是：有r无w有x，r-x （-表示无此权限）
4. 其它用户的权限是：有r无w有x，r-x
   
rwx

1. r表示读权限
2. w表示写权限
3. x表示执行权限

针对文件、文件夹的不同，rwx的含义有细微差别

1. r，针对文件可以查看文件内容
	1. 针对文件夹，可以查看文件夹内容，如ls命令
2. w，针对文件表示可以修改此文件
	1. 针对文件夹，可以在文件夹内：创建、删除、改名等操作
3. x，针对文件表示可以将文件作为程序执行
	1. 针对文件夹，表示可以更改工作目录到此文件夹，即cd进入

### umask产看文件夹权限
在文件夹下执行`umsak`，命令
```
umask
```
![[Pasted image 20240706155508.png]]
于是test文件的权限为
```
777&umask=755
```

### chmod修改文件夹权限

修改文件、文件夹权限

语法：`chmod [-R] 权限 参数`

- 权限，要设置的权限，比如755，表示：`rwxr-xr-x`

  ![image-20221027222157276](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027222157.png)

- 参数，被修改的文件、文件夹

- 选项-R，设置文件夹和其内部全部内容一样生效
```
sudo chmod -R u=rwx,g=rx,o=rx network
sudo chmod -R 755 network
```
![[Pasted image 20240618151624.png]]

### chown修改文件所属用户，组

修改文件、文件夹所属用户、组

语法：`chown [-R] [用户][:][用户组] 文件或文件夹`

![image-20221027222326192](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027222326.png)


## 用户、用户组
Linux系统中可以：

1. 配置多个用户
2. 配置多个用户组
3. 用户可以加入多个用户组中
![[Pasted image 20240617205848.png]]
Linux中关于权限的管控级别有2个级别，分别是：

1. 针对用户的权限控制
2. 针对用户组的权限控制
比如，针对某文件，可以控制用户的权限，也可以控制用户组的权限。
所以，我们需要学习在Linux中进行用户、用户组管理的基础命令，为后面学习权限控制打下基础。

### 用户组管理

以下命令需root用户执行

创建用户组

```
groupadd 用户组名
```

删除用户组

```
groupdel 用户组名
```

为后续演示，我们创建一个itcast用户组：groupadd itcast



### 用户管理

###### whoami查看当前登录用户

```
whoami
```
![[Pasted image 20240628143129.png]]

###### 创建用户

```
useradd [-g -m -d] 用户名
```

选项：-g指定用户的组，不指定-g，会创建同名组并自动加入，指定-g需要组已经存在，如已存在同名组，必须使用-g
选项：-d指定用户HOME路径，不指定，HOME目录默认在：/home/用户名
选项：-m选项告诉 `useradd` 创建用户的主目录。

```
sudo useradd -m -d /home/test -g testgroup test
```

###### 指定附加组

使用 `-G` 选项来指定用户的附加组（可以是多个）。以下命令创建一个用户 `test`，指定主组为 `testgroup`，并添加到 `group1` 和 `group2` 附加组：

```
sudo useradd -m -d /home/test -g testgroup -G group1,group2 test
```


###### 为新用户设置密码
```
sudo passwd test
```
###### 赋予新用户sudo权限
```linux
sudo visudo
//在打开的文件插入下面的语句
用户名 ALL=(ALL)       NOPASSWD: ALL
```


###### 删除用户

```
userdel [-r] 用户名
```

选项：-r，删除用户的HOME目录，不使用-r，删除用户时，HOME目录保留

###### 查看用户所属组

```
id [用户名]
```

参数：用户名，被查看的用户，如果不提供则查看自身

###### 修改用户所属组

```
usermod -aG 用户组 用户名，将指定用户加入指定用户组
```

## genent查看用户，用户组

- `getent group`，查看系统全部的用户组

  ![image-20221027222446514](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027222446.png)

- `getent passwd`，查看系统全部的用户

  ![image-20221027222512274](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027222512.png)




## ctrl + d 退出或登出
•可以通过快捷键：ctrl + d，退出账户的登录
![[Pasted image 20240618160849.png]]
•或者退出某些特定程序的专属页面
![[Pasted image 20240618160856.png]]


## history查看历史命令
`history`

![[Pasted image 20240618161142.png]]

![[Pasted image 20240618161258.png]]

可以通过：!命令前缀，自动执行上一次匹配前缀的命令
![[Pasted image 20240618161429.png]]


可以通过快捷键：ctrl + r，输入内容去匹配历史命令
![[Pasted image 20240618161836.png]]
如果搜索到的内容是你需要的，那么：

1. 回车键可以直接执行
2. 键盘左右键，可以得到此命令（不执行）




## 光标快速移动

1. ctrl + a，跳到命令开头
2. ctrl + e，跳到命令结尾
3. ctrl + 键盘左键，向左跳一个单词
4. ctrl + 键盘右键，向右跳一个单词


## 清屏

通过快捷键ctrl + l，可以清空终端内容
或通过命令clear得到同样效果

# Linux常用操作

## 软件安装

- CentOS系统使用：
  - yum [install remove search] [-y] 软件名称
    - install 安装
    - remove 卸载
    - search 搜索
    - -y，自动确认
- Ubuntu系统使用
  - apt [install remove search] [-y] 软件名称
    - install 安装
    - remove 卸载
    - search 搜索
    - -y，自动确认

> yum 和 apt 均需要root权限



## systemctl控制系统服务的启动关闭

功能：控制系统服务的启动关闭等

语法：`systemctl 服务名 start | stop | restart | disable | enable | status `

- start，启动
- stop，停止
- status，查看状态
- disable，关闭开机自启
- enable，开启开机自启
- restart，重启
```
sudo service mysql start//启动
sudo service mysql stop//关闭
```

系统内置的服务比较多，比如：

1. NetworkManager，主网络服务
2. network，副网络服务
3. firewalld，防火墙服务
4. sshd，ssh服务（FinalShell远程登录Linux使用的就是这个服务）

## ln创建软链接(快捷方式)

功能：创建文件、文件夹软链接（快捷方式）

语法：`ln -s 参数1 参数2`

- 参数1：被链接的
- 参数2：要链接去的地方（快捷方式的名称和存放位置）

```
ln -s /etc/yum.conf ~/yum.conf
ln -s /etc/yum ~/yum
```
![[Pasted image 20240626205348.png]]

***软连接的权限不影响文件本身***


## 硬链接
```linux
ln file(要硬连接的文件)  finle
```

***对任意一个文件修改，都会对同步到硬链接的文件当中，并且他们的权限，并不占用额外的存储空间***
因为他们使用同一个[[文件系统#inode|inode]]，相当于对同一个文件操作，删除时并不会全部删除，只删除那一个，硬链接计数相应改变

<mark style="background: #FF5582A6;">注意：</mark>
硬链接创建时不需要使用绝对路径，并且在移动目录后依然可以使用！！！



## 日期

语法：`date [-d] [+格式化字符串]`

- -d 按照给定的字符串显示日期，一般用于日期计算

- 格式化字符串：通过特定的字符串标记，来控制显示的日期格式
  - %Y   年%y   年份后两位数字 (00..99)
  - %m   月份 (01..12)
  - %d   日 (01..31)
  - %H   小时 (00..23)
  - %M   分钟 (00..59)
  - %S   秒 (00..60)
  - %s   自 1970-01-01 00:00:00 UTC 到现在的秒数



示例：
```
date "+%Y-%m-%d %H:%M:%S"
```
![[Pasted image 20240618165836.png]]

- -d选项日期计算

![[Pasted image 20240618165914.png|951]]

支持的时间标记为：

1. year年
2. month月
3. day天
4. hour小时
5. minute分钟
6. second秒


## 时区

修改时区为中国时区
```
sudo rm -f /etc/localtime
sudo ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```





## ntp同步时间

功能：同步时间

安装：`apt install -y ntp`

启动管理：`systemctl start | stop | restart | status | disable | enable ntpd`

手动校准时间：`ntpdate -u ntp.aliyun.com`



## ip地址

格式：a.b.c.d

- abcd为0~255的数字

特殊IP：

- 127.0.0.1，表示本机
- 0.0.0.0
  - 可以表示本机
  - 也可以表示任意IP（看使用场景）

查看ip：`ifconfig`
![[Pasted image 20240618182805.png]]


## 主机名

功能：Linux系统的名称

查看：`hostname`

设置：`hostnamectl set-hostname 主机名`

![[Pasted image 20240618183148.png]]






## 配置VMware固定IP

1. 修改VMware网络，参阅PPT，图太多

2. 设置Linux内部固定IP

   修改文件：`/etc/sysconfig/network-scripts/ifcfg-ens33`

   示例文件内容：

   ```shell
   TYPE="Ethernet"
   PROXY_METHOD="none"
   BROWSER_ONLY="no"
   BOOTPROTO="static"			# 改为static，固定IP
   DEFROUTE="yes"
   IPV4_FAILURE_FATAL="no"
   IPV6INIT="yes"
   IPV6_AUTOCONF="yes"
   IPV6_DEFROUTE="yes"
   IPV6_FAILURE_FATAL="no"
   IPV6_ADDR_GEN_MODE="stable-privacy"
   NAME="ens33"
   UUID="1b0011cb-0d2e-4eaa-8a11-af7d50ebc876"
   DEVICE="ens33"
   ONBOOT="yes"
   IPADDR="192.168.88.131"		# IP地址，自己设置，要匹配网络范围
   NETMASK="255.255.255.0"		# 子网掩码，固定写法255.255.255.0
   GATEWAY="192.168.88.2"		# 网关，要和VMware中配置的一致
   DNS1="192.168.88.2"			# DNS1服务器，和网关一致即可
   ```

## 主机名和IP地址的映射

#### 域名解析

IP地址实在是难以记忆，有没有什么办法可以通过主机名或替代的字符地址去代替数字化的IP地址呢？实际上，我们一直都是通过字符化的地址去访问服务器，很少指定IP地址，比如，我们在浏览器内打开：www.baidu.com，会打开百度的网址。其中，www.baidu.com，是百度的网址，我们称之为：域名
![[Pasted image 20240618184351.png]]

即：

1. 先查看本机的记录（私人地址本）
	1. Windows看：C:\\Windows\\System32\\drivers\\etc\\hosts
	2. Linux看：/etc/hosts
2. 再联网去DNS服务器（如114.114.114.114，8.8.8.8等）询问


#### 映射

比如，我们FinalShell是通过IP地址连接到的Linux服务器，那有没有可能通过域名（主机名）连接呢？

可以，我们只需要在Windows系统的：C:\\Windows\\System32\\drivers\\etc\\hosts文件中配置记录即可






## ping命令

测试网络是否联通

语法：`ping [-c num] 参数`

![image-20221027221129782](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027221129.png)



## wget命令下载文件

wget是非交互式的文件下载器，可以在命令行内下载网络文件

语法：`wget [-b] url`

1. 选项：-b，可选，后台下载，会将日志写入到当前工作目录的wget-log文件
2. 参数：url，下载链接

示例：

下载apache-hadoop 3.3.0版本：wget http://archive.apache.org/dist/hadoop/common/hadoop-3.3.0/hadoop-3.3.0.tar.gz
![[Pasted image 20240618190733.png]]

在后台下载：
```
wget -b http://archive.apache.org/dist/hadoop/common/hadoop-3.3.0/hadoop-3.3.0.tar.gz
```

通过tail命令可以监控后台下载进度：
```
tail -f wget-log
```

## curl命令

curl可以发送http网络请求，可用于：下载文件、获取信息等

语法：`curl [-O] url`

1. 选项：-O，用于下载文件，当url是下载链接时，可以使用此选项保存文件
2. 参数：url，要发起请求的网络地址

示例：

向cip.cc发起网络请求：
```
curl cip.cc
```
返回你的IP地址
![[Pasted image 20240618193147.png]]


向python.itheima.com发起网络请求
```
curl python.itheima.com
```
返回网页源码

通过curl下载hadoop-3.3.0安装包：
```
curl -O http://archive.apache.org/dist/hadoop/common/hadoop-3.3.0/hadoop-3.3.0.tar.gz
```


## 端口

端口，是设备与外界通讯交流的出入口。端口可以分为：物理端口和虚拟端口两类

1. 物理端口：又可称之为接口，是可见的端口，如USB接口，RJ45网口，HDMI端口等
2. 虚拟端口：是指计算机内部的端口，是不可见的，是用来操作系统和外部进行交互使用的


Linux系统是一个超大号小区，可以支持65535个端口，这6万多个端口分为3类进行使用：

**==公认端口==**：1~1023，通常用于一些系统内置或知名程序的预留使用，如SSH服务的22端口，HTTPS服务的443端口

非特殊需要，不要占用这个范围的端口

**==注册端口：==** 1024~49151，通常可以随意使用，用于松散的绑定一些程序\服务
**==动态端口：==** 49152~65535，通常不会固定绑定程序，而是当程序对外进行网络链接时，用于临时使用。

![[Pasted image 20240618194629.png]]

如图中，计算机A的微信连接计算机B的微信，A使用的50001即动态端口，临时找一个端口作为出口

计算机B的微信使用端口5678，即注册端口，长期绑定此端口等待别人连接
####  nmap查看端口占用

安装nmap
```
sudo apt install nmap
```

语法：nmap 被查看的IP地址
```
nmap 127.0.0.1
```

#### netstat查看指定端口的占用情况

安装netstat
```
sudo apt install net-tools
```

语法：netstat -anp | grep 端口号

## 进程管理

#### ps命令查看进程信息

功能：查看进程信息

语法：`ps [-e -f]`，查看全部进程信息，可以搭配grep做过滤：`ps -ef | grep xxx`
1. 选项：-e，显示出全部的进程
2. 选项：-f，以完全格式化的形式展示信息（展示全部信息）

> [!NOTE] 选项
> -e 显示所有进程
> -f 全格式
> -h 不显示标题
> -l 长格式
> -w 宽输出
> -r 只显示正在运行的进程
> -a 即all，查看当前系统的所有用户的所有进程

一般来说，固定用法就是： ps -ef 列出全部进程的全部信息
![[Pasted image 20240618200738.png]]

从左到右分别是：

1. UID：进程所属的用户ID
2. PID：进程的进程号ID
3. PPID：进程的父ID（启动此进程的其它进程）
4. C：此进程的CPU占用率（百分比）
5. STIME：进程的启动时间
6. TTY：启动此进程的终端序号，如显示?，表示非终端启动
7. TIME：进程占用CPU的时间
8. CMD：进程对应的名称或启动路径或启动命令

#### 查询指定的线程
```
ps aux | grep zzoce
```
![[Pasted image 20240628180406.png]]
####  kill命令关闭进程

语法：
```
kill [-9] 进程ID
```

选项：-9，表示强制关闭进程。不使用此选项会向进程发送信号要求其关闭，但是否关闭看进程自身的处理机制。


## top命令查看主机运行状态

功能：查看主机运行状态，可以通过top命令查看CPU、内存使用情况，**==类似Windows的任务管理器==**
默认***每5秒刷新一次***，语法：直接输入top即可，按q或ctrl + c退出

语法：`top`，查看基础信息
![[Pasted image 20240618202024.png]]
第一行：
![[Pasted image 20240618202246.png]]
top：命令名称，20:19:02：当前系统时间，up 24 min：启动了24分钟，0 users：0个用户登录，load：0、0、0分钟负载

第二行：
![[Pasted image 20240618202259.png]]
Tasks：5个进程，1 running：1个进程子在运行，4 sleeping：4个进程睡眠，0个停止进程，0个僵尸进程

第三行：
![[Pasted image 20240618202315.png]]
%Cpu(s)：CPU使用率，us：用户CPU使用率，sy：系统CPU使用率，ni：高优先级进程占用CPU时间百分比，id：空闲CPU率，wa：IO等待CPU占用率，hi：CPU硬件中断率，si：CPU软件中断率，st：强制等待占用CPU率
第四、五行：
![[Pasted image 20240618202330.png]]
Kib Mem：物理内存，total：总量，free：空闲，used：使用，buff/cache：buff和cache占用
Kib Swap：虚拟内存（交换空间），total：总量，free：空闲，used：使用，buff/cache：buff和cache占用

**==可用选项：==**

![image-20221027221340729](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027221340.png)


**==交互式模式中，可用快捷键：==**

![image-20221027221354137](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027221354.png)



## df查看磁盘占用

查看磁盘占用
语法：
```
df [-h]
```
选项：-h，以更加人性化的单位显示

![[Pasted image 20240618202901.png]]



## iostat命令查看CPU、磁盘的相关信息

```
sudo apt install sysstat
```

查看CPU、磁盘的相关信息
```
iostat [-x] [num1] [num2]
```

选项：
1. -x，显示更多信息
2. num1：数字，刷新间隔，
3. num2：数字，刷新几次
![[Pasted image 20240618203425.png]]
1. tps：该设备每秒的传输次数（Indicate the number of transfers per second that were issued to the device.）。"一次传输"意思是"一次I/O请求"。多个逻辑请求可能会被合并为"一次I/O请求"。"一次传输"请求的大小是未知的。
2. rrqm/s：  每秒这个设备相关的读取请求有多少被Merge了（当系统调用需要读取数据的时候，VFS将请求发到各个FS，如果FS发现不同的读取请求读取的是相同Block的数据，FS会将这个请求合并Merge, 提高IO利用率, 避免重复调用）；
3. wrqm/s：  每秒这个设备相关的写入请求有多少被Merge了。
4. rsec/s：  每秒读取的扇区数；sectors
5. wsec/：  每秒写入的扇区数。
6. **rKB/s：  每秒发送到设备的读取请求数**
7. **wKB/s：  每秒发送到设备的写入请求数**
8. avgrq-sz   平均请求扇区的大小
9. avgqu-sz   平均请求队列的长度。毫无疑问，队列长度越短越好。   
10. await：    每一个IO请求的处理的平均时间（单位是微秒毫秒）。
11. svctm      表示平均每次设备I/O操作的服务时间（以毫秒为单位）
12. **%util：   磁盘利用率**




## sar命令查看网络的相关统计

```
sudo apt install sysstat
```
sar命令非常复杂，这里仅简单用于统计网络
```
sar -n DEV num1 num2
```

选项：
1. -n，查看网络，
2. DEV表示查看网络接口
3. num1：刷新间隔（不填就查看一次结束）
4. num2：查看次数（不填无限次数）


信息解读：

1. IFACE 本地网卡接口的名称
2. rxpck/s 每秒钟接受的数据包
3. txpck/s 每秒钟发送的数据包
4. rxKB/S 每秒钟接受的数据包大小，单位为KB
5. txKB/S 每秒钟发送的数据包大小，单位为KB
6. rxcmp/s 每秒钟接受的压缩数据包
7. txcmp/s 每秒钟发送的压缩包
8. rxmcst/s 每秒钟接收的多播数据包


## env查看环境变量

```
env
```
![[Pasted image 20240618205041.png]]




### PATH变量

记录了执行程序的搜索路径,可以将自定义路径加入PATH内，实现自定义命令在任意地方均可执行的效果

我们说无论当前工作目录是什么，都能执行/usr/bin/cd这个程序，这个就是借助环境变量中：PATH这个项目的值来做到的。

```
env | grep PATH
```
![[Pasted image 20240618205355.png]]
当执行任何命令，都会按照顺序，从上述路径中搜索要执行的程序的本体
比如执行cd命令，就从第二个目录/usr/bin中搜索到了cd命令，并执行


## $符号取出指定的环境变量的值

可以取出指定的环境变量的值

语法：`$变量名`

示例：

`echo $PATH`，输出PATH环境变量的值
![[Pasted image 20240618205523.png]]
`echo ${PATH}ABC`，输出PATH环境变量的值以及ABC
![[Pasted image 20240618205554.png]]
**==如果变量名和其它内容混淆在一起，可以使用${}==**

## 自行设置环境变量

Linux环境变量可以用户自行设置，其中分为：

1. 临时设置，语法：export 变量名=变量值
2. 永久生效
	1. 针对当前用户生效，配置在当前用户的：  ~/.bashrc文件中
	2. 针对所有用户生效，配置在系统的：  /etc/profile文件中
	3. 并通过语法：`source 配置文件`，进行立刻生效，或重新登录FinalShell生效

![[Pasted image 20240618205837.png]]


**==测试：==**

在当前HOME目录内创建文件夹，myenv，在文件夹内创建文件mkhaha
通过vim编辑器，在mkhaha文件内填入：echo 哈哈哈哈哈

完成上述操作后，随意切换工作目录，执行mkhaha命令尝试一下，会发现无法执行

**==修改PATH的值==**

临时修改PATH：`export PATH=$PATH:/home/itheima/myenv`，再次执行mkhaha，无论在哪里都能执行了
或将`export PATH=$PATH:/home/itheima/myenv`，填入用户环境变量文件或系统环境变量文件中去

## 下载和上传

我们可以通过FinalShell工具，方便的和虚拟机进行数据交换。在FinalShell软件的下方窗体中，提供了Linux的文件系统视图，可以方便的：
1. 浏览文件系统，找到合适的文件，右键点击下载，即可传输到本地电脑
2. 浏览文件系统，找到合适的目录，将本地电脑的文件拓展进入，即可方便的上传数据到Linux中
![[Pasted image 20240618210621.png]]

#### rz、sz命令
当然，除了通过FinalShell的下方窗体进行文件的传输以外，也可以通过rz、sz命令进行文件传输。

rz、sz命令需要安装，
```
sudo -y install lrzsz
```

**==rz命令，进行上传，语法：直接输入rz即可==**

![[Pasted image 20240618211014.png]]


**==sz命令进行下载，语法：sz 要下载的文件==**

![[Pasted image 20240618210741.png]]

文件会自动下载到桌面的：fsdownload文件夹中。


> [!NOTE] 注意
> 注意，rz、sz命令需要终端软件支持才可正常运行
> FinalShell、SecureCRT、XShell等常用终端软件均支持此操作





## 压缩解压

Linux和Mac系统常用有2种压缩格式，后缀名分别是：

1. .tar，称之为tarball，归档文件，即简单的将文件组装到一个.tar的文件内，并没有太多文件体积的减少，仅仅是简单的封装
2. .gz，也常见为.tar.gz，gzip格式压缩文件，即使用gzip压缩算法将文件压缩到一个文件内，可以极大的减少压缩后的体积

针对这两种格式，使用tar命令均可以进行压缩和解压缩的操作

```
tar [-c -v -x -f -z -C] 参数1 参数2
```
1. -c，创建压缩文件，用于压缩模式
2. -v，显示压缩、解压过程，用于查看进度
3. -x，解压模式
4. -f，要创建的文件，或要解压的文件，-f选项必须在所有选项中位置处于最后一个
5. -z，gzip模式，不使用-z就是普通的tarball格式
6. -C，选择解压的目的地，用于解压模式


> [!NOTE] 知识小贴士
> -z选项如果使用的话，**==一般处于选项位第一个==**
> -f选项，***必须***在选项位最后一个



### 压缩

`tar -zcvf 压缩包 被压缩1...被压缩2...被压缩N`
-z表示使用gzip，可以不写

`zip [-r] 参数1 参数2 参数N`
-r，被压缩的包含文件夹的时候，需要使用-r选项，和rm、cp等命令的-r效果一致

**==示例==**
```
tar -cvf test.tar 1.txt 2.txt 3.txt
将1.txt 2.txt 3.txt 压缩到test.tar文件内

tar -zcvf test.tar.gz 1.txt 2.txt 3.txt
将1.txt 2.txt 3.txt 压缩到test.tar.gz文件内，使用gzip模式

zip test.zip a.txt b.txt c.txt
将a.txt b.txt c.txt 压缩到test.zip文件内

zip -r test.zip test itheima a.txt
将test、itheima两个文件夹和a.txt文件，压缩到test.zip文件内
```


### 解压

```
tar -zxvf 被解压的文件 -C 要解压去的地方
```
1. -z表示使用gzip，可以省略
2. -C，可以省略，指定要解压去的地方，不写解压到当前目录

 
```
unzip [-d] 参数
```
1. -d，指定要解压去的位置，同tar的-C选项
2. 参数，被解压的zip压缩包文件

示例

```
常用的tar解压组合有

tar -xvf test.tar
解压test.tar，将文件解压至当前目录

tar -xvf test.tar -C /home/itheima
解压test.tar，将文件解压至指定目录（/home/itheima）

tar -zxvf test.tar.gz -C /home/itheima
以Gzip模式解压test.tar.gz，将文件解压至指定目录（/home/itheima）





unzip test.zip
将test.zip解压到当前目录

unzip test.zip -d /home/itheima
将test.zip解压到指定文件夹内（/home/itheima）
```


## alias给命令起别名
```
alias 别名='命令'
```
![[Pasted image 20240628194724.png]]



