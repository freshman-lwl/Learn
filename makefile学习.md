## 1. Makefile简介
### (1) Makefile是什么?
在文件很多时，只用简单的gcc命令编译文件很麻烦，如：  
``` shell
gcc hello.c -o hello  
gcc aa.c bb.c cc.c dd.c ...
```
make工具和Makefile则可以批量处理复杂的项目文件。  
![alt makefile作用](https://gitee.com/Embedfire-imx6/embed_linux_tutorial_ppt/blob/master/Linux%E8%A7%86%E9%A2%91%E6%95%99%E7%A8%8B%E8%AF%BE%E4%BB%B6%E4%B8%8E%E7%AC%94%E8%AE%B0/%E7%AC%AC30%E8%AE%B2%20Makefile%E7%AE%80%E4%BB%8B/makefile.drawio)

### (2) make和Makefile是什么关系？
make工具:找出修改过的文件，根据依赖关系，找出受影响的相关文件，最后按照规则单独编译这些文件。

Makefile文件:记录依赖关系和编译规则。

### (3)怎么学习Makefile?
Makefile的本质:无论多么复杂的语法，都是为了更好地解决项目文件之间的依赖关系。
