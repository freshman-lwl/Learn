## 1. Makefile简介
### (1) Makefile是什么?
在文件很多时，只用简单的gcc命令编译文件很麻烦，如：  
``` shell
gcc hello.c -o hello  
gcc aa.c bb.c cc.c dd.c ...
```
make工具和Makefile则可以批量处理复杂的项目文件。  
![Alt makefile作用](image.png "makefile作用")

### (2) make和Makefile是什么关系？
make工具:找出修改过的文件，根据依赖关系，找出受影响的相关文件，最后按照规则单独编译这些文件。

Makefile文件:记录依赖关系和编译规则。

### (3)怎么学习Makefile?
Makefile的本质:无论多么复杂的语法，都是为了更好地解决项目文件之间的依赖关系。  
[Makefile学习思维导图](.) ![Makefile学习思维导图](Makefile%E5%AD%A6%E4%B9%A0%E6%80%9D%E7%BB%B4%E5%AF%BC%E5%9B%BE.png "Makefile学习思维导图")
