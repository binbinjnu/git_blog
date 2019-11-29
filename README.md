# git_blog
git使用整理

## 博客连接:https://www.cnblogs.com/best/p/7474442.html

## 基本概念
```text
本地工作区域
Workspace:
	工作区，就是你平时存放项目代码的地方
Index / Stage：
	暂存区，用于临时存放你的改动，事实上它只是一个文件，保存即将提交到文件列表信息
Repository：
	仓库区（或本地仓库），就是安全存放数据的位置，这里面有你提交到所有版本的数据。其中HEAD指向最新放入仓库的版本
Remote：
	远程仓库，托管代码的服务器，可以简单的认为是你项目组中的一台电脑用于远程数据交换
	
详解
Directory：
	使用Git管理的一个目录，也就是一个仓库，包含我们的工作空间和Git的管理空间。
WorkSpace：
	需要通过Git进行版本控制的目录和文件，这些目录和文件组成了工作空间。
.git：
	存放Git管理信息的目录，初始化仓库的时候自动创建。
Index/Stage：
	暂存区，或者叫待提交更新区，在提交进入repo之前，我们可以把所有的更新放在暂存区。
Local Repo：
	本地仓库，一个存放在本地的版本库；HEAD会只是当前的开发分支（branch）。
Stash：
	隐藏，是一个工作状态保存栈，用于保存/恢复WorkSpace中的临时状态。
	
文件三状态:
	已修改（modified）,已暂存（staged）,已提交(committed)
状态之间的切换:
	https://images2017.cnblogs.com/blog/63651/201709/63651-20170905201033647-1915833066.png
	https://images2017.cnblogs.com/blog/63651/201709/63651-20170914100820891-2098204183.png
	
文件四状态
	https://git-scm.com/book/en/v2/book/02-git-basics/images/lifecycle.png
	https://images2017.cnblogs.com/blog/63651/201709/63651-20170909091456335-1787774607.jpg
文件操作图
	https://images0.cnblogs.com/blog/221923/201501/061510341401056.png
Untracked: 
	未跟踪, 此文件在文件夹中, 但并没有加入到git库, 不参与版本控制. 通过git add 状态变为Staged.
Unmodify: 
	文件已经入库, 未修改, 即版本库中的文件快照内容与文件夹中完全一致. 这种类型的文件有两种去处, 如果它被修改, 而变为Modified. 如果使用git rm移出版本库, 则成为Untracked文件
Modified: 
	文件已修改, 仅仅是修改, 并没有进行其他的操作. 这个文件也有两个去处, 通过git add可进入暂存staged状态, 使用git checkout 则丢弃修改过, 返回到unmodify状态, 这个git checkout即从库中取出文件, 覆盖当前修改
Staged: 
	暂存状态. 执行git commit则将修改同步到库中, 这时库中的文件和本地文件又变为一致, 文件为Unmodify状态. 执行git reset HEAD filename取消暂存, 文件状态为Modified
```

## 配置相关
```text
# 查看系统config
git config --system --list
　　
# 查看当前用户（global）配置
git config --global  --list
 
# 查看当前仓库配置信息
git config --local  --list

git config --global user.name "daniel"  #名称
git config --global user.email daniel@gmail.com   #邮箱
```

## 基本操作
```text
# 在当前目录新建一个Git代码库
git init
# 新建一个目录，将其初始化为Git代码库
git init [project-name]

# 克隆一个项目和它的整个代码历史(版本信息)
git clone [url]

# 查看指定文件状态
git status [filename]
# 查看所有文件状态
git status
```

### staged操作
```text
## 添加指定文件到暂存区(staged)
git add [file1] [file2] ...

## 添加指定目录到暂存区，包括子目录
git add [dir]

## 添加当前目录的所有文件到暂存区
git add .

## 直接从暂存区(staged)删除文件，工作区则不做出改变
git rm --cached <file>

## 如果已经用add 命令把文件加入stage了，就先需要从stage中撤销, 暂存区的目录树会被重写，被 master 分支指向的目录树所替换，但是工作区不受影响。
git reset HEAD <file>...
```

### 文件移除\删除\改名
```text
## 移除所有未跟踪文件, 一般会加上参数-df，-d表示包含目录，-f表示强制清除。
git clean [options] 

## 只从stage中删除，保留物理文件
git rm --cached readme.txt 

## 不但从stage中删除，同时删除物理文件
git rm readme.txt 

## 把a.txt改名为b.txt
git mv a.txt b.txt 
```

### 文件比较
```text
## 查看文件修改后的差异
git diff [files]

## 比较暂存区的文件与之前已经提交过的文件
git diff --cached

## 比较repo与工作空间中的文件差异
git diff HEAD~n
```

