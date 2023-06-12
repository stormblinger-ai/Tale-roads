# win10配置

![image-20230116190830035](D:\工具\Typora\image\image-20230116190830035.png)

![image-20230116190858610](D:\工具\Typora\image\image-20230116190858610.png)

![image-20230116190949346](D:\工具\Typora\image\image-20230116190949346.png)

![image-20230116191023686](D:\工具\Typora\image\image-20230116191023686.png)

![image-20230116191047338](D:\工具\Typora\image\image-20230116191047338.png)

![image-20230116191117644](D:\工具\Typora\image\image-20230116191117644.png)

注：win10家庭版无组策略编辑器，可自己新建生成一个

## win10文件夹Bonjour删除

1,自己冒出来的文件，还删除不了，自动挂在多个进程上，里面主要的dll文件就是这个mdnsNSP.dll。用管理员打开cmd窗口查看pid

![image-20230122224018648](D:\工具\Typora\image\image-20230122224018648.png)

2,把这些全部关闭了，在删除即可

## 右键添加新建MD文件

新建text文件，把下面内容复制，然后另存为reg文件，双击运行即可，原理为修改注册表，也可以直接regedit,找到下面几个中括号的选项新建项手动修改

```
Windows Registry Editor Version 5.00
[HKEY_CLASSES_ROOT\.md]
@="Typora.md"
"Content Type"="text/markdown"
"PerceivedType"="text"
[HKEY_CLASSES_ROOT\.md\ShellNew]
"NullFile"=""
```

