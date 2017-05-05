# Intellij Idea 快捷键 笔记


- 常用快捷键
ctrl shift backspace 返回上次编辑位置
ctrl + E   最近的文件
ctrl shift E  最近更改的文件
shift + 左键单击 关闭文件

ctrl + P  显示参数

ctrl + f  查找
ctrl + shift + f  全局查找

alt + shift + f10   run
alt + shift + f9  debug

ctrl + r 替换

alt + 数字  快速切换 

alt + d   全屏

alt + 方向键 切换类文件

ctrl alt + insert 新建文件 (当前文件夹)

点击文件夹 alt + insert  新建文件

shift + esc +鼠标 隐藏
[IntelliJ Idea 常用快捷键列表](http://www.cnblogs.com/zhangpengshou/p/5366413.html)
[](http://blog.csdn.net/yangshijin1988/article/details/63262575)

[解决Intellij IDEA 通过archetype创建Maven项目缓慢的问题](http://www.cnblogs.com/lycsky/p/6144691.html)
- 生成项目目录结构速度慢  

1. 由于默认情况下，根据archetype创建maven项目会从网络下载catalog文件，导致创建maven项目缓慢
```
Searching for remote catalog: http://repo1.maven.org/maven2/archetype-catalog.xml
```

2. 解决办法可以设置使用本地catalog文件，在IDEA中设置archetype的使用方式为local;  
--default setting 
  --Build
    -- Maven 
      -- Runner
        -- VM Option
```
-DarchetypeCatalog=local
```

[archetype-catalog.xml](http://repo1.maven.org/maven2/archetype-catalog.xml)

根据 官网介绍，把下载的文件放到%userprofile%/.m2目录下即可。
```
C:\Users\Brady\.m2
```

## 查看 Java类字节码


+setting
  +Tools
    +External Tools

Name 

[Intellij idea快速查看Java类字节码](http://blog.csdn.net/qq_24489717/article/details/53837493)

