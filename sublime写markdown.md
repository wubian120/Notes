# 使用Sublime Text 2 编辑Markdown #

## 一、安装  ##

下载Sublime Text 2
安装
## 二、安装Package Control ##

按Ctrl + ` 打开console
粘贴代码到console并回车
重启Sublime Text 2
```
import urllib2,os;pf='Package Control.sublime-package';ipp=sublime.installed_packages_path();os.makedirs(ipp) if not os.path.exists(ipp) else None;open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read())
```

## 三、安装Markdown Preview ##

按Ctrl + Shift + P
输入pci 后回车(Package Control: Install Package)
稍等... ^_^
输入Markdown Preview回车
## 四、编辑 ##

按Ctrl + N 新建一个文档
按Ctrl + Shift + P
使用Markdown语法编辑文档
语法高亮，输入ssm 后回车(Set Syntax: Markdown)
## 五、在浏览器预览Markdown文档  ##

按Ctrl + Shift + P
输入mp 后回车(Markdown Preview: current file in browser)
此时就可以在浏览器里看到刚才编辑的文档了

## 六、 如何写表格






