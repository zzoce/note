# GCC编译
![[Pasted image 20240703165654.png]]

## 常用参数
### 查看gcc版本号
```
 gcc -v/-v/-version：
```
![[Pasted image 20240704132543.png]]
### -I：指定头文件目录
```
-I：目录 
g++ -I ./Thread hello.cpp -o hello
//gcc编译.c文件，g++编译.cpp文件
```
![[Pasted image 20240704134807.png]]
如果头文件和源码在同一个文件夹，则不需要指定头文件目录
### -c ：只编译，生成.o文件
**==只做预处理，编译，和汇编==**,得到二进制文件
```
g++ -c hello.cpp
```
![[Pasted image 20240704135613.png]]
### -g ：编译包含调试信息，支持gdb调试
编译时添加调试语句，支持调试功能
```
g++ hello.cpp -o hello2 -g
```
![[Pasted image 20240704140101.png]]

**==使用gdb调试hello1==**
![[Pasted image 20240704141441.png]]

**==使用gdb调试hello2==**
![[Pasted image 20240704142726.png]]

### -On： 对程序进行优化
**==n=0~3编译优化，n越大优化得越多==**，例如没有被使用的变量，while(0)等等
```
```

### -Wall： 提示更多警告信息
```
 g++ hello.cpp -o hello3 -Wall
```

![[Pasted image 20240704143905.png]]

### -D ：编译时定义宏
```
 g++ hello.cpp -o hello3 -D HELLO
```
![[Pasted image 20240704145821.png]]

> [!NOTE]  知识小贴士
> 在编写大项目时，通常使用这种方法来编写调试语句，方便在发布时统一注释掉，以及后续维护

### -E：生成预处理文件
### -M：生成.c文件与头文件依赖关系以用于Makefile，包括系统库的头文件
### -MM：生成.c文件与头文件依赖关系以用于Makefile，不包括系统库的头文件

# GDB调试
### 常用指令

1. -g：使用该参数编译可以执行文件，得到调试表。  
2. gdb： ./a.out  
3. list： list 1 列出源码。根据源码指定 行号设置断点。  
4. b： b 20 在20行位置设置断点。  
5. run/r: 运行程序  
6. n/next: 下一条指令（会越过函数）  
7. s/step: 下一条指令（会进入函数）  
8. p/print：p i 查看变量的值。   
9. continue：继续执行断点后续指令。  
10. finish：结束当前函数调用。  
11. quit：退出gdb当前调试。

### 其他指令：  
1. run：使用run查找段错误出现位置。  
2. start：直接启动调试
3. set args： 设置main函数命令行参数 （在 start、run 之前）  
4. run 字串1 字串2 ...: 设置main函数命令行参数  
5. info b: 查看断点信息表  
6. b 20 if i = 5： 设置条件断点。  
7. ptype：查看变量类型。  
8. bt：列出当前程序正存活着的栈帧。  
9. frame： 根据栈帧编号，切换栈帧。  
10. display：设置跟踪变量  
11. undisplay：取消设置跟踪变量。 使用跟踪变量的编号。


![[Pasted image 20240705141220.png]]