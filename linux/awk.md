# awk

强大的文本分析工具。

### 语法

---

`awk [-F 域分隔符] '命令' 文件`

`awk [-F 域分隔符] 'BEGIN {操作行之前的命令} {每行的操作} END {行操作完之后的命令}' 文件`

1. 文件的每一行，由域分隔符分开的每一项称为一个域。默认域分隔符是空格或Tab
2. \$0表示全部列，\$n表示第n列

### 内置变量

---

> ARGC：命令行参数个数
>
> ARGV：命令行参数
>
> FILENAME：awk浏览的文件名
>
> FNR：浏览文件记录数
>
> NR：已读的记录数
>
> NF：已读记录的域的个数
>
> FS：设置输入域分隔符
>
> OFS：输出域分隔符
>
> RS：控制记录分隔符
>
> ORS：输出记录分隔符

### 示例

---

- last -n 5 | awk '{print $0;}'   

  ```shell
  $ last -n 5 | awk '{print $0;}'
  a  pts/5        10.210.66.44     Wed Aug 29 16:03   still logged in   
  b  pts/2        10.210.66.44     Wed Aug 29 16:01   still logged in   
  b  pts/2        10.210.66.44     Wed Aug 29 14:00 - 16:01  (02:01)    
  a  pts/4        10.210.66.44     Wed Aug 29 13:35   still logged in   
  a  pts/1        10.210.66.44     Wed Aug 29 13:00   still logged in 
  ```

- last -n 5 | awk '{print $3;}'

  ```shell
  $ last -n 5 | awk '{print $3;}'
  10.210.66.44
  10.210.66.44
  10.210.66.44
  10.210.66.44
  10.210.66.44
  ```

  

- cat /etc/passwd | awk -F ':' '{print \$1"\t"\$7;}'

  ```shell
  $ cat /etc/passwd |awk  -F ':'  '{print $1"\t"$7}'
  root    /bin/bash
  daemon  /bin/sh
  bin     /bin/sh
  sys     /bin/sh
  ```

  

- cat /etc/passwd | awk -F ':' 'BEGIN {print "name,shell"} {print \$1","\$7} END {print "name,shell"}'

  ```shell
  $ cat /etc/passwd | awk -F ':' 'BEGIN {print "name,shell"} {print $1","$7} END {print "name,shell"}'
  name,shell
  root,/bin/bash
  daemon,/bin/sh
  bin,/bin/sh
  sys,/bin/sh
  ....
  name,shell
  ```

- 统计ip出现次数最多的前10个ip

  ```shell
  # 1.文件内容：ip 时间戳
  # 2.awk摘出ip列
  # 3.sort对ip排序，将相同的ip放到连续区域
  # 4.uniq计算ip连续重复次数
  # 5.以ip重复倒叙排序
  # 6.head从第一行开始输出到第2行
  $ awk '{print $1}' file | sort | uniq -c | sort -r | head -n 2
        20 10.211.211.199
        12 21.14.14.17
  ```

- 统计文件的：文件名，每行的行号，每行的列数，对应的完整行内容

  ```shell
  $ awk -F ':' '{print "filename:" FILENAME ",linenumber:" NR ",columns:" NF ",linecontent:"$0}' /etc/passwd
  filename:/etc/passwd,linenumber:1,columns:7,linecontent:root:x:0:0:root:/root:/bin/bash
  filename:/etc/passwd,linenumber:2,columns:7,linecontent:daemon:x:1:1:daemon:/usr/sbin:/bin/sh
  filename:/etc/passwd,linenumber:3,columns:7,linecontent:bin:x:2:2:bin:/bin:/bin/sh
  filename:/etc/passwd,linenumber:4,columns:7,linecontent:sys:x:3:3:sys:/dev:/bin/sh
  ```

- printf代替print，代码更简洁

  ```shell
  $ awk -F ':'  '{printf("filename:%10s,linenumber:%s,columns:%s,linecontent:%s\n",FILENAME,NR,NF,$0)}' /etc/passwd
  ```

- 自定义变量

  ```shell
  $ awk '{count++; print $0;} END {print "user count is " count;}' file
  a 2
  d 3
  d 2
  a 5
  t 6
  user count is 5
  # count 默认初始值为0，安全起见还是要在BEGIN中初始化的
  
  $ awk 'BEGIN {count = 0; print "count init is " count;} {count++; print "count: " count;} END {print "count finally is " count;}' file
  count init is 0
  count: 1
  count: 2
  count: 3
  count: 4
  count: 5
  count finally is 5
  
  # 统计某目录下文件占用字节数
  $ ls l | awk 'BEGIN {size = 0;} {size += $5;} END {print "total size is " size;}'
  total size is 187
  ```

- 条件语句

  ```shell
  # if () {}
  # if () {} else {}
  # if () {} else if () {} else {}
  $ ls l | awk 'BEGIN {size = 0;} {if ($5 > 30) {size += $5;}} END {print "total size is " size;}' 
  total size is 167
  ```

- 循环语句

  ```shell
  # while () {}
  # do {} while ()
  # for () {}
  # break
  # continue
  $ awk -F ':' 'BEGIN {count=0;} {name[count] = $1; count++;} END {for (i = 0; i < NR; i++) {print i,name[i]}}' /etc/passwd
  0 root
  1 daemon
  2 bin
  3 sys
  4 sync
  5 games
  ……
  ```

  