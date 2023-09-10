## 1. GCC
### (1) 概念
GCC：原名GNU C语言编译器，是由GNU开发的编程语言译器，只能处理C语言。但其很快扩展，变得可处理C++，后来又扩展为能够支持更多编程语言，如Fortran、Pascal、Objective -C、Java、Ada、Go以及各类处理器架构上的汇编语言等，所以改名GNU编译器套件（GNU Compiler Collection）。

GCC不仅仅是个本地编译器，它还能跨平台交叉编译。例如，ARM-GCC是针对arm平台的一款编译器，它是GCC编译工具链的一个分支。
### (2) gcc与g++
GCC编译器通常以**gcc命令**的形式在终端（Shell）中使用。实际上，只要是 GCC 支持编译的程序代码，都可以使用 gcc 命令完成编译。可以这样理解，gcc 是 GCC 编译器的通用编译指令，因为根据程序文件的后缀名，gcc 指令可以自行判断出当前程序所用编程语言的类别。

g++是GCC中的GUN C++ Compiler（C++编译器），如果使用 g++ 指令，则无论目标文件的后缀名是什么，该指令都一律按照编译 C++代码的方式编译该文件。

要注意，C++ 标准对代码书写规范的要求更加严格，可能某些不规范的 C 语言代码用gcc可以正常编译，而用g++就会报错。另外，使用g++编译文件时，g++会自动链接标准库STL，而gcc不会自动链接STL。

### (3) 用法与选项
GCC最基本的用法是∶gcc [options] [filenames]

常用选项如下所示： 
``` shell
-o:产生目标（.i/.s/.o/可执行文件.out等）  
-c:通知gcc取消链接步骤，即编译源码并最终生成目标文件->.o  
-E:只运行C预处理器 ->.i  
-S:告诉编译器产生汇编文件后停止编译->.s  
-Wall:使gcc对源文件代码有问题的地方发出警告  
-Idir:后面直接接dir路径，将dir目录加入**搜索头文件**的目录路径  
-Ldir:后面直接接dir路径，将dir目录加入**搜索库**的目录路径  
-llib:链接lib库，lib是库文件目录，包含了所有对系统有用的库文件；库文件是应用程序、命令或进程正确执行所需要的文件。 
-g:在目标文件中嵌入调试信息，以便gdb之类的调试程序调试。
```

选项应用示例：
```shell
gcc -E hello.c -o hello.i #预处理
gcc -S hello.i -o hello.s #编译
gcc -c hello.s -o hello.o #汇编
gcc hello.o -o hello #链接
gcc hello.c -o hello #直接编译链接成可执行目标文件
gcc -c hello.c 或 gcc -c hello.c -o hello.o #编译生成可重定位目标文件
```

### (4) 编译过程
使用gcc把C文件编译成可执行文件可分为四步：预编译、编译、汇编、链接。如下所示：
 ![编译过程](image.png "编译过程")
* 预处理， C 编译器对各种预处理命令进行处理，包括头文件包含、宏定义的扩
展、条件编译的选择等；
* 编译，将预处理得到的源代码文件，进行“翻译转换”，产生出机器语言的目标程序，得到机器语言的汇编文件；
* 汇编，将汇编代码翻译成了机器码，但是还不可以运行；
* 链接，处理可重定位文件，把各种符号引用和符号定义转换成为可执行文件中的合适信息，通常是虚拟地址。

 #### (i) 预处理
预处理器cpp(也称预编译器，c语言pre-proccessor)把源文件和相关的头文件（如stdio.h）预编译成一个.i的文件。

作用：头文件展开，宏替换，注释去掉。

执行的命令：  
``` shell
gcc -E hello.c -o hello.i
```
 #### (ii) 编译
编译器（ccl，c语言compile）把预处理后的文件进行语法分析、语义分析以及优化后生成汇编代码文件。

作用：C文件变成汇编文件（.s文件）。

执行的命令：  
``` shell
gcc -S hello.i -o hello.s
```
 #### (iii) 汇编
 汇编器（as，assembler）把汇编代码文件转换成中间目标文件（二进制文件，即机器码）。此时还不可以运行。

 执行的命令：  
``` shell
gcc -c hello.s -o hello.o
```
 #### (iv) 链接
链接器（ld，GNU Linker,loader）把目标文件与所需要的所有的附加的目标文件(如静态链接库、动态链接库)链接起来成为可执行的文件。

