## git常用命令

#### 初始化git

```
	$ git config -global user.name <name>  #设置提交者名字
    $ git config -global user.email <email>  #设置提交者邮箱
    $ git config -global core.editor <editor>  #设置默认文本编辑器
    $ git config -global merge.tool <tool>  #设置解决合并冲突时差异分析工具
    $ git config -list  #检查已有的配置信息
```

#### 1、创建git版本库，把当前目录变成git可以管理的目录:

```
	$ git init
```

#### 2、把文件添加到仓库

```
 $ git add  .           #添加所有文件
 $ git add readme.md    #添加指定文件
 $ git add -u   #把所有tracked文件中被修改过或已删除文件的信息添加到索引库。它不会处理untracted的文件。
 $ git add -A   # 表示把中所有tracked文件中被修改过或已删除文件和所有untracted的文件信息添加到索引库。省略表示.,即当前目录。
```

查看被所有修改过或已删除文件但没有提交的文件的状态，进入一个命令系统：

```
 $ git add -i
          staged     unstaged path
  1:        +0/-0      nothing branch/t.txt
  2:        +0/-0      nothing branch/t2.txt
  3:    unchanged        +1/-0 readme.txt

*** Commands ***
  1: [s]tatus     2: [u]pdate     3: [r]evert     4: [a]dd untracked
  5: [p]atch      6: [d]iff       7: [q]uit       8: [h]elp
What now>
```
这里的t.txt和t2.txt表示已经被执行了git add，待提交。即已经添加到索引库中。
readme.txt表示已经处于tracked下，它被修改了，但是还没有被执行了git add。即还没添加到索引库中。

revert子命令：把已经添加到索引库中的文件从索引库中剔除。

update子命令：把已经tracked的文件添加到索引库中。

add untracked子命令：可以把还没被git管理的文件添加到索引库中。

diff子命令：可以比较索引库中文件和原版本的差异。

status子命令： 功能上和git add -i相似

quit子命令：用于退出git add -i命令系统

#### 3、把文件提交到仓库

```
 $ git commit -m 'commit a file'
```

`-m`是本次提交的说明,可以是任何内容，当然最好是有意义的说明。

#### 4、查看仓库当前的的状态

```
	$ git status
```

#### 5、查看修改过的内容

```
	$ git diff readme.md
```

#### 6、查看历史记录，包括提交信息，修改信息等等

```
	$ git log
```

如果嫌弃信息太多，可以使用`--pretty=oneline`来简化输出信息.
`3628164...882e1e0`这样的一堆信息是git的版本号（commit id），类似svn的版本号（1,2,3....）

查看指定文件的提交历史：

```
	$ git log -p   #查看提交历史并列出相关的文件
	$ git log -p <file> #查看指定文件的提交历史
	$ git blame <file> #以列表方式查看指定文件的提交历史
```


#### 7、版本回退命令（回滚，撤销）

回到过去版本,撤消工作目录中所有未提交文件的修改内容:

```
	$ git reset --hard HEAD^ 
```

`HEAD^`回退到上一个版本

`HEAD^^`回退到上上个版本

...

`HEAD~10`回退到上10个版本

回到未来的版本：

```
	$ git reset --hard 3628164fb26d48395383f8f31179f24e0882e1e0
```

版本号可以不用写全，前几位即可。可万一版本号忘记了咋办？

可以使用以下命令查看每一次命令操作：

```
	$ git reflog
```

另外的撤销命令：

```
	$ git checkout HEAD <file1> <file2>  #撤消指定的未提交文件的修改内容
    $ git checkout HEAD. #撤消所有文件
    $ git revert <commit>  #撤消指定的提交
```


#### 8、工作区和暂存区

工作区：你电脑里边看到的目录，平时操作就是操作的工作区

暂存区：git init初始化之后，会有一个隐藏的目录.git，这个就是暂存区，在工作目录下是一个隐藏文件夹（当然，你也可以设置为显示状态）

我们提交文件，只是提交到暂存区，不像svn是直接提交到服务器。


#### 9、丢弃工作区的修改

```
	$ git checkout -- readme.md
```

就是让这个文件回到最近一次git commit或git add时的状态。

**git checkout -- file命令中的--很重要**


场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，则考虑进行版本回退，不过前提是没有推送到远程库。


#### 10、文件操作

```
	 $ git mv <old> <new> 	#重命名文件
```


添加到版本库之前，文件管理器直接删除。

添加到版本库之后，如果想连同版本库里边的一起删除，可用命令：

```
	$ git rm readme.md
```

