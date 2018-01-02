# Git

## Linux
mkdir git-test -->创建目录
cd git-test    -->进入目录
pwd            -->显示路径
**/GitHub/git-test
[项目 GitHub 地址][1]


<!--more-->


## 初始化git
//确保目录中没有中文
git init -->git 初始化

## 把Git装进本地库需要2步 add commit
git add README.md
git commit -m '提交README.md文件'

## 放到远程库
git remote add origin git@github.com:***/git-test.git //添加到远程库
//远程库 名字 origin

git push -u origin master //推送到远程库origin 的master分支上- -u是与远程的master分支做关联，用在第一次创建分支时候以后就不需要-u了

## 多啦A梦的时光机
git status  查看当前工作区状态
git diff    查看工作区修改的文件 ctrl+z 退出

git add README.md
git commit -m 'commit'
[master 3628164] append GPL
 1 file changed, 1 insertion(+), 1 deletion(-)

git log 查看提交历史记录

######  回到19世纪
 git reset --hard HEAD^ ^^^表示回退版本个数
#####   回到21世纪
 git reset --hard 3628164 提交时候的版本号
###### 查看commit版本号
git reflog

###### 未add的区域是工作区
git add 是把文件放到暂存区
git commit 放到分支上

#### 2.2 撤销
###### 撤销尚未add的文件
git checkout -- readme.txt
###### 撤销add的文件,但没有commit的
通过 git status 发现问题
git reset HEAD README.md

###### 删除文件
git rm file 删除;
git commit file -m 'remove file';
git push orgin master;


## 分支
创建分支:
git branch dev 创建dev分支
git checkout -b dev 创建dev并切换到该分支

切换分支:
git checkout dev 切换到dev分支

查看分支list
git branch  当前分支标*

合并分支
git merge dev
把dev分支合并到当前分支上

删除本地分支
git branch -d dev
删除远程分支
git branch -r -d origin/dev
git push origin :dev  推送到远程

## 解决冲突

### 多人协作的工作模式通常是这样：

首先，可以试图用git push origin branch-name推送自己的修改；

如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

如果合并有冲突，则解决冲突，并在本地提交；

没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！

如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream   branch-name origin/branch-name。

这就是多人协作的工作模式，一旦熟悉了，就非常简单。

## 标签管理
##### create Tag

  git branch
  git checkout master 切换分支
  git tag v1.0 打标签
  git tag 查看所有标签
  git tag v0.9 6224937  对log里的commit id 某个d打tag
  git show tagName 查看标签信息

  -a 指定tagName -m tag说明
  git tag -a v0.1 -m "version 0.1 released" 3628164

##### controll Tag

  git tag -d v0.1 删除tag
  git push origin <tagname>  推送某个标签到远程
  git push origin --tags 推送所有tag到远程

  如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：
  git tag -d <tagname>
  然后，从远程删除。删除命令也是push，但是格式如下：
  git push origin :refs/tags/<tagname>

## 忽略文件
 .gitignore

## 创建Git服务器
搭建Git服务器需要准备一台运行Linux的机器，强烈推荐用Ubuntu或Debian，这样，通过几条简单的apt命令就可以完成安装。
第一步，安装git：
sudo apt-get install git

第二步，创建一个git用户，用来运行git服务：
sudo adduser git

第三步，创建证书登录：
收集所有需要登录的用户的公钥，就是他们自己的id_rsa.pub文件，把所有公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个。

第四步，初始化Git仓库：
先选定一个目录作为Git仓库，假定是/srv/sample.git，在/srv目录下输入命令：
sudo git init --bare sample.git

Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以.git结尾。然后，把owner改为git：
sudo chown -R git:git sample.git

第五步，禁用shell登录：
出于安全考虑，第二步创建的git用户不允许登录shell，这可以通过编辑/etc/passwd文件完成。找到类似下面的一行：
git : x:1001:1001:,,,:/home/git:/bin/bash
改为：
git : x:1001:1001:,,,:/home/git:/usr/bin/git-shell
这样，git用户可以正常通过ssh使用git，但无法登录shell，因为我们为git用户指定的git-shell每次一登录就自动退出。

第六步，克隆远程仓库：
现在，可以通过git clone命令克隆远程仓库了，在各自的电脑上运行：
git clone git@server:/srv/sample.git
Cloning into 'sample'...
warning: You appear to have cloned an empty repository.


## END

http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137586810169600f39e17409a4358b1ac0d3621356287000


  [1]: https://github.com/liumangtul/git-test/blob/master/README.md