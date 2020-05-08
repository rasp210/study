- [Linux基本命令](#Linux基本命令)
    - [给某个目录下的所有文件添加前缀](#给某个目录下的所有文件添加前缀)

# Linux基本命令  
## 给某个目录下的所有文件添加前缀
for f in *; do mv -- "$f" "here_is_prev_$f"; done  

### 语法   
---
`comm [OPTION]... FILE1 FILE2`   
**OPTION**   
> 无选项：输出三列-只在FILE1中的行    只在FILE2中的行    两个文件的公共行   
> -1：三列中去掉第一列输出   
> -2：三列中去掉第二列输出   
> -3：三列中去掉第三列输出
> --check-order：检查文件是否有序，默认   
> --nocheck-order：不检查是否有序   


### 示例   
---
- 假设有两个文件     
```shell
$ cat f1
a 1
b 2
c 3
d 4

$ cat f2
b 2
c 3
e 5
```

- 没有选项   
```shell
$ comm f1 f2
a 1
                b 2
                c 3
d 4
        e 5
```

- 两个文件的差集   
```shell
$ comm f1 f2 -3 | awk '{print $1;}'   
等价于   
$ awk '{print $1}' f1 f2 | sort | uniq -u    
a
d
e
```

- 两个文件的交集   
```shell
$ comm f1 f2 -12 | awk '{print $1;}'
b
c
```
