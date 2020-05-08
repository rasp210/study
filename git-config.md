# Git 简介和配置

## 一、Git 简介

### Git 设计的目标
1. 速度
2. 简单设计
3. 对非线性开发模式的强力支持（允许成千上万个并行开发的分支）
4. 完全分布式
5. 有能力高效管理类似 Linux 内核一样的超大规模项目（速度和数据量）

### Git 特点
1. 直接记录快照而非差异比较
Git 和其他版本控制系统的差异在于 Git 对待数据的方法。
概念上区分，其他大部分系统是以文件变更列表的方式来存储信息，这类系统将他们保存的信息看作是一组基本文件和每个文件随时间逐步累积的差异；而 Git 更像是把数据看作是对小型文件系统的一组快照，每次提交更新或在 Git 中保存项目状态时，它主要对当时的全部文件制作一个快照并保存这个快照的索引，Git 对待数据更像是一个快照流。
2. 近乎所有操作都是本地执行
3. Git 保证数据完整性
	- Git 用以计算校验和的机制叫做 SHA-1 散列（hash，哈希），这是一个由 40 个十六进制字符组成的字符串，基于 Git 中文件内容或目录结构计算出来的
 	- Git 数据库中保存的信息都是以文件内容的哈希值来索引
4. Git 一般只添加数据
意思是提交到暂存区的数据很难丢失或被清除
5. 三种状态
 	- 已修改 modified
 	- 已暂存 staged
 	- 已提交 committed
6. Git 的三个工作区
 	- 工作目录
 	- 暂存区域：是一个文件，保存下一次要提交的文件列表信息
 	- Git 仓库：用来保存项目元数据和对象数据库的地方

## 二、Git 配置
Git 自带一个 `git config` 工具来帮助设置控制 Git 外观和行为的配置变量。这些变量存储在三个不同的位置：
1. `/etc/gitconfig` 是系统级别的配置，影响系统上的所有用户和仓库。通过 `git config --system` 命令来设置。
2. `～/.gitconfig` 或 `~/.config/git/config` 是用户级别的配用，影响当前用户所有仓库配置。通过 `git config --global` 命令来设置。
3. `仓库目录/.git/config` 是仓库级别的配置，影响当前仓库。通过 `git config --local` 命令来设置，其中 `--local` 可省略。

下面介绍 Git 配置相关的命令，其中的 `--global` 可以替换成 `--system` 或 `--local` 或省略:
- 用户信息
安装完 Git 或使用 Git 前的第一件事就是设置用户名称和邮箱，这两个信息会写入每一次的提交中而且不可更改：
```
git config --global user.name "yourname"
git config --global user.email "youremail"
```
- 颜色设置
```
开启颜色，值为 false 即关闭
git config --global color.ui true

可配置项
color.branch
color.diff
color.interactive
color.status

颜色值
normal
black
red
green
yellow
blue
magenta
cyan
white

字体值
bold
dim
ul
blink
reverse 
```
- 别名
```
git config --global alias.st status
添加配置后可以直接用别名执行命令，如：`git st`
git config --global alias.br branch
git config --global alias.co checkout
git config --global alias.ci commit 
```
- 推送映射
```
git config --global push.default simple
```
- 检查配置信息
```
git config --list
```
