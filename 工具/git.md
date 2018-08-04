# 功能与优势

- 协同修改
- 数据备份
- 版本管理
- 权限控制
- 历史记录
- 分支管理

优势

- 本地操作
- 完整性保证
- 尽可能添加数据而不是删除或者修改数据
- 与Linux命令全面兼容

#文档

<https://git-scm.com/book/zh/v2/> 文档

## 区域分割

本地库	存放历史版本

暂存区	修改的文件就会存到暂存区，可以提交，也可以撤回操作

工作区	写代码的地方

git的工作流程如下:

在工作目录修改的文件，对修改的文件进行快照，然后放到暂存区，提交更新，将暂存区的文件持久化到本地库

# Git的托管中心

局域网环境下可以搭建gitLab做为文件的托管中心，外网情况下，可以使用gitHub或者码云做为我们的托管中心，

代码托管中心的作用就是维护一个远程库

#使用

## 仓库初始化

```java
git init	//会产生.git文件夹，存放的是本地库相关的目录或文件
```

## 设置和展示签名

```javascript
 git config --global user.name "wangyuqiang"
$ git config -l 	//展示全部的属性
```

## 对文件进行的操作

```java
git add good.txt	//添加
git rm --cached good.txt	//从缓存区移除文件
cat good.txt	//查看文件，猫一眼文件
git commit . -m "second commit"	//对于没有跟踪的文件必须要先add，但是对一个已经add的文件，可以直接提交，但是要-m添加信息

```

# vim编辑器

```java
vim good.txt	//对某个文件进行操作
:set nu	//显示行号
esc		//回到操作
i		//在文本区输入
:wq		//结束操作
```

# 历史记录操作

```java
git log	//查看提交的历史版本
git log --pretty=oneline	//每条记录一行的方式显示历史记录
git oneline	//哈希地址选取一部分最有代表的显示的方式展示一条记录	*只显示过去的记录
git reflog	//同时会显示移动到某个版本指针所需要移动的次数	*前面后面的版本都显示
```

## 多屏分页控制

空格向下分页，b向上翻页，q退出

## 对于版本的操作

```java
git reset --hard 49ae879	//回到某个索引号的版本(推荐)
git reset --hard HEAD^^		//通过HEAD指针向回退^符号个数的版本
git reset --hard HEAD~n		//向后回退n个版本
git reset --hard HEAD		//将缓存区与本地库的指针同步到工作区，未提交的文件会提交

git diff good.txt	//文件未add时可以比较工作区与暂存区good.txt文件的区别
git diff HEAD^ good.txt	//将工作区中的文件和本地库的记录进行比较，不带文件名比较多个文件
```

###reset的三个参数
--soft	仅仅在本地库移动下指针
--mixed	在本地库移动指针，重置暂存区
--hard	在本地库移动head指针，重置暂存区，重置工作区

# 分支操作

```java
git branch -a	//查看所有分支及分支的当前哈希值
git branch myBranch	//创建一条分支
git checkout myBranch	//切换分支
git merge myBranch	//将myBranch的内容合并到master分支，需要在master分支上进行操作
```

## 冲突的解决

解决后重新将文件进行

```
git add 文件名
git commit -m	*不写文件名
```

# 原理

## 哈希

- 不管输入的数据多大，得出的哈希值长度相同，例如md5：都会得到一个16字节的文本
- 哈希算法确定，输入数据确定，结果确定
- 哈希算法固定，输入数据变化，输出结果就会有变化，而且通常是非常大的变化
- 是不可逆的

下载文件后面的sha-1等是官方给提供的哈希码，可以用于比较文件传输是否有丢失

git记录版本为每个文件记录快照，文件更新通过指纹算法检验，如果更新则为新文件拍一张快照，如果没更新，则新文件指向旧文件hashcode，通过sha-1算法计算文件的哈希值，用于时刻保证文件的完整性

## 分支的原理

master,myMaster指向着文件中同一个版本，当一个分支进行操作后，则快照指向一个新的哈希值，这样就像是形成了两条分支，而不是文件的复制，切换分支，就是将HEAD移动到对象的分支即可



#GitHub

```java
git remote -v	//查看github的项目地址
git remote add first https://github.com/lesssugar/first.git	//添加github上的first项目的地址，起名叫first
git clone https://github.com/lesssugar/first.git //在远程库克隆一个项目，别名，本地库一起弄好
```

## 文件的推送与拉取

```java
git push first master	//将master分支推送到github上，会打开一个登录框
git fetch origin master	//将远程库的内容抓取到工作区master，这时候会有一个分支origin/master,可以查看里面的东西
git merge origin/master	//对拉取下来的文件进行合并
git pull origin master	//相当于上面两条操作的合并，拉取并合并内容
```

## 冲突解决

如果本地文件和服务器文件版本不一致，那么服务器会禁止提交，你需要先将服务器的文件pull与你的文件进行合并，然后打开文件进行修改，然后不带文件名的提交，然后在推送到远端

## 权限的给予

仓库	->	setting	->	Collaborators	->	输入要邀请的人的账号        ->	点邀请	->		然后将地址发给他(邮件也会发送)	->	成员打开连接		->	同意加入		->	成员获得推送的权限

# 团队合作

项目外成员可以通过项目地址内右上角的fork，fork人的仓库就会多出一个项目的复制。然后可以复制进行操作等，两个远程库暂时互不冲突

## 远程项目的提交

通过github项目上的PullRequests按钮，newPullRequests,CreatePullRequest,然后写标题，写内容，createPullRequest,然后接受方点PullRequests,在里面可以对代码进行审核，可以进行合并，填写信息

#密匙的生成

$ ssh-keygen -t rsa -C "youremail@example.com"

C:\Users\Administrator\.ssh 在该文件夹下就生成了秘钥

将文件中id_rsa.pub文件中的内容添加到github中，添加一个公钥即可

```
git remote add origin_ssh git@github.com:lesssugar/huashan.git	//配置ssh地址的别名
推送的话推送到ssh地址即可，不会验证用户的账号密码
```

# GitLab环境的搭建

