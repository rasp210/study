# uniq

### 定义   

---

Linux命令uniq的作用是过滤重复部分显示文件内容，这个命令读取输入文件，并比较相邻的行。

在正常情况下，第二个及以后更多个重复行将被删去，行比较是根据所用字符集的排序序列进行的。该命令加工后的结果写到输出文件中。输入文件和输出文件必须不同。如果输入文件用“- ”表示，则从标准输入读取。   

### 用法

---

`uniq [OPTION]... [INPUT [OUTPUT]]`

### 可选项

---

- -c, --count

  > 每行前输出该行连续重复的次数   

- -d, --repeated

  > 仅输出连续重复行一行

- -D, --all-repeated[=METHOD]  

  > 输出连续重复行的所有行
  >
  > --all-repeated=prepend：重复行前置空行隔开
  >
  > --all-repeated=separate：重复行之间空行隔开
  >
  > --all-repeated=none：默认选项，没有空行输出

- -f, --skip-fields=N

  > 比较时跳过前N列

- --group[=METHOD]

  > 分组展示所有项，每组都是相同连续行，默认用空行隔开
  >
  > --group=prepend：前置空行隔开分组
  >
  > --group=append：后置空行隔开分组
  >
  > --group=both：同时用前置和后置行隔开分组
  >
  > --group=separate：默认选项，空行隔开分组

- -i, --igonre-case

  > 比较时忽略连续行间的大小写

- -u, --unique

  > 仅输出没有连续重复的行

- -z, --zero-terminated

  > 使用'\0'作为行结束符，而不是新换行 

- -w, --check-chars=N

  > 仅对每行的前N个字符进行比较

- --help

- --version

### 示例

---

- 文件内容

  ```shell
  $ cat file
  boy took bat home
  boy took bat home
  girl took bat home
  boy took bat home
  boy took bat home
  dog brought hat home
  dog brought hat home
  dog brought hat home
  ```

- 仅显示连续重复行一次

  ```shell
  $ cat file | uniq
  boy took bat home
  girl took bat home
  boy took bat home
  dog brought hat home
  ```

- 输出每行连续出现的次数

  ```shell
  $ uniq -c file
        2 boy took bat home
        1 girl took bat home
        2 boy took bat home
        3 dog brought hat home
  ```

- 先排序，后输出每行连续出现次数

  ```shell
  $ sort file | uniq -c
        4 boy took bat home
        3 dog brought hat home
        1 girl took bat home
  ```

- 仅输出有连续出现的行

  ```shell
  $ uniq -d file
  boy took bat home
  boy took bat home
  dog brought hat home
  ```

- 仅输出没有连续出现的行

  ```shell
  $ uniq -u file
  girl took bat home
  ```

  