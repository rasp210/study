#### Git 配置
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
