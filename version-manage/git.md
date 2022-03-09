## 设置用户名和邮箱

设置与修改用户名和邮箱
$ git config --global user.name "xxx"
$ git config --global user.email "xxx"

查看自己的用户名和邮箱地址：
$ git config user.name
$ git config user.email

--global参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，我们也可以对某个仓库指定不同的用户名和Email地址；

找到项目所在目录下的 .git/文件夹，进入.git/文件夹，然后执行如下命令分别设置用户名和邮箱：
$ git config user.name "xxx"
$ git config user.email "xxx"
注意与前面相比，没有--global参数

然后再执行一下：
$ git config user.name
$ git config user.email
就能查看到该仓库项目的用户名和邮箱，

也可以执行：
cat config
查看配置文件
[core]
        repositoryformatversion = 0
        filemode = false
        bare = false
        logallrefupdates = true
        symlinks = false
        ignorecase = true
[remote "origin"]
        url = https://github.com/xiaolinsean/gitLearn.git
        fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
        remote = origin
        merge = refs/heads/master
[user]
        name = gitname
        email = git@163.com



## 基本操作

- git add 将文件修改添加到暂存区

  示例：  
  git add readme.txt   提交一个文件
  git add file2.txt file3.txt  提交多个文件
  git add 文件夹/            添加整个文件夹及内容
  git add . ：把工作时的所有变化提交到暂存区，包括文件内容修改(modified)以及新文件(new)，但不包括被删除的文件。
  git add -u ：仅提交已经被add的文件（即tracked file），不会提交新文件（untracked file）。（git add --update的缩写）
  git add -A ：是上面两个功能的合集（git add --all的缩写）


- git commit 将暂存区的所有内容提交到当前分支
  git commit -m "message" -m 后面可以输入本次提交的相关信息说明；
  git commit -am "message"  -am 针对已经tracked的文件，相当于git add和git commit -m的组合技，如果有新增文件，则需要先执行git add；


- 本地推送分支到远程
  - 远程先开好分支然后拉到本地
    git checkout -b branch1 origin/branch1    //检出远程的branch1分支到本地
  - 本地先开好分支然后推送到远程
     git checkout -b branch1    //创建并切换到分支branch1  
     git push origin branch1:branch1    //推送本地的branch1(冒号前面的)分支到远程origin的branch1(冒号后面的)分支(没有会自动创建)

- pull对应分支
  - git pull origin <remote_branch>：<local_branch>
        将远程分支拉取到指定本地分支，不在当前分支也可拉取
        例如：当前分支是dev，但是你想把远程master”同步”到本地master，但又不想使checkout切换到master分支；
        git pull origin master:master
  - git pull origin <remote_branch>
        将指定远程分支同步到当前本地分支；
        例如：当前分支是dev，但是你想把远程master”同步”到本地dev:
        git pull origin master;
        或者是本地的dev还未与远程的dev关联起来:
        git pull origin dev.
  - git pull
        拉取所有远程分支的新版本"坐标"，并同步当前分支的本地代码；前提是：本地分支已经和想要拉取的分支建立了“关联”关系；

- git分支关联
  - git branch -vv 查看本地分支与远程分支关联关系
  - 本地检出远程分支时会建立：
        git checkout -b dev origin/dev
  - 提交时配置关联关系：
        git push -u origin <remote_branch> 或 
        git push --set-upstream origin <remote_branch>
  - 更改git/config文件
       git branch --set-upstream-to=origin/remote_branch 

- push 对应分支
  - git push
        将当前分支代码同步到远程同名分支，本地分支必须与远程关联分支同名；
  - git push origin <local_branch>
        "同步"指定的本地分支到远程关联同名分支，
        特别注意：
                1.如果本地dev的关联分支与dev名称不一样，则会在远程新建一个dev分支；
                2.新建的远程dev分支并不会与本地的dev分支建立关联关系；(本地的dev还关联的是其检出时的那个分支)
                3.如果想在检出时建立分支，需要使用git push -u dev这样同步时就会关联新创建的远程分支；
  - git push origin <local_branch>：<remote_branch>
        将指定的本地分支推送到指定的远程分支；（这两个分支并没有建立关联关系，且可以不同名）

- log 查看对应文件修改/提交记录
  - git log
      查看当前项目的提交记录，这时会进入查看模式，按enter可查看后面的提交记录内容，如果需退出，按q；
  - git log -p filename
      查看文件的每一个详细的历史修改，如果没有-p选项，只显示提交记录，不显示文件内容修改，git log -p -3 filename 显示最近的3次提交。
  - git log --pretty=oneline filename
      每一行显示一个提交，先显示哈希码，再显示提交说明。
  - git show <git提交版本号> <文件名>
      查看对应文件名具体的修改详情

- delete分支
  - git branch -d <branch_name>
      删除本地分支，删除前先合并，如果没有合并就删除,系统会提示
  - git push origin --delete <branch_name>
      删除远端分支
  - git branch -D <branch_name>
      强制删除分支（不需要合并等操作）

- tag 打标签
  - git tag
    查看现有所有标签
  - git tag -l 'v1.4.2.*'
    筛选出符合条件的标签，如上为刷选出1.4.2系列的版本
  - git tag -a v1.4 -m "my version 1.4" 打标签，
    创建一个含附注类型的标签，用 -a （译注：取 annotated 的首字母）指定标签名，用 -m 选项指定对应的标签说明(注意标签说明在window下需要用双引号，不然会报错)
    git tag v1.4-lw 直接创建一个标签，不添加标注，
  - git show 
    查看对应标签的信息，例如：git show v1.4-lw
  - push tag 往远端推本地tag
    git push origin v1.5,往远端推本地名为v1.5的tag
    git push origin --tags 一次将本地的新增的所有tag都推送到远端
  - delete tag 删除tag
    git tag -d tag_name 删除本地标签
    git push origin :refs/tags/tag_name 删除远程tag


- 分支提交错误处理
  1、改完bug忘记切换分支了，代码改了很多怎么办。
  git add .      (把所有改动暂存)
  git stash     (把暂存的文件提交到git的暂存栈)
  git checkout 本该提交代码的分支 
  git stash pop (将暂存栈中的代码放出来)

  2、代码不但改了，还提交了。
  git  checkout 不该提交代码提交了代码的分支
  git reset HEAD~1  （最近一次提交放回暂存区, 并取消此次提交）
  git stash   (把暂存的文件提交到git的暂存栈)
  git checkout 该提交代码的分支
  git stash pop 
  等你把代码提交到了正确的分支后，再次切到刚刚错的分支git push origin 错误的分支 -f  (把远程不该上去的文件回退掉)

- 修改git分支名称
  - 1、将本地分支oldbranch切一个分支到本地
  
     git branch -m oldbranch newbranch

  - 2、删除远程分支
     
    git push --delete origin oldbranch
  - 3、将本地新分支推送到远程

    git push origin newbranch

- 暂存修改 git stash
  - 暂存当前修改
  
    git stash
    
  - 查看当前的暂存列表
  
    git stash list

  - 恢复暂存修改
  
      - git stash apply (恢复最新的一条暂存)
      - git stash apply "stash@{1}" （恢复指定的一条暂存）
      - git stash drop "stash@{1}" （删除对应的暂存）

      - git stash pop （恢复最新的一条暂存并删除之）

  - git stash clear 删除所有缓存的stash

- 本地删除远程已不存在的分支
  - git branch -a 查看所有分支（包括远程和本地）
  - git branch -r 只查看远程分支
  - git remote show origin 查看本地与远程分支的关联关系
  - git remote prune origin 删除远程仓库不存在的分支
  