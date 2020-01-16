# 友情推荐

[例行性工作排程(crontab) - 鸟哥私房菜](http://linux.vbird.org/linux_basic/0430cron.php)

[Linux Crontab 百度百科](https://baike.baidu.com/item/crontab/8819388?fr=aladdin)

# crontab 的使用

## crontab 简介

crontab 命令从输入设备读取指令，并将其存放于 crontab 文件中，以供之后读取和执行。通常，crontab 储存的指令被守护进程激活，crond 为其守护进程，crond 常常在后台运行，每一分钟会检查一次是否有预定的作业需要执行。

通过 crontab 命令，我们可以在固定的间隔时间执行指定的系统指令或 shell　script 脚本。时间间隔的单位可以是分钟、小时、日、月、周的任意组合。

这里我们看一看crontab 的格式

```linux
# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
```

## crontab 准备

crontab 在本实验环境中需要做一些特殊的准备，首先我们会启动 rsyslog，以便我们可以通过日志中的信息来了解我们的任务是否真正的被执行了

```linux
sudo apt-get install -y rsyslog
sudo service rsyslog start
```

crontab 也是不被默认启动的，同时不能在后台由 upstart 来管理

```linux
sudo cron －f &
```

## crontab 使用

通过下面一个命令来添加一个计划任务

```linux
crontab -e
```

*可使用man crontab偷偷查看*

通过这样的一个例子来完成一个任务的添加，在文档的最后一排加上这样一排命令,该任务是每分钟我们会在/home/shiyanlou目录下创建一个以当前的年月日时分秒为名字的空白文件

在文件中添加如下文本

```linux
*/1 * * * * touch /home/shiyanlou/$(date +\%Y\%m\%d\%H\%M\%S)
```

*注意 “ % ” 在 crontab 文件中，有结束命令行、换行、重定向的作用，前面加 ” \ ” 符号转义，否则，“ % ” 符号将执行其结束命令行或者换行的作用，并且其后的内容会被做为标准输入发送给前面的命令。*

添加成功后我们会得到最后一排 installing new crontab 的一个提示

当然我们也可以通过这样的一个指令来查看我们添加了哪些任务

```linux
crontab -l 
```

我们添加了任务，但是如果 cron 的守护进程并没有启动，它根本都不会监测到有任务，当然也就不会帮我们执行，我们可以通过以下2种方式来确定我们的 cron 是否成功的在后台启动，默默的帮我们做事，若是没有就得执行上文准备中的第二步了

```linux
ps aux | grep cron

or

pgrep cron
```

*通过ll语句可以确认已经每分钟生了一个对应的文件*

我们通过这样一个命令可以查看到执行任务命令之后在日志中的信息反馈

```linux
sudo tail -f /var/log/syslog
```

*可以看到每分钟的指令执行*

当我们并不需要这个任务的时候我们可以使用这么一个命令去删除任务

```linux
crontab -r
crontab -l
```

-r删除-l查看任务列表中是否还有任务

# crontab 的深入的深入了解

每个用户使用 crontab -e 添加计划任务，都会在 /var/spool/cron/crontabs 中添加一个该用户自己的任务文档，这样目的是为了隔离

如果是系统级别的定时任务，应该如何处理？只需要以 sudo 权限编辑 /etc/crontab 文件就可以。

cron 服务监测时间最小单位是分钟，所以 cron 会每分钟去读取一次 /etc/crontab 与 /var/spool/cron/crontabs 里面的內容。

在 /etc 目录下，cron 相关的目录有下面几个

![cron 相关的目录](https://dn-simplecloud.shiyanlou.com/1135081468206856712 "cron 相关的目录")

每个目录的作用：

/etc/cron.daily，目录下的脚本会每天执行一次，在每天的6点25分时运行；

/etc/cron.hourly，目录下的脚本会每个小时执行一次，在每小时的17分钟时运行；

/etc/cron.monthly，目录下的脚本会每月执行一次，在每月1号的6点52分时运行；

/etc/cron.weekly，目录下的脚本会每周执行一次，在每周第七天的6点47分时运行；

系统默认执行时间可以根据需求进行修改。

