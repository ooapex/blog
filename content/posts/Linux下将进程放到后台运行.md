---
Date: 2021-08-30
Draft: false
Categories:
  - Tech
Title: Linux下将进程放到后台运行
---

# Linux 下将进程放到后台运行

要想让一个程序在后台运行，有两种主要的方法:

1. 使用 `&` 搭配 `nohup`
2. 使用 screen 命令

## `&` 搭配 `nohup` 让程序在后台运行

### 切换成后台

```shell
nohup xxx.sh &
```

`&` 的作用是将程序由前台切换到后台，且没有了标准输入输出流。一般在脚本的后面加上 `&` 就能够将其切换成后台程序。

但是，此时的后台程序属于当前 shell 的一个子进程，也就是意味着当前连接的 shell 一旦关闭，就会将这个后台进程也关闭。

因此，需要使用 `nohup` 将这个进程从当前的 shell 中脱离出来，将此后台进程的进程号切换为 ppid 切换为 1，也就是父进程不再是当前的 shell 了。

这样，就算关闭当前的 shell 也不会关闭当前进程。

### 查看后台进程

当 shell 没有关闭时，可以使用 `jobs -l` 查看当前的后台进程。

当 shell 已经关闭了一次，有重新开了一个时，使用以上命令已经不能查看后台进程了，所以需要用 `ps -ef | grep processName` 来查看进程。

**在我实际的使用过程中，我发现即使我使用了 `nohup`，和 `&`，还是无法保证当前 shell 关闭后程序继续运行，因此我采用下面 screen 的方法**

## Screen 保证程序在后台运行

### 安装

一般系统不自带 screen，因此需要自己安装。

```shell
ubuntu/debian: sudo apt-get install screen
centos: yum install screen
```

### 常用命令

熟练的掌握以下命令，基本的常见就够用了。

```shell
screen -S <screenName>  # 新建并进入一个窗口，名字叫screenName
screen -list # 查看目前所有窗口的名字
screen -r <screenName> # 进入指定的窗口，注意要使用上一步list出来的全称
Ctrl+A+D # 退出当前窗口并挂起，窗口内的命令继续执行
Ctrl+A+X 或 exit # 关闭窗口，全关闭
```

### 使用场景

1. 执行 `screen -S test` 创建并自动一个新的 screen
2. 执行一个不会结束的进程 `top(CPU占用查看程序)`
3. 使用 `Ctrl+A+D` 退出窗口
4. 使用 `screen -list` 查看后台进程
5. 使用 `screen -r <screenName>` 进入窗口
6. 停止 `top` 程序，输入 `exit` 关闭当前窗口

Enjoy It~
