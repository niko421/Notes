git 的主要作用是(个人感觉)当你每次修改代码或者对某个文件进行增删改查,可以对操作进行一个记录.(记住:每次需要进行add,和commit,不燃git没办法记录到).直到你突然想回到某个修改版本,就可以通过git回到你想要的版本.

branch就是在不影响master的情况下,复制一份master的记录到branch单独进行操作,如果修改满意了,就可以合并到master,不满意可以直接剔除掉branch,也不会影响master

##### git init 

把这个目录变成Git可以管理的仓库

##### git add ***

文件添加到仓库

##### git add . 

不但可以跟单一文件，还可以跟通配符，更可以跟目录。一个点就把当前目录下所有未追踪的文件全部add了 

##### git commit -m "first commit" 

把文件提交到仓库

##### git remote add origin https://github.com/niko421/LJX_projects.git

关联远程仓库 后面加自己的仓库地址

如图:

![](C:\Users\86189\Desktop\笔记\GIt\imgs\gitSSH.png)

 如果当前分支与多个主机存在追踪关系，则可以使用-u选项指定一个默认主机，这样后面就可以不加任何参数使用git push。 

##### git reset --hard  9dc8f8f56034cd199db2693621b4bd85488aa304

回滚到之前的版本.git reset --hard 后面的代码需要通过 git reflog 查看

##### git reflog 

查看回滚之前的commit信息

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