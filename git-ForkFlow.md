## Git ForkFlow


1. 创建仓库

2. 本地新建目录 并添加文件
 a.md
 在本地  暂存  提交到本地 master   

3. 推送到 远端 origin 
```
git push -u origin master
```



* nihao 

 https://

https://brady-wu@bitbucket.org/brady-wu/testproject-p.git

## source tree 
拉取 pull  = fetch + merge
获取 fetch 

对于后者，“获取”的含义是命令git fetch，即从远程仓库抓取本地没有的修改；至于前者，大多数情况下，这里“拉取”的含义是git fetch紧接着一个git merge，对应git中的命令git pull,即从远程仓库抓取本地没有的修改并自动合并到远程分支。
由于git pull的结果有时会让我们看不懂，所以显式地使用fetch和merge命令会比较好一些。当然，对于一些简单的情况，前者git pull更方便一点。\

前面两个楼上已经说的很好了，我再用大白话说一下，拉取会把你本地仓库没有 而远程仓库有的更新写到你本地中，而获取的用处更多的是用来查看对于你本地仓库的状态来说远程仓库是否有更新，仅此而已，并不会使你的本地仓库发生改变


https://brady-wu@bitbucket.org/brady-wu/test2.git

bitbucket 新建项目 
test2 url:https://brady-wu@bitbucket.org/brady-wu/test2.git


fork 
clone  url：https://brady-wu@bitbucket.org/brady-wu/test2-fork.git


git remote add upstream https://brady-wu@bitbucket.org/brady-wu/test2.git




