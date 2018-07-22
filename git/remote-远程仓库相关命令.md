#### Git Remote
##### options 可选项，即在使用其他命令时可添加的选项，位置紧跟 remote
###### git remote [-v | --verbose]
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
##### commands 命令
###### git remote get-url [--all | --push] 远程仓库名
###### git remote set-url 远程仓库名 新的远程仓库地址

