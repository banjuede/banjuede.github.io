
# 创建版本库
注意仓库和版本库的区别
```
git init
```
### 查看仓库的状态
```
git status
```
### 查看文件修改内容
```
git diff
```
### 向暂存区中添加文件修改
```
git add read.txt
# 添加所有文件 git add .
```
### 向当前分支中提交文件修改
```
git commit -m "此处注释"
```

# 版本管理
### 查看提交历史
```
git log [--pretty=oneline]
# 会显示从最近到最远的提交日志，包含 commit_id
```
### 回退到上一个版本
```
git reset --hard HEAD^
```
### 回退到某一个版本
```
# 回退前用 git log 查看提交历史，以便确定回退到哪个版本
git reset --hard commit_id
```
### 查看命令历史
```
git reflog
```
### 回到过去要重返未来
```
# 先用 git reflog 查看命令历史，以便确定要回到未来哪个版本
git reset --hard commit_id
```
### 工作区、版本库和暂存区
```
工作区就是电脑里看到的工作目录。
工作区有一个隐藏目录 .git，这个就是版本库。
版本库里存了很多东西，最重要的就是暂存区(stage/index)和Git为我们自动创建的第一个分支 master，以及指向master的一个指针叫 HEAD。
git add 就是向暂存区中添加文件修改，git commit 就是向当前分支中提交文件修改。
其实就是：需要提交的文件修改通通先放到暂存区，然后一次性提交暂存区的所有修改。
```
### Git管理的是修改不是文件
### 查看工作区和版本库里最新版的区别
```
git diff HEAD -- <fileName>
```
### 丢弃工作区的修改
```
git checkout -- <fileName>
命令 git checkout -- <fileName> 意思就是，把<fileName>文件在工作区的修改全部撤销，这里有两种情况：
一种是<fileName>自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
一种是<fileName>已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
总之，就是让这个文件回到最近一次git commit或git add时的状态。
```
### 丢弃暂存区的修改
```
git reset HEAD <fileName>
```
### 从工作区删除文件后再从版本库中删除该文件
```
git rm <fileName>
git commit -m ""
```
### 如果工作区误删文件，从版本库中恢复该文件
```
git checkout -- <fileName>
```

# 远程仓库
### 创建SSH Key
```
ssh-keygen -t rsa -C "youremail@example.com"
```
### 添加远程仓库
```
git remote add origin git@github.com:banjuede/devnote.git
```
### 第一次把本地库推送到远程库
```
git push -u origin master
# 由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令：git push origin master
```
### 从远程库克隆到本地
```
git clone git@github.com:banjuede/test.git
```

# 分支管理
### 查看分支
```
git branch
```
### 创建分支
```
git branch <name>
```
### 切换分支
```
git checkout <name>
```
### 创建并切换分支
```
git checkout -b <name>
```
### 合并某分支到当前分支
```
git merge <name>
```
### 删除已经合并过的分支
```
git branch -d <name>
```
### 删除没有被合并过的分支
```
git branch -D <name>
```
### 当Git无法自动合并分支时，就必须首先解决冲突，解决冲突后，再提交，合并完成
### 查看分支合并图
```
git log --graph --pretty=oneline --abbrev-commit
```
### 分支管理策略？
### Bug分支？
### 开发一个新功能最好新建一个分支

## 多人协作
### 查看远程库信息
```
git remote -v
```
### 从本地推送分支
```
git push origin <branchName>
```
### 抓取当前分支远程的新提交
```
git pull
```
如果有冲突则要处理冲突后才可以提交
### 在本地创建和远程分支对应的分支
本地和远程分支的名称最好一致
```
git checkout -b <local-branch-name> origin/<branch-name>
```
### 建立本地分支和远程分支的关联
```
git branch --set-upstream <local-branch-name> origin/<branch-name>
```

# 标签管理
### 查看所有标签
```
git tag
```
### 打一个新标签
```
git tag <name>
```
