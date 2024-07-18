# 简单了解Makefile
#### 1.Makefile简介

​ **Makefile是一个GNU make程序在执行时默认读取的配置文件，写好Makefile之后，配合make工具，只需要一个“make”命令，整个工程就能完全自动编译，极大地提高了软件开发的效率。**

> ​ Makefile 可以简单的认为是一个工程文件的编译规则，描述了整个工程的编译和链接等规则。
> 
> ​ 其中包含了哪些文件需要编译，哪些文件不需要编译，哪些文件需要先编译，哪些文件需要后编译，哪些文件需要重建等等。编译整个工程需要涉及到的，在 Makefile 中都可以进行描述。
> 
> ​ 换句话说，Makefile 可以使得我们的项目工程的编译变得自动化，不需要每次都手动输入一堆源文件和参数。

#### 2.Makefile 原理

**基本原理：若想生成目标, 检查规则中的所有的依赖文件是否都存在**

**当有依赖文件不存在**

- **如果有的依赖文件不存在, 则向下搜索规则, 看是否有生成该依赖文件的规则**
- **如果有规则用来生成该依赖文件,则执行规则中的命令生成依赖文件**
- **如果没有规则用来生成该依赖文件, 则报错**
![[Pasted image 20240705144926.png|608]]

**当所有依赖文件存在**

- **如果所有依赖都存在, 检查规则中的目标是否需要更新, 必须先检查它的所有依赖,依赖中有任何一个被更新, 则目标必须更新.(检查的规则是哪个时间大哪个最新)**

![[Pasted image 20240705144944.png|693]]

#### 3.为什么要使用Makefile

**以 `Linux` 下的 `C` 语言开发为例来具体说明一下，多文件编译生成一个文件，编译的命令如下所示：**

```bash
gcc bathroomLight.c mainPro.c socketContrl.c voiceContrl.c beep.c fire.c livingroomLight.c restaurantLight.c upstairsLight.c func.c mp3.c  -lwiringPi -lpthread -lsqlite3 -o smart
```

**`smart` 要生成的可执行程序的名字，后缀为`.c` 的都是源文件，`-l`后边是我们链接的库文件。**

**这是我们在 `Linux` 下使用 `gcc` 编译器编译 `C` 文件的例子。如果我们遇到的源文件的数量不是很多的话，可以选择这样的编译方式。如果像上面那样源文件非常的多的话，这样就会有以下不便。**

###### 解决编译时链库的不便

> 下面列举了一些需要我们手动链接的标准库：
> 
> 1. beep.c 用到了硬件驱动库 wiringPi中的函数，我们得手动添加参数 -lwiringPi；
> 2. mainPro.c 用到了小型数据库 SQLite 中的函数，我们得手动添加参数 -lsqlite3；
> 3. socketContrl.c 使用到了线程，我们需要去手动添加参数 -lpthread；因为有很多的文件，还要去链接很多的第三方库，所以在编译的时候命令会很长，并且在编译的时候我们可能会涉及到文件链接的顺序问题，所以手动编译会很麻烦。




​ **如果我们学会使用 Makefile 就不一样了，它会彻底简化编译的操作。把要链接的库文件放在 Makefile 中，制定相应的规则和对应的链接顺序。这样只需要执行 make 命令，工程就会自动编译，省略掉手动编译中的参数选项和命令，非常的方便。**

---

###### 提高编译效率，缩短编译时间（尤其是大工程）

​ **Makefile 支持多线程并发操作，会极大的缩短我们的编译时间，并且当我们修改了源文件之后，编译整个工程的时候，make 命令只会编译我们修改过的文件，没有修改的文件不用重新编译，也极大的解决了我们耗费时间的问题。**

​ **并且文件中的 Makefile 只需要完成一次，一般我们只要不增加或者是删除工程中的文件，Makefile 基本上不用去修改，编译时只用一个 make 命令。为我们提供了极大的便利，很大程度上提高编译的效率。**


# 一个规则
### 书写格式
目标：依赖条件
		（一个tab缩进）命令
```
//文件名只能是 makefile
hello:hello.cpp
        g++ hello.cpp ./lib/libmylib.a -o hello -I ./inc
```
![[Pasted image 20240705171142.png]]

