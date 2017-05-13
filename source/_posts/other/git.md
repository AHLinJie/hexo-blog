---
layout: post
title: Git命令
date: 2015-07-23
category: 版本控制
tag: git
---

**摘要:**
传说林纳斯嫌弃svn不好用就自己写了一个版本控制器叫做git.然后git成了很多人使用的
版本控制器,当然排除他的个人影响力之外git是一个非常了不起的版本控制工具.当然git
好用还是在命令行里面,一下是在命令行里面的一些命令,方便自己查阅用到.git还有很多
很特别和使用的功能.比如subtree等等.



---

#### global config

- git config --global color.ui true # show the color in ui 
- git config --global user.name "youname" 
- git config --global user.email " xxxx@xx.com" vjknct
- git config -u-list  # show the git config list

---

#### Config Ignore File
Touch a file with named `.gitignore`  and save in git init dir .
edit the file you want to ignore 


---

#### Git Usually Command
- cd adir
- git init # init a git dit
- git add readme.txt # 添加文件（修改的文件或者新的文件）到暂存区 
- git commit -m "wrote a readme file" # 提交暂存区中所有文件到当前分支中 -m标注提交信息 
 
- git status # 查看仓库状态 
- git commit readme.txt -m "commit the modefied file" 

---

#### Git Log
- git log # 查看git操作的日志信息（提交信息 
- git log -5 # the last 5 logs
- git log --pretty=oneline # show log main commit info in oneline by every log
- git log --graph --pretty=oneline --abbrev-commit # 查看分支合并情况 
- git log --graph # 查看分支合并图（左侧的那条线或者分支线） 

---

#### About reset command

- git reset --hard HEAD^ # 版本回退到上一个版本 
- git reset --hard HEAD~3 # 版本回退3次的版本 
- git reflog # 查看 commit-id 以及操作信息 
- git reset --hard <commit-id> # 版本回退找回新版本 
- git reset HEAD filename # 将已经添加到暂存区的撤销到工作区 
- git reset HEAD filename # 移动指向被删除的文件的指针到删除前 

---

#### Git Diff
git diff # 查看工作区的文件内容和暂存区文件内同的不同部分 
git diff HEAD -- <filename> # 查看暂存区和版本库中文件内容的不同部分 

---

#### Git Checkout
- git checkout -- filename # remove you modify  
- git checkout f5810e08df0cb7cf82608d65cdb709cb430df2ea path/to/file # assagin a file conten to the commit id
 
---

#### Git rm 

- git rm filename 
删除和git库的关联，同时删除文件,如果通过 rm 直接删除的则 利用git status 查看时候回有提醒删除文件，touch一个同名的文件则git status 则会取消提醒了 


---

#### About Github


- ssh-keygen -t rsa -C " xxx@xx.com" # create file cmd by ssh
add the ssh puh key file into github setting
注册github的账号：AHLinJie 
在github中新建test文件夹 

- git remote add origin  https://github.com/AHLinJie/test.git # 添加远程仓库名称为origin 
- git remote # 查看远程仓库名称 
- git remote -v # 查询更详细的远程仓库信息 
- git push -u origin master # 将本地库推送到远程仓库中 并建立本地与远程的master分支的关联  随后再次推送的时候就没必要用-u 参数 
- git push origin dev # 推送dev分支到远程仓库 
- git clone  https://github.com/AHLinJie/test2.git # 克隆github上面的远程文件到本地  前提是有远程的存在 

注意： 
    1. 远程克隆的文件包含了.git的文件，status是clean的 
    2. 克隆到本地的文件夹都是没有提交的，但是在克隆的文件夹内是提交的，这里要注意区分版本库的范围，可以对应的目录下status查看下 


---

#### Git Branch 

- git checkout -b dev # 创建名称为dev的分支，并且切换到该分支 
- git branch dev # 创建名称为dev的分支 
- git branch # 查看当前分支 * 表示的分支 
- git branch --all # 查看所有包括远程的分支 
- git checkout master # 回到master分支 
- git merge dev # 合并dev分支到master分支 
- git merge --no-ff -m "merge with no-f" dev  # 或者使用--no-ff模式来提交保留分支历史信息 
- git branch -d dev # 删除dev分支
- git push origin --delete branchname  # 删除远程分支

注意合并冲突内容： 
在合并的分支中处理冲突的内容（手动修改） 

---

#### Branch Strategy： 
- branch master: 
  1. 非常稳定，时刻要和远程保持同步 
  2. 用来发布新版本 
  3. 禁止其他操作 
-branch dev： 
  1. 用于开发测试，团队开发 
  2. 保持与远程同步 

- bug branch:
  1. git checkout -b issure-101 # 新建一个bug处理分支 
  2. 在dev分支 git stash # 存储工作现场，等处理bug后继续开发工作时恢复现场用 
  3. 选择要修复bug用的分支去处理bug问题 
  4. 等问题处理好了 
  5. 处理好后，回到工作现场所在的分支 
  6. git stash list # 查看保存的现场信息 
  7. git stash pop # 恢复现场（删除stash的信息） 

- feature branch: 
  1. 开发过程中添加新特性、新功能 
  2. git checkout -b feature-weixin 
  3. 新建文件、编码开发、添加、提交、合并 
  4. git branch -d feature-weixin # 删除该分区 
  5. 如果因为经费不足等问题要中止该功能开发 git branch -D feature-weixin # 强行删除该分支 

---

#### 多人协作开发： 
- git branch --set-upstream dev origin/dev # 指定本地dev分支与远程origin/dev分支的链接 
- git checkout -b dev origin/dev # 创建远程的origin dev分支到本地，如果有dev存在，则可以-d后重新建立 
- git push origin dev # 推送本地分支到远程分支 

---

#### Git Tag
- git tag v1.01 # 为当前的commit id （版本）设置标签 切换到对应分支 
- git log --pretty=oneline --abbrev-commit # 查询历史版本的commit id 设置标签 
- git tag commit-id # 指定历史commit记录打标签 
- git tag -a v1.03 -m "create a ......" commit-id # 指定commit-id 和注释的版本标签 
- git show <tag-name> # 查看标签信息 
- git tag -s v1.05 -m "......" <commit-id> # 使用秘钥签名一个标签 前提是安装了PGP(GnuPG) 没有安装则会报错 
- git tag -d <tagname> # 删除tag标签 
- git push origin <tagname> # 推送标签到远程仓库 
- git push origin --tags # 推送本地所有标签到远程仓库 
- **删除远程tag标签操作 首先删除本地的然后删除远程的**
- git tag -d <tagname> # 删除本地的tag标签 
- git push origin :refs/tags/<tagname> # 删除远程的tag标签 

---

#### 搭建git服务器： 
/home/lijie/gitbar # 远程仓库目录，这里是Ubuntu虚拟机中的目录 

1. sudo apt-get install git 
2. sudo adduser git  # 创建用户 
3. sudo git init --bare sample.git # 创建裸仓库（没有工作区：服务器是纯粹共享用的） 
4. edit /etc/passwd
  4.1 git:x:1001:1001:,,,:/home/git:/bin/bash 改为： 
      git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell # 禁用shell登陆 

5. sae git server certificate verification failed
6. edit file ～/.bashrc
add the line: `export GIT_SSL_NO_VERIFY=1`end of the file and source ~/.bashrc

---

#### Git Submodel

....

---

#### Git Fetch

- git fetch origin  # 是将远程的代码pull下来在对应的分支　不去merge



#### Git Blame

- git blame 
  - `git blame -L 569,+10 models.py`

