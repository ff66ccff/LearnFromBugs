# Ubuntu下从零开始搭建环境
安装vscode,进入扩展商店安装C/C++ extensions pack,然后打开终端,输入  
```
sudo apt install gcc
```
然后新建一个main.c  
```
#include <stdio.h>
int main(){
    printf("hello world");
    return 0;
}
```
运行后成功输出 hello world