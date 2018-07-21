#### 【问题】
在Mac terminal 中的 `~/.bash_profile` 文件中添加命令别名 `alias ls='ls --color=auto'` 并使其生效后出现如下错误提示：
```
$ ls
ls: illegal option -- -
usage: ls [-ABCFGHLOPRSTUWabcdefghiklmnopqrstuwx1] [file ...]
```
#### 【原因】
Mac OS X 的 `ls` 是 BSD 版本的，不支持 `ls --color=auto` 。 

#### 【处理】
1. 通过命令 `grep -Es 'ls \s+--color'` 查找所有配置了 `ls --color` 的地方改为 `ls -G`;
2. 执行 `source ~/.bash_profile` 使修改生效；
3. 若问题仍存在，将 `.bash_profile` 文件中的内容备份后删除，然后重建生效后即可解决；

PS: 在 Mac 上修改文件（夹）字体颜色的命令是 `ls -G`，也可以通过修改环境变量方式来设置，即在 `.bash_profile` 中添加如下两行：
```
export CLICOLOR=1
export LSCOLORS=Exfxcxdxbxegedabagacad
```
