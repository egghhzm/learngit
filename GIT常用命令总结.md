# Git常用命令总结


### 本地创建GIT库

```
# 在任何目录下均可创建git库
$ git init

```

### 添加文件到GIT库,例如readme.txt，添加到临时区域
```
$ git add readme.txt
```

### 提交GIT文件，提交版本库

```bash
$ git commit -m "Initial commit"
```
### 查看当前GIT状态
```
$ git status

```
### 比较修改文件和仓库文件的区别
```
$ git diff readme.txt
```

### 查看整个GIT的提交历史
```
$ git log
# 单行查看
$ git log --pretty=oneline
```
### 回退版本

```
# 回退到上个版本 HEAD表示最后一个提交的版本, 最后一个版本的上个版本用 ^ 表示
# HEAD 为一个指向版本的指针

git reset --hard HEAD^

# 回退到前100个版本
git reset --hard HEAD^100

# 回退到某个版本
git reset --hard 3628164

```
### 查看命令历史

```
git reflog

```

### 放弃工作区修改
```
# 当修改工作区readme.txt文件，需要放弃修改执行以下命令
git checkout -- readme.txt
```
### 放弃提交修改，只要没有push到远程库中，均可放弃和回滚
```
git reset HEAD readme.txt
```

### 删除错误的提交(错误的提交了某个工作区中不该提交的文件，并想从本地版本库中删除)
```
# 先删除再提交
git rm readme.txt
git commit -m "remove readme.txt"
```
### 错误的删除了工作区文件，可以从版本库中找回
```
git checkout -- readme.txt
```

### 用SSH方式连接远程仓库,并推送本地库到远程库
1. 产生公钥
```
ssh-keygen -t rsa -C "zm.he@qq.com"

```
默认在windows下放在`` C:\Users\Administrator\.ssh``目录中：
其中``id_rsa`` 为私钥 ``id_rsa.pub``为公钥
2. 将公钥放在一个云端仓库中，比如：
[GitHub](https://github.com/)
[码云](git.oschina.net)
[coding](https://coding.net/user)
3. 在云端建立一个仓库
4. 将本地仓库添加到云端
```
# ssh方式
git remote add origin git@github.com:egghhzm/learngit.git
# http方式
git remote add origin https://github.com/egghhzm/learngit.git
```
然后用 ``push ``命令来将本地库推送到远端，第一次推送需要加上 ``-u``参数 

**Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令**
```
git push -u origin master
```
5. 以后就可以不需要加上``-u``参数了
```
git push origin master
```
### 从远程库克隆到本地仓库(多采用此种方式)
1. 首先需要在公有仓库上建立一个新的仓库，比如gitHub，osChina等等。
2. 克隆到本地
```
#本地自动建立文件夹
git clone git@github.com:egghhzm/learngit.git
git clone https://github.com/egghhzm/learngit.git
#两种方式均可

```
3. 其他操作与上一节相同

### GIT分支

1. 查看分支 `` git branch``

2. 创建分支 **dev**  `` git branch dev``

3. 切换分支 `` git checkout dev``

4. 创建+切换分支：``git checkout -b dev``

5. 合并某分支到当前分支：``git merge dev``  在master分支上合并dev分支，先切换到``master``分支上
6. 删除分支 `` git branch -d dev ``

#### 分支管理

1. 合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。
2. 采用命令参数禁用Fastforward方式
```
 git merge --no-ff -m "merge with no-ff" dev
```

2. 推送分支 `` git push origin master``
3. 查看远程 `` git remote -v ``

**并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？**

* ``master``分支是主分支，因此要时刻与远程同步；

* ``dev``分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；

* ``bug``分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；

* ``feature``分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

**总之，就是在Git中，分支完全可以在本地自己藏着玩，是否推送，视你的心情而定！**

**多人协作的工作模式通常是这样：**
1. 首先，可以试图用 `` git push origin branch-name`` 推送自己的修改;

2. 如果推送失败，则因为远程分支比你的本地更新，需要先用 ``git pull `` 试图合并；

3. 如果合并有冲突，则解决冲突，并在本地提交；

4. 没有冲突或者解决掉冲突后，再用 ``git push origin branch-name`` 推送就能成功！

如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令 ``git branch --set-upstream branch-name origin/branch-name`` 。

这就是多人协作的工作模式，一旦熟悉了，就非常简单。

**小结**

* 查看远程库信息，使用 ``git remote -v``；

* 本地新建的分支如果不推送到远程，对其他人就是不可见的；

* 从本地推送分支，使用 ``git push origin branch-name``，如果推送失败，先用git pull抓取远程的新提交；

* 在本地创建和远程分支对应的分支，使用` `git checkout -b branch-name origin/branch-name``，本地和远程分支的名称最好一致；

* 建立本地分支和远程分支的关联，使用 ``git branch --set-upstream branch-name origin/branch-name``；

* 从远程抓取分支，使用 ``git pull``，如果有冲突，要先处理冲突。

### 标签管理

1. 切换到需要打标签的分支下： 
``` 
#显示分支
git branch 
git checkout <branch-name> //例如master
git tag v0.1.0
```
2. 查看标签 `` git tag ``
3. 如果需要对某个提交版本打标签，需要先查看某个标签的具体id
```
$ git log --pretty=oneline --abbrev-commit

c5421b7 conflict fixed
ffa5efc & simple
4a2a2e9 AND simple
a206a3c branch test
2aa838a remove test.txt
8e9ae3d add test.txt
d14dd8e git change file
49a9b49 git tracks changes
1e50f75 undrstand how stage works
9e8bab9 append GPL
b80de0c add distributed
415f790 wrote a readme file

$ git tag v0.1.1 c5421b7  //对c5421b7 进行版本标注
```
具体查看某个版本的详情可以使用命令  `` git show c5421b7 ``

4. 可以创建带有说明的标签: ``git tag -a v0.1.0 -m "version 0.1.0 released" c5421b7``
5. 标签也可以删除 `` git tag -d v0.1.0 ``
6. 推送标签到远端  `` git push origin v0.1.0 `` 
推送全部标签 `` git push origin --tags ``
7. 如果推送后发现需要删除标签，需要先删除本地，再删除远程:
```
$ git tag -d v0.1.0
$ git push origin :refs/tags/v0.1.0




