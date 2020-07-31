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

- `ssh-keygen -t rsa -C youremail`: 用于产生两个ssh keys分别为私有的和共有的

![image-20200731081838927](C:\Users\LvGJ\AppData\Roaming\Typora\typora-user-images\image-20200731081838927.png)

- 找到生成的ssh文件，打开GitHub->setting->ssh keys->news ssh key，复制id_rsa.pub到里面，完成添加

- `git remote add origin git@github.com:yourname/responame `: 将这个本地仓库关联到GIthub上面的仓库。
- `git push -u origin master`: 把本地的文件推送到仓库的master分支中。之后就可以直接使用`git push origin master`完成推送
- `git clone git@github.com:yourname/responame`：使用这个命令可以克隆仓库有，此外也可以使用https地址
- `rm -rf 文件夹名字`：一般使用rm filename删除文件，但是对于文件夹这样不行，要使用-rf-r代表向下递归，-f代表强制删除

## 分支管理

### 创建与合并分支

- `git switch -c dev = git checkout -b dev = git branch dev+git checkout dev`：用于创建分支并且转到这个分支上，分支工作原理如下：

![image-20200731084740274](C:\Users\LvGJ\AppData\Roaming\Typora\typora-user-images\image-20200731084740274.png)

![image-20200731084752454](C:\Users\LvGJ\AppData\Roaming\Typora\typora-user-images\image-20200731084752454.png)

![image-20200731084759758](C:\Users\LvGJ\AppData\Roaming\Typora\typora-user-images\image-20200731084759758.png)

- 当这个时候我们切换分支会发现，在dev中改变的东西被删除了

<img src="C:\Users\LvGJ\AppData\Roaming\Typora\typora-user-images\image-20200731084835062.png" alt="image-20200731084835062" style="zoom:50%;" />

- 对他们进行合并，删除dev

![image-20200731084858005](C:\Users\LvGJ\AppData\Roaming\Typora\typora-user-images\image-20200731084858005.png)

- `git branch`可以查看有哪些分支

  ```
  例如，在readme.md上进行了更改
  添加了Creating a new branch is quick.
  
  再:
  git add readme.md
  git commit -m "branch test"
  等于是在dev分支上提交了变更
  使用：
  git switch master 切换
  会发现在dev分支中进行地更改不见了
  使用
  git merge dev
  将master和dev合并
  最后
  git branch -d dev
  删除dev
  ```

- `git switch master`：更换分支
- `git merge dev`：分支合并
- `git branch -d dev`：删除dev分支

### 无法自动合并，合并冲突

<img src="C:\Users\LvGJ\Documents\PostGraduate\Tool\image-20200731090656826.png" alt="image-20200731090656826" style="zoom:50%;" />

- 例如上述情况，在两个分支中都进行了更改（add+commit)，再使用`git merge feature1`会出现无法自动合并的提示

<img src="C:\Users\LvGJ\AppData\Roaming\Typora\typora-user-images\image-20200731090829542.png" alt="image-20200731090829542" style="zoom:67%;" />

- 使用`cat readme.md`可以查看内容发现，如下：

<img src="C:\Users\LvGJ\AppData\Roaming\Typora\typora-user-images\image-20200731090942393.png" alt="image-20200731090942393" style="zoom:50%;" />

- git使用>>>>>和<<<<<来进行标记不同分支的改变，这时候，需要重新打开readme.md 进行更改，并且add+commit完成合并
- `git log --graph --pretty=oneline --abbrev-commit`使用带参数的git log，可以查看合并情况：

<img src="C:\Users\LvGJ\AppData\Roaming\Typora\typora-user-images\image-20200731091202119.png" alt="image-20200731091202119" style="zoom:50%;" />

### 分支管理策略

- 使用fast forward删除分支后会丢失分支历史，使用--no-ff任然会保存分支信息
- `git merge --no-ff -m "add merge" dev`

<img src="C:\Users\LvGJ\AppData\Roaming\Typora\typora-user-images\image-20200731094028497.png" alt="image-20200731094028497" style="zoom:50%;" />

- 正常工作中不在master上操作，因为上面是稳定的，而是在dev上操作，最后同步到master上
<img src="C:\Users\LvGJ\AppData\Roaming\Typora\typora-user-images\image-20200731094227784.png" alt="image-20200731094227784" style="zoom:50%;" />

### bug分支

在进行开发的时候，比如我现在在dev上做开发，还没做完，这个时候突然收到，需要紧急修复一个bug，但是dev没有写完，肯定不能add+commit，该怎么办？

- `git stash`：可以把当前的分支存起来。
- `git switch -c issue-101`：可以创建一个新的分支，这个分支用来修复bug101，修复完成后add+commit
- 切换到master分支，对issue的分支就行合并，`git merge --no-ff -m "xxxx" branchname`
- 现在`git switch dev`重新切回到dev，可以直接使用`git stash pop`恢复前面的状态
- 但是dev是老版本，也存在这个bug所以也需要更新下，使用`git cherry-pick  分支号`
- `git stash list`显示所有的stash

- 已经提交的分支，强制删除使用`git branch -D xxx`

### 协作开发

- 使用`git push origin branchname`推送自己的分支
- 如过失败，则代表上面的更新，需要使用`git pull`从上面获取最新的，再手动合并冲突再使用git push

> 如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to  origin/`。

## 标签管理

### 创建标签

- `git tag v1.0`可以为当前版本添加标签
- 对于以前的提交的版本，可以使用`git tag v0.9 commitNUMber`进行创建
- `git tag`可以查看所有tag
- `git show tagID`可以展现改tag的详情信息
- `git tag -d tagId`删除
- `git tag -a v0.1 -m "comment" commitID`创建tag并且带有说明

### 操作标签

- `git push origin tagID`可以把指定版本发布到GIthub
- `git push origin --tags`推送全部标签
- 对与云端的tag删除，首先删除本地的，然后`git push origin :refs/tags/tagID`

## 自定义Git

### 忽略特殊文件

在进行使用git的时候，每当你添加一个文件他就提醒你，什么文件又untrack了，有点烦，解决方法是配置.gitignore文件，在这个文件中进行指定文件的格式，或者指定文件夹名字，进行忽略

- 首先`touch .gitignore`创建该文件，下一步，进行配置

```
# ignore all .a files
*.a

# but do track lib.a, even though you're ignoring .a files above
!lib.a

# only ignore the TODO file in the current directory, not subdir/TODO
/TODO

# ignore all files in any directory named build
build/

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory and any of its subdirectories
doc/**/*.pdf
```

- 具体需求，具体分析，配置好了之后，add+commit

- 这时候就完成创建了，但是当需要添加一个ignore文件的时候，会报错
- `git add app.class`，配置中又\*.class ，所以会被忽略，无法添加，这个时候要强制添加使用`git add -f app.class`
- `git check-ignore -v app.class`可以查看是属于哪一种ignore类型

### 配置别名

例如`git status`可以直接写成`git st`

- `git config --global alias.st status`就可以了

- `git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"`高端用法

- `cat .git/config`可以查看配置文件，删除其中[alias]下面的别名就可以删除了

  

  
