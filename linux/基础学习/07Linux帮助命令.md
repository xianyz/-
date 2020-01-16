# 内建命令与外部命令
内建命令实际上是 shell 程序的一部分，其中包含的是一些比较简单的 Linux 系统命令，这些命令是写在bash源码的builtins里面的，由 shell 程序识别并在 shell 程序内部完成运行，通常在 Linux 系统加载运行时 shell 就被加载并驻留在系统内存中。而且解析内部命令 shell 不需要创建子进程，因此其执行速度比外部命令快。比如：history、cd、exit 等等。

外部命令是 Linux 系统中的实用程序部分，因为实用程序的功能通常都比较强大，所以其包含的程序量也会很大，在系统加载时并不随系统一起被加载到内存中，而是在需要时才将其调入内存。虽然其不包含在 shell 中，但是其命令执行过程是由 shell 程序控制的。外部命令是在 Bash 之外额外安装的，通常放在/bin，/usr/bin，/sbin，/usr/sbin等等。比如：ls、vi等。

type 命令来区分命令是内建的还是外部

```linux
type exit

type vim

type ls
```

```linux
#得到这样的结果说明是内建命令，正如上文所说内建命令都是在 bash 源码中的 builtins 的.def中
xxx is a shell builtin
#得到这样的结果说明是外部命令，正如上文所说，外部命令在/usr/bin or /usr/sbin等等中
xxx is /usr/bin/xxx
#若是得到alias的结果，说明该指令为命令别名所设定的名称；
xxx is an alias for xx --xxx
```

## help

本实验环境是 zsh，而 zsh 中内置并没有 help 命令，我们可以进入 bash 中，在 bash 中内置有该命令

```linux
bash
```

做好了以上的准备，我们就可以愉快的使用 help 命令了，我们可以尝试下这个命令:

```linux
help ls
```

 help 命令是用于显示 shell 内建命令的简要帮助信息。帮助信息中显示有该命令的简要说明以及一些参数的使用以及说明，一定记住 help 命令只能用于显示内建命令的帮助信息，不然就会得到你刚刚得到的结果。

那如果是外部命令怎么办，不能就这么抛弃它呀。其实外部命令基本上都有一个参数--help,这样就可以得到相应的帮助，看到你想要的东西了。试试下面这个命令是不是能看到你想要的东西了。

```linux
ls --help
```

## man

```linux
man ls
```

得到的内容比用 help 更多更详细，而且　man　没有内建与外部命令的区分，因为 man 工具是显示系统手册页中的内容，也就是一本电子版的字典，这些内容大多数都是对命令的解释信息，还有一些相关的描述。通过查看系统文档中的 man 也可以得到程序的更多相关信息和 Linux 的更多特性。

是不是好用许多，当然也不代表 help 就没有存在的必要，当你非常紧急只是忘记该用哪个参数的时候，help 这种显示简单扼要的信息就特别实用，若是不太紧急的时候就可以用 man 这种详细描述的查询方式

在尝试上面这个命令时我们会发现最左上角显示“ LS （1）”，在这里，“ LS ”表示手册名称，而“（1）”表示该手册位于第一章节。这个章节又是什么？在 man 手册中一共有这么几个章节

| 章节数 | 说明 |
| :----: | :----: |
| 1 | Standard commands （标准命令） |
| 2 | System calls （系统调用） |
| 3 | Library functions （库函数） |
| 4 | Special devices （设备说明） |
| 5 | File formats （文件格式） |
| 6 | Games and toys （游戏和娱乐） |
| 7 | Miscellaneous （杂项） |
| 8 | Administrative Commands （管理员命令） |
| 9 | 其他（Linux特定的）， 用来存放内核例行程序的文档。 |

打开手册之后我们可以通过 pgup 与 pgdn 或者上下键来上下翻看，可以按 q 退出当前页面

## info

要是你觉得man显示的信息都还不够，满足不了你的需求，那试试 info 命令，注意实验楼的环境中没有安装 info，可以手动安装，安装和操作步骤如下

```linux
# 安装 info
$ sudo apt-get update
$ sudo apt-get install info
# 查看 ls 命令的 info
$ info ls
```

得到的信息是不是比 man 还要多了，info 来自自由软件基金会的 GNU 项目，是 GNU 的超文本帮助系统，能够更完整的显示出 GNU 信息。所以得到的信息当然更多

man 和 info 就像两个集合，它们有一个交集部分，但与 man 相比，info 工具可显示更完整的　GNU　工具信息。若 man 页包含的某个工具的概要信息在 info 中也有介绍，那么 man 页中会有“请参考 info 页更详细内容”的字样
