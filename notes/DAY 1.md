## 一些概念性的知识

计算机与用户交互的方式：  
比如 GUI (graphic user interfaces)：用户操作鼠标键盘的图形用户界面，脑机接口等。但是这些交互方式通常只能执行开发者所编写的程序，而shell可以将这些操作结合起来。shell是终端里跑的程序，是一种文本交互界面，是早期的交互方式，也是一门编程语言。（就像python是一个程序，也是一门编程语言）  
shell效率很高而且可以自动化处理操作，这就是为什么不用GUI。

Ubuntu 与 bash：  
Ubuntu 是一种以 Linux 内核为基础的操作系统，可以理解为Linux是一种发动机，Ubuntu是一台车，bash是跑在Ubuntu里面的一种shell。打开 Ubuntu 实际上就是提供了一个 bash 交互途径。

## 一些命令

**date**

```bash
date   #显示时间，其中输出 CST 表示中国标准时间（上海时间）
```

**echo**

```bash
echo xxx   #输出xxx
```

其中用户输入的内容为传入参数，按照空格划分，第一个字符串为命令。  
几种输入方式：

```bash
echo hello world        #输出hello world
echo hello    world     #输出hello world
echo "hello    world"   #输出hello   world
echo hello\ \ \ world   #输出hello   world
```

反斜杠加空格就是不将空格视作特殊字符而是普通字符（跟c++方向相反）。

**man**

操作手册manual

```bash
man ls   #介绍 echo 的操作
```

**cd(change directory)**

```bash
cd xxx   #切换到相对目录
cd /xxx  #切换到绝对目录
cd /     #切换到Ubuntu的根目录
cd .     #切换到当前目录（原地）
cd ..    #切换到父目录（根目录中不动）
```

TAB键可以自动补全，如果存在歧义，则双击TAB可以出现备选项

**which**

```bash
which echo    #查询 echo 这个程序的第一路径
echo $PATH    #shell 查询程序位置的搜索路径，冒号分隔不同路径
which -a echo #查询 echo 这个程序的所有路径
```

**ls**

```bash
ls       #列出当前目录下的文件
ls  path #列出指定路径下的文件
ls -l    
#-  rwx  rwx  rwx
#①   ②    ③    ④
#① 类型：- 文件  d 目录  l 链接
#② 属主权限  ③ 属组权限  ④ 其他人权限
ls *.txt      #列出所有.txt
ls test?.txt  #列出形如 test1.txt test2.txt
ls {a,b}.txt  #列出 a.txt 和 b.txt（若有）
```

**cat**

```bash
cat file  #打印文件内容
```

**nvim**

```bash
nvim file  #打开终端编辑器
```

vim基本操作：  
esc退出编辑   :wq保存并退出

**特殊的输出文件方式**

```bash
sort file     #将文件中的内容排序后输出
uniq file     #合并相邻的相同内容
head -nx file #输出前x行
tail -nx file #输出后x行
grep x file   #输出含有x的内容
```

**sed**

这是一门编程语言，常用操作如下

```bash
sed 's/a/b/'            # 每行第一个 a 换成 b 后输出，s 表示替换
sed 's/a/b/g'           # 每行全部 a 换成 b 后输出
sed -i 's/a/b/g' f      # 直接改文件（-i）
sed -n '/error/p' f     # 只打印含 error 的行
sed '/^#/d' f           # 删掉以 # 开头的行
```

**find**

find命令过于复杂，先暂时用简化版fd

```bash
fd -e py          # 所有 .py
fd -e py -x wc -l # 对每个执行命令
```

**awk**

也是一门编程语言，常用操作如下

```bash
awk '{print $2}' file           # 打印第 2 列（默认空格分隔）
awk -F, '{print $1}' file       # 逗号分隔时打印第 1 列
awk '$3 > 100' file             # 只打印第 3 列大于 100 的行
awk '{sum += $1} END {print sum}' file  # 求和
```

**元字符**

```bash
date > file    # 将 date 的结果直接覆盖 file
date >> file   # 将 date 的结果追加到 file
date | ...     # 将 date 的结果直接作为后续程序的输入
ls /nonexistent /tmp 1> out.txt 2> err.txt    # 分开
ls /nonexistent /tmp 1> all.txt 2>&1          # 合并
```

**条件语句**

```bash
if  条件 ; then 命令; fi
for 变量 in 列表; do 命令; done
while 条件; do 命令; done
```

在循环中也有些特殊的命令

```bash
sleep x    # 休息 x 秒
ctrl + c   # 跳出死循环
$x         # x表示一个变量，这是在取值
seq 1 10   # 打印 1 到 10
seq 1 2 10 #打印 1 到 10，步长为2
for v in $(seq 1 10)  #利用$()表示执行的程序
```

批量改名

```bash
for i in *.txt; do mv "$i" "2026-$i"; done           # 加上同一前缀2026-
for i in 2026-*.txt; do mv "$i" "${i#2026-}"; done   # 删除同一前缀2026-
# 将所有.cpp文件改为 1.cpp 2.cpp ...
i=1; for f in *.cpp; do mv "$f" "$i.cpp"; i=$((i+1)); done
```

**引号**

```bash
echo "价格 $HOME"   # 双引号：变量被展开
echo '价格 $HOME'   # 单引号：原样输出
echo $'a\nb'        # $''：支持 \n 等转义
```

**新建**

```bash
touch file    # 新建文件
mkdir path    # 新建目录，若父目录不存在则创建失败
mkdir -p path # 若父目录不存在则一起创建
```
