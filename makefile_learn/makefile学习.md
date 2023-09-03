## 1. Makefile简介
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

## 2. Makefile三要素

### (1) Makefile三要素是什么?

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

但是可以用**.PHONY**指定伪目标，此时将忽略targets是否存在，直接运行commands。
>.PHONY:target  