执行的命令：  
``` shell
gcc hello.o -o hello(or hello.out)
```
**链接可分为动态链接.so和静态链接.a**：
* 动态链接使用动态链接库进行链接，生成的程序在执行的时候需要加载所需的动态库才能运行。**动态链接生成的程序小巧**，但是必须依赖动态库，否则无法执行。
* Linux下的动态链接库实际是共享目标文件（shared object），一般是.so文件，作用类似于 Windows 下的.dll 文件。程序**在运行的时候才去链接共享库的代码**，多个程序共享使用库的代码。
* 静态链接使用静态库进行链接，生成的程序包含程序运行所需要的全部库，可以直接运行，不过体积较大。
* Linux 下静态库是汇编产生的.o 文件的集合，一般以.a 文件形式出现。
* gcc默认是动态链接，加上-static 参数则采用静态链接。


## 2. Makefile简介
### (1) Makefile是什么?
在文件很多时，只用简单的gcc命令编译文件很麻烦，如：  
``` shell
gcc hello.c -o hello  
gcc aa.c bb.c cc.c dd.c ...
```
make工具和Makefile则可以批量处理复杂的项目文件。  
![Alt makefile作用](makefile%E4%BD%9C%E7%94%A8.png "makefile作用")

### (2) make和Makefile是什么关系？
make工具:找出修改过的文件，根据依赖关系，找出受影响的相关文件，最后按照规则单独编译这些文件。

Makefile文件:记录依赖关系和编译规则。

### (3)怎么学习Makefile?
Makefile的本质:无论多么复杂的语法，都是为了更好地解决项目文件之间的依赖关系。  
![Makefile学习思维导图](Makefile%E5%AD%A6%E4%B9%A0%E6%80%9D%E7%BB%B4%E5%AF%BC%E5%9B%BE.png "Makefile学习思维导图")

## 3. Makefile格式与执行规则

### (1) Makefile三要素

目标、依赖、命令  
在执行`make`命令后，makefile文件会先去执行依赖文件所对应的命令，最后再执行目标文件所对应的命令。

### (2) makefile格式
> 目标:依赖的文件，或者是其他目标。  
\<tab>命令1  
\<tab>命令2  
\<tab>...  

例如：  
``` makefile
targeta:targetb targetc
    echo "targeta"
targetb:
    echo "targetb"
targetc:
    echo "targetc"
```
直接执行`make`后，输出如下：
```shell
echo "targetb"
targetb
echo "targetc"
targetc
targeta
```
也可以`make`指定目标，例如
```shell
make targetc
#输出如下
echo "targetc"
targetc
```
如果目标文件已经存在当前目录下，则`make`命令不会再去执行目标文件下的命令，会提示当前文件已经是最新。  

但是可以用**PHONY**指定伪目标，此时将忽略targets是否存在，直接运行commands。
>.PHONY:target  

### (3) 执行规则：
GNU的make工作时的执行步骤

　　（1）读入所有的Makefile

　　（2）读入被include的其它Makefile

　　（3）初始化文件中的变量

　　（4）推导隐晦规则，并分析所有的规则

　　（5）为所有的目标文件创建依赖关系链

　　（6）根据依赖关系，决定哪些目标重新生成

　　（7）执行生成命令

　　定义在Makefile中的目标可能会有很多，但是**第一条规则中的目标将被确立为最终的目标**。如果第一条规则中的目标有很多个，那么，**第一个目标会成为最终的目标。**

`make`会调用 shell 去执行每一条指令。需要注意的是，即便在同一个目标下，每一条指令都是相互独立的。 也就是说 `make` 会分别调用多个shell去执行每一条指令，而非使用一个shell进程按顺序执行所有的命令。

## 4. makefile的变量和模式规则
### (1) 系统变量：
不常使用，记住几个常用的即可
``` makefile
CC=gcc #用于指定编译器的类型
CPPFLAGS: C预处理的选项 如：-I
CFLAGS：C编译器的选项 -Wall -g -c
LDFLAGS：链接器选项 -L -I
AS：汇编器
MAKE：make工具
SRC_FILES变量：用于指定源文件列表
OBJ_FILES变量：用于指定目标文件列表
```

### (2) 自定义变量：
#### (i)延迟赋值=
=，延迟赋值，自定义变量只有在被引用时才会被赋值。与在文中位置无关，系统自动推导将**最终的赋值**作为该变量的值。例如：
``` makefile
A=123
B=$(A)
A=456

.PHONY:all

all:
    echo "$(B)"
```
执行`make`后，输出为：
```shell
echo "456" #打印echo语句
456 #echo语句打印变量B的值
```
#### (ii)立即赋值:=
:=，立即赋值，变量会被立即赋值。也称覆盖式赋值，假如某变量在前面已经定义赋值过，则**将本次赋值作为最新的变量值**。（注意：该符号赋值只能推导该符号位置之前的值）

