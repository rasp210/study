# git request-pull
## 【Doing】
## 语法
git request-pull [-p] <start> <url> [<end>]
## 描述
请求你的上游项目拉取你更新的代码。该请求将输出到标准输出中，开头为分支描述，变化摘要，指明代码 pull 地址。
## 可选项
### -p
在输出中包含补丁文本
### <start>
提交的开始，已经在上游历史记录中
### <url>
要拉取的仓库地址
### <end>
提交的结束，默认为 HEAD
### 例子
```
git push 远程地址 master
git request-pull v1.0 远程地址 master

git push 远程地址 master:for-linux
git request-pull v1.0 远程地址 master:for-linux
```


