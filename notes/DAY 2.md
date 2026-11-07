**参数**

```bash
cat >> arg.sh << 'EOF'   # 键盘输入 arg.sh，输入 EOF 结束
# 脚本的内容如下，只要是 bash 脚本第一行都这么写，第二行的三4个变量是固定用法
#!/bin/bash
echo "脚本名=$0  参数个数=$#  全部参数=$@  上一条命令的返回码=$?"
echo "第一个=$1  第二个=$2  第三个=$3"     # 未传入的参数为空串
```

```bash
touch -123    # 会报错，- 会被当成选项
touch -- -123 # 这样才能创建 - 开头的文件
rm -- -123    # 删除同理
```

**流**

每个程序有三个流

| 编号  | 名字     | 方向    |
| --- | ------ | ----- |
| 0   | stdin  | 入     |
| 1   | stdout | 出（正常） |
| 2   | stderr | 出（报错） |

```bash
ls /nonexistent /tmp > out.txt 2> err.txt   # 注意有两个参数
cat out.txt; cat err.txt
ls /nonexistent /tmp | grep tmp      # stderr 绕过管道，stdout 作为下一个输入
ls /nonexistent 2>/dev/null          # 不输出报错信息
ls /usr/bin | tee list.txt | wc -l   # 记录文件名称并数行数（这里就是文件数）
ls | fzf           # 提供输入选项
nvim $(ls | fzf)   # 选择文件进行编辑
```

<是把0号流从键盘到文件，>是把1号流从屏幕到文件

**环境变量**

赋值和取值

```bash
foo=bar        # 不要打空格！！！
echo $foo      # 输出 bar
echo "$foo"    # 输出 bar
echo '$foo'    # 输出 $foo
```

export：让该变量在子进程中也能被读到

```bash
export foo
bash -c 'echo $foo'    # 开启一个子进程
printenv foo           # 查看 export 的变量
```

进程替换

```bash
diff <(ls a) <(ls b)  
# diff 只能传入文件，要比较两个目录下内容的差异，就先用 < 将 ls 的结果生成临时文件
```

**返回码**

具体数值每个程序不同，通用标准只有 0 = 成功， ！0 = 失败  
&& 表示成功才执行， || 表示失败才执行

```bash
cat > t.sh <<'EOF'
#!/bin/bash
[[ -f "$1" ]] || { echo "没这个文件" >&2; exit 1; }
echo "有"
EOF
bash t.sh /etc/passwd; echo $?   # 0
bash t.sh /nope;      echo $?    # 1
```

```bash
false | true; echo $?    # 0 ← 明明前面失败了
set -o pipefail
false | true; echo $?    # 1 ← 任何一个失败就算失败
```

**信号**

```bash
sleep 100        # Ctrl+Z 挂起，Ctrl+C 杀掉
jobs             # 展示当前工作
bg %1            # 转后台继续跑
fg %1            # 拉回前台
```

```bash
[1]   Stopped                    sleep 200
[2]-  Stopped                    sleep 100
[3]+  Stopped                    sleep 300
```

表示最新操作是 3 号工作，次新的是 2 号工作

```bash
sleep 300 &      # 后台启动，屏幕会打印 pid
pgrep -lf sleep  # 按名字搜进程
pkill sleep      # 按名字杀进程
kill pid         # 按 pid 杀进程
nohup sleep 300 &    # 关终端也不死
```

**tmux**

```bash
tmux new -s work      # 建一个叫 work 的会话，是终端中的终端，退出不打断
```

**软链接**

```bash
mkdir -p ~/dotfiles                 # 创建一个新的目录
cp ~/.bashrc ~/dotfiles/bashrc      # 复制一份到新的目录
mv ~/.bashrc ~/.bashrc.bak          # 原地留个备份
ln -s ~/dotfiles/bashrc ~/.bashrc   # 创建快捷方式，先写目标，后写文件名
```
