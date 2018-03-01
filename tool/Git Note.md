# Git Note

#### git 三种状态:

状态|说明
----|----
Modified|操作在工作区中，当修改过工作区中的内容，未做任何提交时的状态
Staged| 通过add方法，将修改的内容提交保存到暂存区
commited| 通过commit将修改内容提交保存到本地库，

git的流程图：

```sequence
workDirectory->index:  git add filename
index->resposition: git commit filename
resposition->remote resposition: git push
workDirectory->resposition: git commit -a -m "log"
remote resposition-->resposition: git fetch
resposition-->workDirectory: git merge
remote resposition-->workDirectory: git pull
```

#### git 查看日志：

命令 | 用途
----|----
git log | 显示提交的日志
git log -p -n | 显示最近的提交的n条日志
git log --stat | 显示提交的简略统计
git log --pretty=oneline/shot/full/fuller  | 显示日志的格式
git log --since time | 显示指定时间之后提交的日志
git log --until time | 显示指定时间之前的提交日志
git log --author str | 仅显示指定作者的相关提交记录
git log --grep str | 仅显示指定关键字的提交记录   
    
    
查看当前状态：git status

在工作区有修改，但并未存储到缓存区(需要通过git add 命令添加到暂存区)

```
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   README.txt

no changes added to commit (use "git add" and/or "git commit -a")
```


将修改提交到了暂存区，但还未提交到本地库

```
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	modified:   README.txt
```

修改存入到了本地库,git就干净了

```
On branch master
nothing to commit, working tree clean
```

#### git 对比

git diff file 工作做区内容和暂存区内容比较

```
diff --git a/README.txt b/README.txt
index dc59844..0c11ddd 100644
--- a/README.txt
+++ b/README.txt
@@ -10,4 +10,5 @@ test git checkout cmd111.

 correct in resposition     
 correct in Staged          
+correct in working directory   //修改的位置(工作区和暂存的修改位置)
```

git diif --cached file 比较暂存区和本地库内容差异

```
diff --git a/README.txt b/README.txt
index 5973780..dc59844 100644   //index 表示暂存区
--- a/README.txt
+++ b/README.txt
@@ -9,4 +9,5 @@ test 11git checkout cmd FOR CONFILT.
 test git checkout cmd111.

 correct in resposition
+correct in Staged
```

git difftool 用对比工具比较工作区和暂存区的差异，可以加入--cached 比较暂存区和本地库内容差异


#### 撤销操作
git commit --amend: 会覆盖上一次操作


git reset 操作

操作方法|用途
----|----
git reset -- soft hash | 移动HEAD指针到对应hash码版本，操作在本地库。
git rest --mixed hash | 将暂存区和本地库内容设置到hash对应的版本,操作本地库、暂存区
git reset --hard hash | 将本地库、暂存区、工作区内容设置到hash对应版本，操作本地库、暂存区、工作区
git rest file | 命令默认为--mixed，将本地库、暂存区中的file文件各自回退到上一个状态

checkout 操作

操作方法|用途
----|----
git checkout branch | 切换分支，修改了将HEAD指向branch分支
git checkout file | 将暂存区的内容覆盖到工作区


#### 远程操作

命令|用途
----|----
git remote|查看远程仓库简写名称
git remote -v|查看远程仓库简写名称对应的url
git remote add shortname url|添加一个远程仓库，shortname为远程仓库的简写
git fetch shortname branch|从远程库branch分支拉取内容到本地仓库，但是不会合并到工作目录，如果需要合并的工作目录可以用git merge
git pull shortname branch|从远程库branch分支拉取内容到工作目录，想当于 git fetch shortname branch  git merge 两个命令
git remote show|显示远程仓库信息
git remote rename orginalname newname|重命名远程仓库


#### 分支操作

命令|用途
----|----
git branch branchname | 创建一个分支
git checkout -b branchname | 创建一个分支，并且切换到该分支上
git branck -d branchname | 删除分支
git merge branch | 合并分支
git mergetool | 启用可视化工具解决合并冲突
git branck | 列出所有分支
git branck -v | 列出所有分支，显示各个分支的最后一次提交
--merged | 分支列表中只显示已经合并的分支
--no-merged | 分支列表中只显示未合并的分支

#### 基变
分支的合并方式：

1.通常使用git merge branchname 操作合并分支，通常git会将两个分支进行对比，然后将其合并成一个分支

2.基变，git先找出两个分支最近的共同祖先，对比该祖先的历次提交，形成一个零时文件，然后将当前分支指向目标分支，最终将比较的文件运用到目标分支上

git rebase targetname: 合并分支，targetname为合并之后的那个分支，即为目标分支

git rebase --onto targetbranch comparebranch mergebrach: comparebranch 和 mergebranch有共同的祖先，现在将mergebranch分支合并到targetbranch分支上，而不合并comparebranch分支。将mergebranch分支和comparebranch、mergebranch俩分支的共同祖先进行对比，而后将其合并到targebranch上


#### github配置
创建ssh key,先查看~/.ssh/目录下面是否有ssh key，带有pub的为公钥，另外一个为私钥

```
➜  ~ cd ~/.ssh/
➜  .ssh ls
id_rsa      id_rsa.pub  known_hosts
➜  .ssh
```

如果该目录下没有key，则需要创建一个公钥，方法如下

```
ssh-keygen -t rsa -C "your_email"
```

然后查看公钥的hash值，如下:

```
➜  .ssh cat id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC22BKlb4TslA29QJP0TJ9YT2gQ2G+6l45cwMsS1xcHzLky514SQ3MtgXSmoy7pMf+qiDJPt5agcFT2HL0tAc/aMEUeIL70EVETMKEI+L0USj7Q4Kw80E0CHlFlWVfJQEwC/RKb15oX/Vda2ZvWj5ltbfS5zGnoR+0a0KrX3Kgfa7PH+FO4YtN97dknG1pk6v0euuApzPU3vLa+wmaBVBdJBu+jxr87AihUpytM+czIs8Ls3PwketF/4Zc7RhF9ez0UcCx41OiBwPuzdLOeWclgzu+LJj16tLlZjW5KcxnwHW5SXtcUeAioD4yHqElNmrnr2IzHXrY4D4Rm+Ptjyf8z tryingEv@Outlook.com
➜  .ssh
```

其后将该hash值粘贴到github下的Settings--->SSH and GPG keys--->New SSH key







    
    

    




