### Linux 中 Vim 文本编辑器使用

[TOC]

#### 定义

Vim 共分为三种模式：**命令模式（Command mode）**，**输入模式（Insert mode）**和**底线命令模式（Last line mode）**。 

1. 命令模式：打开一个文件即进入命令模式
   - 【i】【o】【s】【a】切换为输入模式
   - 【:】切换到底线命令模式
2. 输入模式
   - 【Esc】进入命令模式
3. 底线命令模式
   - 【删除从:开始的命令字符串】进入命令模式 

#### 使用

##### 1. 命令模式

###### 移动光标

- 【h】【←】光标向左移动一个字符。命令前加数字，如30h，表示向左移动30字符 
- 【j】【↓】【回车】光标向下移动一行。命令前加数字，如30j，表示向下移动30行
- 【k】【↑】光标向上移动一行。命令前加数字，如30k，表示向上移动30行
- 【l】【→】光标向右移动一个字符。命令前加数字，如30l，表示向右移动30字符
- 【0】【Home】移动光标到当前行的首字符
- 【$】【End】移动光标到当前行的末尾
- 【Ctrl+f】【Page Down】【空格】向下翻页。其中，按下空格前输入数字，表示向后翻数字页
- 【Ctrl+b】【Page Up】向上翻页
- 【Ctrl+d】向上翻半页
- 【Ctrl+u】向下翻半页
- 【gg】跳转到文档的首行
- 【Shift+g】【G】跳转到文档的最后一行。命令前加数字，如30G，表示跳转到文档的第30行

###### 搜索替换

- 【/搜索内容】回车之后，从光标往下查找第一个。按 n 是重复前一个动作即向下查找，按 N 反向查找
- 【?搜索内容】回车之后，从光标往上查找第一个。按 n 是重复前一个动作即向上查找，按 N 反向查找

###### 删除、复制、粘贴



- 【x】删除光标所在的字符
- 

##### 2. 输入模式

##### 3. 底线命令模式

- 【:!命令】不退出 Vim，直接执行命令

  ```
  :!ls -a
  :!pwd
  ```

- 【:r !命令】将命令的执行结果插入到光标所在行的下一行

  ```
  示例
  :r !date
  输出如下：
   10 /**
   11  * 光标在此 
   12 Thu Jul 12 11:14:03 CST 2018
   13  */
   
   示例
   :r !pwd
  ```

- 【:起始行号,结束行号 !命令】将指定行中的内容，作为命令的输入参数，命令执行的结果输出到指定行

  ```
  示例-排序
  :1,18 !sort
  
  示例-小写转大写
  :1 !tr [a-z] [A-Z]
  
  示例-数字转对应的小写字母
  :. !tr [0-9] [a-z]
  ```

- 【:起始行号,结束行号 w !命令】同上，不同之处是不会将结果直接覆盖到指定行

  ```
  示例一
  :13,15 w !sort
   13 1
   14 4
   15 5
   16 ?>          
  ~/a.php[+][21][unix][php][90%][0019,0001]                                               
  :13,15 w !sort
  1
  4
  5
  
  Press ENTER or type command to continue
  
  示例二
  不退出，用 sudo 权限保存：
  :w !sudo tee %
  其中，
  %-当前文件的文件名，在:%s/a/b/g中代码当前整个文件
  tee-拷贝一份数据到 % 所指的文件，同时拷贝另一份给标准输出
  ```

- 【:help :w】查看 :w 的说明文档

- 【:help :cmdline-special】查看 % 等特殊符号的说明文档