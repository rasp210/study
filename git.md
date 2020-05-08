# Git 命令
-----
## git --version
-----
## git 获取帮助
### git help <verb>
### git <verb> --help
加上 --all 或 -a 即可查看所欲命令
### man git-<verb>
-----
## git add
-----
## git commit
-----
## git pull
-----
## git push
-----
## git remote
### options 可选项，即在使用其他命令时可添加的选项，位置紧跟 remote
### git remote [-v | --verbose]
```
不带参数，列出已存在的远程仓库名，默认为 origin
$git remote
origin

带参数，列出详细信息，即远程仓库名和相应的远程仓库地址
$git remote -v 
origin	git@github.com:symi210/study.git (fetch)
origin	git@github.com:symi210/study.git (push)
usera   git@github.com:symi210/usera/study.git (fetch)
usera   git@github.com:symi210/usera/study.git (push)
```
### commands 命令
#### git remote get-url [--all | --push] 远程仓库名
#### git remote set-url 远程仓库名 新的远程仓库地址
-----
## git request-pull
### 语法
git request-pull [-p] <start> <url> [<end>]
### 描述
请求你的上游项目拉取你更新的代码。该请求将输出到标准输出中，开头为分支描述，变化摘要，指明代码 pull 地址。
### 可选项
#### -p
在输出中包含补丁文本
#### <start>
提交的开始，已经在上游历史记录中
#### <url>
要拉取的仓库地址
#### <end>
提交的结束，默认为 HEAD
#### 例子
```
git push 远程地址 master
git request-pull v1.0 远程地址 master

git push 远程地址 master:for-linux
git request-pull v1.0 远程地址 master:for-linux
```