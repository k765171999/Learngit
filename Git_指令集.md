# Git 指令集

## 基础

- `git status` 查看状态
- `git diff readme.md`直接列举出两个不一样的地方
- `git add readme.md`添加到队列等待提交
- `git commit -m "xxx"`提交

## 查看或者回退

- `git log`用于查看commit的历史

![image-20200730221932569](C:\Users\LvGJ\AppData\Roaming\Typora\typora-user-images\image-20200730221932569.png)

> 可以使用`git log --pretty=oneline`来进行紧凑显示

- `git reset --hard HEAD^`版本回退HEAD\^是上一个版本，\^\^是上上个版本，前一百个版本是HEAD~100

![image-20200730222735391](C:\Users\LvGJ\AppData\Roaming\Typora\typora-user-images\image-20200730222735391.png)

![image-20200730222744716](C:\Users\LvGJ\AppData\Roaming\Typora\typora-user-images\image-20200730222744716.png)

- `git reset --hard  版本号`这个可以使得版本回退

![image-20200730222906811](C:\Users\LvGJ\AppData\Roaming\Typora\typora-user-images\image-20200730222906811.png)

> 版本号可以使用`git reflog`查看

- Git分为三个，工作区，缓冲区，分支，

> 添加一个文件是在工作区
>
> `git add` 之后是在缓冲区
>
> `git commit`之后到了分支

![image-20200730231531063](C:\Users\LvGJ\AppData\Roaming\Typora\typora-user-images\image-20200730231531063.png)

- `git`处理顺序是第一次修改 -> `git add` -> 第二次修改 -> `git add` -> `git commit`

- `git status`查看状态信息后，对于不想要的更改，可以用`git checkout -- filename`

- `git rm filename` ：对于以及add 和commit的文件，可以使用这个命令来进行删除，最后还需要commit一下

## 远程仓库