### 为什么要这样写
在编写大项目时，有时只对部分源码进行修改，如果每次都全部编译一次会浪费大量的资源，而使用makefile文件则可以避免这些情况
```
hello:hello.o add.o sub.o div1.o
	g++ hello.o add.o sub.o div1.o -o hello


hello.o:hello.cpp
	g++ -c hello.cpp -o hello.o -I ./inc

add.o:add.cpp
	g++ -c add.cpp -o add.o

sub.o:sub.cpp
	g++ -c sub.cpp -o sub.o

div1.o:div1.cpp
	g++ -c div1.cpp -o div1.o

```
![[Pasted image 20240705175544.png]]
### 原理
***1. 目标的时间必须晚于依赖的时间，否则，更新目录***
***2. 依赖条件如果不存在，找寻新的规则去产生依赖***
![[Pasted image 20240705180158.png]]

### 规则
将makefile文件中的第一个规则，当作终极目标去完成，只要完成了这个规则就结束执行，而不去关心其他规则的情况
 
**==如何解决这个问题——ALL==**
![[Pasted image 20240705180831.png]]
# 两个函数和clean

```
src = $(wildcard *.cpp) 
``` 
匹配当前工作 目录下的所有.cpp文件。将文件名组成列表，赋值给变量src。  
找到当前目录下所有后缀为.cpp的文件，赋值给src  
  
```
obj = $(patsubset %.cpp,%.o, $(src))  
```
将参数3中，包含参数1的部分，替换成参数2  
把src变量里所有后缀为.c的文件替换成.o
```
src = $(wildcard *.cpp) # add.cpp sub.cpp div1.cpp hello.cpp
obj = $(patsubst %.cpp, %.o, $(src)) # add.o sub.o div1.o hello.o



ALL:hello


hello:$(obj)
        g++ $(obj) -o hello

hello.o:hello.cpp
        g++ -c hello.cpp -o hello.o -I ./inc

add.o:add.cpp
        g++ -c add.cpp -o add.o

sub.o:sub.cpp
        g++ -c sub.cpp -o sub.o

div1.o:div1.cpp
        g++ -c div1.cpp -o div1.o

clean:
        -rm -rf $(obj) hello
```

### clean函数
执行删除操作
```
make clean //运行clean命令
make clean -n //模拟clean命令，防止误删
-rm -rf $(obj) hello //- 表示报错依然执行
```
<mark style="background: #FF5582A6;">注意：</mark>
`make`命令不会执行clean操作
![[Pasted image 20240705194636.png]]


# 三个自动变量

1. `$@` ：在规则**命令中**，表示规则中的目标  
2. `$<` ：在规则**命令中**，表示规则中的第一个条件，如果将该变量用在模式规则中，它可以将依赖条件列表中的依赖依次取出，套用模式规则  
3. `$^` ：在规则**命令中**，表示规则中的所有条件，组成一个列表，以空格隔开，如果这个列表中有重复项，则去重
# 模式规则
```
%.o:%.cpp
        g++ -c $< -o $@ -I ./inc
```

### 静态模式规则
```
a.out:$(obj)
        g++ $^ -o $@
$(obj):%.o:%.cpp                          #指定obj
        g++ -c $< -o $@ -I ./inc
%.o:%.s
        g++ -c $< -o $@ -I ./inc
```
# 伪目标
```
.PHONY: clean ALL
```

**==原因==**
![[Pasted image 20240705203841.png]]


# 代码实现
```
src = $(wildcard *.cpp) # add.cpp sub.cpp div1.cpp hello.cpp
obj = $(patsubst %.cpp, %.o, $(src)) # add.o sub.o div1.o hello.o



ALL:a.out


a.out:$(obj)
        g++ $^ -o $@

$(obj):%.o:%.cpp
        g++ -c $< -o $@ -I ./inc

clean:
        -rm -rf $(obj) a.out                             
```
![[Pasted image 20240705201715.png]]

# 为什么要这样做?
方便后续扩展，当项目再增加新的源文件时，不需要对makefile文件在进行修改就可以直接进行编译
![[Pasted image 20240705202459.png]]

# 总结
![[Pasted image 20240705211213.png]]