git 的主要作用是(个人感觉)当你每次修改代码或者对某个文件进行增删改查,可以对操作进行一个记录.(记住:每次需要进行add,和commit,不燃git没办法记录到).直到你突然想回到某个修改版本,就可以通过git回到你想要的版本.

branch就是在不影响master的情况下,复制一份master的记录到branch单独进行操作,如果修改满意了,就可以合并到master,不满意可以直接剔除掉branch,也不会影响master

### 创建版本库

##### git init 

把这个目录变成Git可以管理的仓库

##### $ cd

切换到指定目录

![](C:\Users\86189\Desktop\笔记\GIt\imgs\gitPwd.png)

##### $ pwd

查看当前目录

### 把文件添加add和提交commit到版本库

 Git 本地数据管理，大概可以分为三个区：

- 工作区（Working Directory）：是可以直接编辑的地方。
- 暂存区（Stage/Index）：数据暂时存放的区域。
- 版本库（commit History）：存放已经提交的数据。

##### git add ***

工作区文件添加暂存区

##### git add . 

不但可以跟单一文件，还可以跟通配符，更可以跟目录。一个点就把当前目录下所有未追踪的文件全部add了 

##### git commit -m "first commit" 

暂存区文件提交到版本库

### 版本控制

##### git log --pretty= oneline	

加参，简洁查看

##### git reflog	

查看每一次修改历史

#####  cat test.txt	

查看文件内容

#####  git status	

查看工作区中文件当前状态

##### git checkout -- test.txt(需要先add 不然git识别不到文件)	

丢弃工作区的修改，即撤销修改(那为什么不用Ctrl Z)

##### git reset HEAD test.txt	

丢弃暂存区的修改（若已提交，则回退）

##### git reset --hard  9dc8f8f56034cd199db2693621b4bd85488aa304

回滚到之前的版本.git reset --hard 后面的代码需要通过 git reflog 查看

### 删除文件

#####  rm test.txt

删除工作区

##### git rm -f test.txt

从磁盘和暂存区中同时删除

 如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 -f（译注：即 force 的首字母）。 这是一种安全特性，用于防止误删还没有添加到快照的数据，这样的数据不能被 Git 恢复。 

##### git rm --cached

   删除了暂存区和版本库的文件 ，但保留工作区的文件 

### 远程仓库

##### git remote add origin https://github.com/niko421/LJX_projects.git

关联远程仓库 后面加自己的仓库地址

如图:

![](C:\Users\86189\Desktop\笔记\GIt\imgs\gitSSH.png)

 如果当前分支与多个主机存在追踪关系，则可以使用-u选项指定一个默认主机，这样后面就可以不加任何参数使用git push。 

##### git remote -v        

查看远程仓库信息

##### git remote rm origin	

删除远程仓库（解绑）

##### $ git clone https://gitee.com/niko55/note.git

克隆远程仓库

### 多人协作

##### git branch  ***  

新建分支

##### git branch   

查看分支

#####  git branch -D + 分支名称的命令 

删除分支

##### git checkout  ***  

切换到指定分支

##### git merge  ***   

需要切换到master分支上,将指定分支合并到master分支

##### git push (需要先初始化和关联远程仓库)

git push 使用本地的对应分支来更新对应的远程分支


$ git push <远程主机名> <本地分支名>:<远程分支名>

注意: 命令中的本地分支是指将要被推送到远端的分支，而远程分支是指推送的目标分支，即将本地分支合并到远程分支。本地分支不固定 可以是新建的分支 ,不一定是master分支,可以在master下面新建一个分支test,在执行git push -u origin  test ,远程仓库就会在master新增一个test分支  

如果省略远程分支名，则表示将本地分支推送与之存在”追踪关系”的远程分支(通常两者同名)，如果该远程分支不存在，则会被新建。

- 
  $ git push origin master


上面命令表示，将本地的master分支推送到origin主机的master分支。(把本地库的所有内容推送到远程库上)如果后者不存在，则会被新建。

origin是一个远程厂库地址。

- $ git push origin : master 等同于$ git push origin --delete master


如果省略本地分支名，则表示删除指定的远程分支，因为这等同于推送一个空的本地分支到远程分支，这条命令是删除远程master分支。

上面命令表示删除origin主机的master分支。

- $ git push origin

如果当前分支与远程分支之间存在追踪关系（即分支名相同），则本地分支和远程分支都可以省略。

上面命令表示，将当前分支推送到origin主机的对应分支。

如果当前分支只有一个追踪分支，那么主机名都可以省略。


$ git push -u origin master

如果当前分支与多个主机存在追踪关系，则可以使用-u选项指定一个默认主机，这样后面就可以不加任何参数使用git push。



#####  **git pull**  (需要先初始化和关联远程仓库)

 命令格式如下：


git pull <远程主机> <远程分支>:<本地分支>

- 
  git pull origin master:my_test


上面的命令是将origin厂库的master分支拉取并合并到本地的my_test分支上。

- git pull origin master

如果省略本地分支，则将自动合并到当前所在分支上。如下：

##### 解决合并冲突

[](https://wenku.baidu.com/view/6c2b4047551252d380eb6294dd88d0d233d43ca1.html)

#####  git config --global http.proxy 

查看代理

#####  git config --global --unset http.proxy 

取消代理