例如，修改B的赋值方式为`B:=$(A)`

执行`make`后，输出为：
```shell
echo "123" #打印echo语句
123 #echo语句打印变量B的值
```

#### (iii)空赋值?=
?=，空赋值，当某变量前面已经定义赋值过，则不执行本次定义赋值，否则执行本次赋值。例如：

``` makefile
A=123 #或 A?=123
A?=456

.PHONY:all

all:
    echo "$(A)"
```
执行`make`后，输出为：
```shell
echo "123" #打印echo语句
123 #echo语句打印变量A的值
```

#### (iv)追加赋值+=
+=，追加赋值，旧值保持不变，将新值黏贴到旧值后面（每个值之间自动加空格）。例如：

``` makefile
A?=123
A+=456

.PHONY:all

all:
    echo "$(A)"
```
执行`make`后，输出为：
```shell
echo "123 456" #打印echo语句
123 456 #echo语句打印变量A的值
```

### (3) 自动化变量：
$@：表示规则的**目标文件名**。如果目标是一个文档文件（Linux 中，一般成 .a 文件为文档文件，也成为静态的库文件），那么它代表这个文档的文件名。在多目标模式规则中，它代表的是**触发规则被执行的文件名**。

$<：表示规则的**第一个依赖的文件名**。如果是一个目标文件使用隐含的规则来重建，则它代表由隐含规则加入的第一个依赖文件。

\$^：代表的是**所有依赖文件**列表，使用空格分隔。如果目标是静态库文件，它所代表的只能是所有的库成员（.o 文件）名。一个文件可重复的出现在目标的依赖中，变量“\$^”只记录它的第一次引用的情况。就是说变量“\$^”会去掉重复的依赖文件。

### (4) 变量应用实例：
在习惯上，变量名最好是以大写字母为主
``` makefile
CC=gcc
TARGET=mp3
OBJS=main.o mp3.o

$(TARGET):$(OBJS)
    $(CC) $^ -o $@ 

main.o:main.c
    $(CC) -c main.c -o main.o

mp3.o:mp3.c
    $(CC) -c mp3.c -o mp3.o

.PHONY:clean

clean:
    rm mp3
```
### (5) 模式匹配
%:匹配任意多个非空字符，类似于shell中的通配符\*。例如：
``` makefile
CC=gcc
TARGET=mp3
OBJS=main.o mp3.o

$(TARGET):$(OBJS)
    $(CC) $^ -o $@ 

%.o:%.c
    $(CC) -c $< -o $@

.PHONY:clean

clean:
    rm mp3
```
执行`make`后，自动编译所有文件。

### (6) 默认规则
.o文件默认使用当前目录下的.c文件来进行编译。

### (7) export变量
* 在（parent，上层的）makefile中export出来变量，子makefile（sub make）中，是可以访问的。

* 而同一级别的makefile（可通过makefile中内置变量MAKELEVEL查看得知当前makefile的levlel），是无法通过export来传递变量的，即一个makefile中export出来一个变量，同一级的另外一个makefile中，是无法访问/得到的。

* makefile中的export是导出变量到子makfile，而目标对应执行的动作中的export，是属于shell中的export，其作用是导出变量到当前shell。此两个export的作用是不同的。

## 5. makefile的条件语句与函数
### (1) 条件语句
#### (i)ifeq和ifneq
ifeq和ifneq分别表示等于和不等于的条件判断语句，用法如下：
```makefile
ifeq ($(VARIABLE),value)
    ...
else
    ...
endif

ifneq ($(VARIABLE),value)
    ...
else
    ...
endif
```
其中，$(VARIABLE)是需要被判断的变量名，value是需要和变量比较的值(若value省略，代表value为空)。如果判断是正确的，则执行第一组命令；否则执行第二组命令。

#### (ii)ifdef和ifndef
ifdef 和 ifndef 用于判断变量是否被定义，用法如下：
``` makefile
ifdef VARIABLE
    ...
else
    ...
endif

ifndef VARIABLE
    ...
else
    ...
endif
```
如果变量VARIABLE被定义，则执行第一组命令；否则执行第二组命令。

### (2) 常用函数
在makefile中所有的函数都是有返回值的。

`wildcard`：查找指定目录下的指定类型的文件  
例如：`src=$(wildcard *.c)`//找到当前目录下所有后缀为.c的文件，赋值给src。

`patsubst`：匹配替换  
格式：`$(patsubst  pattern， replacement,text)`  
例如：`obj=$(patsubst %.c,%.o,(dir))`//把dir变量里所有后缀为.c的文件替换成.o