### 签出
```text
## 检出branch分支。要完成图中的三个步骤，更新HEAD以指向branch分支，以及用branch  指向的树更新暂存区和工作区。
git checkout branch

## 汇总显示工作区、暂存区与HEAD的差异。
git checkout
git checkout HEAD

## 用暂存区中filename文件来覆盖工作区中的filename文件。相当于取消自上次执行git add filename以来（如果执行过）的本地修改。
git checkout -- filename

## 维持HEAD的指向不变。用branch所指向的提交中filename替换暂存区和工作区中相   应的文件。注意会将暂存区和工作区中的filename文件直接覆盖。
git checkout branch -- filename

## 注意git checkout 命令后的参数为一个点（“.”）。这条命令最危险！会取消所有本地的  #修改（相对于暂存区）。相当于用暂存区的所有文件直接覆盖本地文件，不给用户任何确认的机会！
git checkout -- . 或写作 git checkout .

## 如果不加commit_id，那么git checkout -- file_name 表示恢复文件到本地版本库中最新的状态。
git checkout commit_id -- file_name

当执行提交操作（git commit）时，暂存区的目录树写到版本库（对象库）中，master 分支会做相应的更新。即 master 指向的目录树就是提交时暂存区的目录树。
当执行 “git reset HEAD” 命令时，暂存区的目录树会被重写，被 master 分支指向的目录树所替换，但是工作区不受影响。
当执行 “git rm –cached <file>” 命令时，会直接从暂存区删除文件，工作区则不做出改变。
当执行 “git checkout .” 或者 “git checkout — <file>” 命令时，会用暂存区全部或指定的文件替换工作区的文件。这个操作很危险，会清除工作区中未添加到暂存区的改动。
当执行 “git checkout HEAD .” 或者 “git checkout HEAD <file>” 命令时，会用 HEAD 指向的 master 分支中的全部或者部分文件替换暂存区和以及工作区中的文件。这个命令也是极具危险性的，因为不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改 动。
```

### 提交
```text
## 提交暂存区到仓库区
git commit -m [message]
## 提交暂存区的指定文件到仓库区
git commit [file1] [file2] ... -m [message]
## 提交工作区自上次commit之后的变化，直接到仓库区，跳过了add,对新文件无效
git commit -a
## 提交时显示所有diff信息
git commit -v
## 使用一次新的commit，替代上一次提交. 如果代码没有任何新变化，则用来改写上一次commit的提交信息(修改-m的信息)
git commit --amend -m [message]
## 重做上一次commit，并包括指定文件的新变化 一般都需要 -m, 不然不让提交, 同上
git commit --amend [file1] [file2] ...
```

### 日志和历史
```text
## 查看提交日志
git log [<options>] [<revision range>] [[\--] <path>…?]

## 查看这个仓库中所有的分支的所有更新记录，包括已经撤销的更新。
git reflog
```

### 远程仓库操作
```text
## 下载远程仓库的所有变动
git fetch [remote]

## 显示所有远程仓库
git remote -v

## 显示某个远程仓库的信息
git remote show [remote]

## 增加一个新的远程仓库，并命名
git remote add [shortname] [url]

## 取回远程仓库的变化，并与本地分支合并
git pull [remote] [branch]

## 上传本地指定分支到远程仓库
git push [remote] [branch]

## 强行推送当前分支到远程仓库，即使有冲突
git push [remote] --force

## 推送所有分支到远程仓库
git push [remote] --all

## 简单查看远程---所有仓库
git remote  （只能查看远程仓库的名字）

## 查看单个仓库
git remote show [remote-branch-name]

## 新建远程仓库
git remote add [branchname]  [url]

## 修改远程仓库
git remote rename [oldname] [newname]

## 删除远程仓库
git remote rm [remote-name]

## 获取远程仓库数据
git fetch [remote-name] (获取仓库所有更新，但不自动合并当前分支)

git pull (获取仓库所有更新，并自动合并到当前分支)
## 上传数据，如git push origin master
git push [remote-name] [branch]
```

## 忽略文件
```text
在主目录下建立".gitignore"文件，此文件有如下规则：
	忽略文件中的空行或以井号（#）开始的行将会被忽略。
	可以使用Linux通配符。例如：星号（*）代表任意多个字符，问号（？）代表一个字符，方括号（[abc]）代表可选字符范围，大括号（{string1,string2,...}）代表可选的字符串等。
	如果名称的最前面有一个感叹号（!），表示例外规则，将不被忽略。
	如果名称的最前面是一个路径分隔符（/），表示要忽略的文件在此目录下，而子目录中的文件不忽略。
	如果名称的最后面是一个路径分隔符（/），表示要忽略的是此目录下该名称的子目录，而非文件（默认文件或目录都忽略）。
	#号为注释
		*.txt 		#忽略所有 .txt结尾的文件
		!lib.txt 	#但lib.txt除外
		/temp 		#仅忽略项目根目录下的TODO文件,不包括其它目录temp
		build/ 		#忽略build/目录下的所有文件
		doc/*.txt 	#会忽略 doc/notes.txt 但不包括 doc/server/arch.txt
```

## git分支
```text

```