万一删除错了呢？版本库有，就直接`git checkout -- readme.md`,版本号没有？回收站找找，再找不到你就到厕所哭去吧。

```
	$ git rm --cached <file> #停止跟踪文件，但不删除
```

#### 11、远程仓库

远程仓库，可以是github，gitlab，也可以是你自己电脑，反正就是一个保存代码的地方。

要推送到远程仓库,首先得关联一个远程库：

```
	$ git remote add origin git@gitlab.com:mmzer/eDraft.git
```

origin 就是远程库的名字，也可以改成其它的。


把本地库的所有内容推送到远程库：

```
	$ git push -u origin master
```

命令含义：用git push命令，把当前分支master推送到远程库，`-u`参数可以第一此提交的时候加上，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。以后就可以省略`-u`了。

另外的一些命令：

```
	$ git remote -v  #查看远程版本库信息
    $ git remote show <remote>  #查看指定远程版本库信息
```

#### 12、拉取远程数据

克隆：

```
	$ git clone git@gitlab.com:mmzer/eDraft.git
```

更新数据：

```
	$ git fetch <remote>  #从远程库获取代码
    $ git pull <remote> <branch>  #下载代码及快速合并
```

#### 13、分支管理

主干：

![](0.png)

查看分支状态：

```
	$ git branch
```

创建新分支`dev`，并切换到分支`dev`:

```
	$ git checkout -b dev
```

相当于以下两条命令：

```
	# 创建分支
	$ git branch dev

	# 切换分支 
	$ git checkout dev
```

创建分支之后：

![](2.png)

在分支上进行了提交操作：

![](3.png)

合并分支：

```
	$ git merge dev
```

`git merge`命令用于合并指定分支到当前分支。

也可以查看分支合并情况：

```
	$ git log --graph --pretty=oneline --abbrev-commit

	*   59bc1cb conflict fixed
	|\
	| * 75a857c AND simple
	* | 400b400 & simple
	|/
	* fec145a branch test

```


**注意到上面的Fast-forward信息，Git告诉我们，这次合并是“快进模式”，也就是直接把master指向dev的当前提交，所以合并速度非常快。**

衍合：

```
	 $ git rebase <branch>  #衍合指定分支到当前分支
```

**【注意：】一旦分支中的提交对象发布到公共仓库，就千万不要对该分支进行衍合操作。**


删除分支：

```
	$ git branch -d dev
```

如果分支没有被合并，删除就会不成功，可以强行删除：

```
	$ git branch -D feature-vulcan
```

因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在master分支上工作效果是一样的，但过程更安全。


解决冲突：

Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容。

冲突只能手动编辑，或者利用图形化工具操作。


分支策略：

合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

```
	$ git merge --no-ff -m "merge with no-ff" dev
```

`--no-ff`表示强制禁用`Fast forward`模式.

团队合作分支策略：

![](4.png)

修复bug:

Git还提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

```
	$ git stash
```

查看工作现场：

```
	$ git stash list
```

恢复工作现场:

```
	$ git stash apply stash@{0} 
```

删除工作现场：

```
	$ git stash drop
```

多人协作：

```
	首先，可以试图用git push origin branch-name推送自己的修改；

		1. 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

		2. 如果合并有冲突，则解决冲突，并在本地提交；

 	    3. 没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！

	如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，
	用命令git branch --set-upstream branch-name origin/branch-name。
```

查看远程库信息:

```
	$ git remote -v
```

本地新建的分支如果不推送到远程，对其他人就是不可见的；

从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；

在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；

建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；

从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。


#### 14、标签管理

创建标签:

```
	- 命令git tag <name>用于新建一个标签，默认为HEAD，也可以指定一个commit id；

	- git tag -a <tagname> -m "blablabla..."可以指定标签信息；

	- git tag -s <tagname> -m "blablabla..."可以用PGP签名标签；

	- 命令git tag可以查看所有标签。
```

删除本地标签：

```
	git tag -d v0.1
```

删除远程标签：
```
	git push origin :refs/tags/v0.1
```


因为创建的标签都只存储在本地，不会自动推送到远程。

如果要推送某个标签到远程，使用命令`git push origin <tagname>`

一次性推送全部尚未推送到远程的本地标签：

```
	$ git push origin --tags
```

#### 参考链接：

[https://git-scm.com/docs](https://git-scm.com/docs)

[http://iissnan.com/progit/](http://iissnan.com/progit/)

[http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

[http://www.runoob.com/git/git-tutorial.html](http://www.runoob.com/git/git-tutorial.html)