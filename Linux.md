# Linux

## 一、关机、重启

### 1.立即关机

> 指令 ：shutdown
>
> 指令 ：halt

### 2.定时关机

> 指令 ：shutdown -h [关机时间]
>
> 细节 ：以分钟为单位

### 3.重启

> 指令 ：shutdown -r now
>
> 指令 ：reboot

### 4.把内存中的数据同步到磁盘

> 指令 ：sync
>
> 细节 ：每次关机之前都应该同步数据到磁盘，保证数据不丢失

## 二、用户登录和注销

### 1.注销

>  指令 ：logout

## 三、用户管理

### 1.添加用户

> 指令 ：useradd [选项] 用户名
>
> 指令 ：useradd -d 指定目录名 用户名

### 2.删除用户

> 指令 ：userdel 用户名
>
> + 删除用户并保留家目录
>   + 指令 ： userdel 用户名
> + 删除用户不保留家目录
>   + 指令 ：userdel -r 用户名

### 3.查询用户信息

> 指令 ： id 用户名

### 4.切换用户

> 指令 ：su - 用户名
>
> + 注意 ：高权限用户切换到低权限用户时不需要密码
> + 指令 ：exit
>   + 退出到高权限用户

## 四、用户组管理

### 1.添加组

> 指令 ：groupadd 组名

### 2.删除组

> 指令 ：groupdel 组名

### 3.指定用户组

>指令 ：useradd -g 用户组 用户名

### 4.修改用户组

>usermod -g 新用户组 用户名

## 五、指令运行级别

1.运行级别说明： 

> 0 ：关机 
>
> 1 ：单用户【找回丢失密码】 
>
> 2：多用户状态没有网络服务 
>
> 3：多用户状态有网络服务 
>
> 4：系统未使用保留给用户 
>
> 5：图形界面 
>
> 6：系统重启
>
> `注意：常用运行级别是 3 和 5 ，要修改默认的运行级别可改文件 /etc/inittab 的 id:5:initdefault:这一行中的数字`



### 2.运行级别切换

>  指令 ：init [1234567]

### 3.面试题

> + 如何找回 root 密码，如果我们不小心，忘记 root 密码，怎么找回。 
>   + 思路： 进入到 单用户模式，然后修改 root 密码。因为进入单用户模式，root 不需要密码就可以登录。
>
> + 总结
>   + 开机->在引导时输入 回车键-> 看到一个界面输入 e -> 看到一个新的界面，选中第二行（编辑 内核）在输入 e-> 在这行最后输入空格，再输入1 ,再输入 回车键->再次输入 b ,这时就会进入到单用户模式。 这时，我们就进入到单用户模式，使用 passwd 指令来修改 root 密码。 

## 六、帮助指令

### 3.man命令

> 功能描述：获得帮助信息
>
> 指令 ：man [命令或配置文件]

### 2.help命令 

>功能描述：获得 shell 内置命令的帮助信息
>
>指令 ：help [命令或配置文件]

## 七、文件目录指令



### 1.pwd 命令

> 功能描述：显示当前工作目录的绝对路径
>
> 指令 ：pwd

### 2.ls命令

> + 基本语法
>   + ls [选项] [目录或是文件] 
>
> + 常用选项
>   + -a ：显示当前目录所有的文件和目录，包括隐藏的。 
>   + -l ：以列表的方式显示信息
>   + -h : 以人能看懂的方式显示

### 3.cd 命令

> + 功能描述：切换到指定目录
> + 基本语法
>   + cd [参数] (功能描述：切换到指定目录)
>
> 指令 ：cd ~ 或者 cd ：回到自己的家目录 
>
> 指令 ：cd .. 回到当前目录的上一级目录 

### 4.mkdir 命令

>+ 功能描述：用于创建目录
> + 基本语法 
> + mkdir [选项] 要创建的目录 
>+ 常用选项 
>  + -p ：创建多级目录

### 5.rmdir 命令

> + 功能描述：删除空目录
> + 基本语法 
>  + rmdir [选项] 要删除的空目录 
> + 使用细节
>   + rmdir 删除的是空目录，如果目录下有内容时无法删除的。 
>   + 提示：如果需要删除非空目录，需要使用 rm -rf 要删除的目录

### 6.touch 命令

> + 功能描述：创建空文件
> + 基本语法
>   + touch 文件名称

### 7.cp 命令

> + 功能描述：拷贝文件到指定目录
> + 基本语法 
>  + cp [选项] source dest 
> + 常用选项
>   + -r ：递归复制整个文件夹

### 8.rm 命令

> + 功能描述：移除【删除】文件或目录
> + 基本语法
>  + rm [选项] 要删除的文件或目录 
> + 常用选项
>   + -r ：递归删除整个文件夹 
>   + -f ： 强制删除不提示 

### 9.mv 命令

> + 功能描述：移动文件与目录或重命名
> + 基本语法
>   + mv oldNameFile newNameFile (重命名) 
>   + mv /temp/movefile /targetFolder (移动文件) 

### 10.cat 命令

> + 功能描述：查看文件内容，是以只读的方式打开
> + 基本语法
>  + cat [选项] 要查看的文件 
> + 常用选项
>
>   + -n ：显示行号
>+ 使用细节 
> 
>  + cat 只能浏览文件，而不能修改文件，为了浏览方便，一般会带上 管道命令 | more 
> 
>  + cat 文件名 | more （分页浏览）

### 11.more 命令

> + 功能描述：它以全屏幕的方式按页显示文本文件的内容
> + 描述 ：more 指令是一个基于 VI 编辑器的文本过滤器，它以全屏幕的方式按页显示文本文件的内容。more 指令中内置了若干快捷键，详见操作说明 
> + 基本语法
>   + more 要查看的文件

<img src="picture/more.png">

### 12.less 命令

> + 功能描述 : 分屏查看文件内容
> + 描述 ：less 指令用来分屏查看文件内容，它的功能与 more 指令类似，但是比 more 指令更加强大，支持 各种显示终端。less 指令在显示文件内容时，并不是一次将整个文件加载之后才显示，而是根据显示 需要加载内容，**对于显示大型文件具有较高的效率**。 
> + 基本语法
>   + less 要查看的文件

<img src="picture/less.png">

### 13.">" 和 ">>"命令

> + 功能描述：
>   + \> 输出重定向 : 会将原来的文件的内容覆盖 
>   + \>> 追加： 不会覆盖原来文件的内容，而是追加到文件的尾部
> + 基本语法
>   +  ls -l >文件 （列表的内容写入文件 a.txt 中（覆盖写））
>   + ls -al >>文件 （列表的内容追加到文件 aa.txt 的末尾） 
>   + cat 文件 1 > 文件 2 （将文件 1 的内容覆盖到文件 2）
>   + echo "内容" >> 文件 （将内容写入到文件中）
> + 说明：ls -l > a.txt , 将 ls -l 的显示的内容覆盖写入到 a.txt 文件，如果该文件不存在，就创建该文 件

### 14.echo命令 

> + 功能描述 ：输出内容到控制台
> + 基本语法
>   + echo [选项] [输出内容] 

### 15.head 命令

> + 功能描述 ：显示文件的开头部分内容，默认情况下 head 指令显示文件的前 10 行内容
> + 基本语法
>   + head 文件 (查看文件头 10 行内容) 
>   + head -n 5 文件 (查看文件头 5 行内容，5 可以是任意行数)

### 16.tail 命令

> + 功能描述 ：输出文件中尾部的内容，默认情况下 tail 指令显示文件的后 10 行内容
> + 基本语法
>   +  tail 文件 （查看文件后 10 行内容） 
>   + tail -n 5 文件 （查看文件后 5 行内容，5 可以是任意行数） 
>   + `tail -f 文件` (实时追踪该文档的所有更新，工作经常使用） 

### 17.ln 命令

> + 功能描述 ：软链接也叫符号链接，类似于 windows 里的快捷方式，主要存放了链接其他文件的路径
> + 基本语法
>   + ln -s [原文件或目录] [软链接名] （给原文件创建一个软链接）
>   + rm -rf 软链接名（注意：删除软链接文件时，不要带/，否则提示资源忙）

### 18.history 命令

> + 功能描述 ：查看已经执行过历史命令,也可以执行历史指令
> + 基本语法
>   + history （查看已经执行过历史命令） 
>   + ！行号 （执行指令）

### 19.date 命令

> + 功能描述 ：显示当前日期
>+ 基本语法
> +  date （显示当前时间） 
>  
> +  date +%Y （显示当前年份） 
>  
> + date +%m （显示当前月份） 
>  
> + date +%d （显示当前是哪一天） 
>   + date "+%Y-%m-%d %H:%M:%S"（显示年月日时分秒）
>   + date -s 字符串时间 （比如设置成 2018-10-10 11:22:22）

### 20.cal命令

>+ 功能描述 ：查看日历指令
>+ 基本语法
> + cal [选项] (不加选项，显示本月日历） 

### 21.find 命令

> + 功能描述 ：从指定目录向下递归地遍历其各个子目录，将满足条件的文件或者目录显示在终端
>
> + 基本语法
>
>   + find [搜索范围] [选项] 
>
>   + 按文件名：根据名称查找/home 目录下的 hello.txt 文件
>
>     + find /home -name hello.txt
>
>   + 按拥有者：查找/opt 目录下，用户名称为 nobody 的文件
>
>     + find /opt -user -nobody
>
>   + 查找整个 linux 系统下大于 20m 的文件（+n 大于 -n 小于 
>
>     n 等于）
>
>     + find / -size +20M
>     + find / -size -20M
>     + find / -size 20M
>     + find / -size +20480K
>
>   + 查询 / 目录下，所有 .txt 的文件
>
>   + find / -name *.txt

<img src="picture/find.png">

### 22.locate 命令

> + 功能描述 ：快速定位文件路径
> + 描述 ：locaate 指令可以快速定位文件路径。locate 指令利用事先建立的系统中所有文件名称及路径的 locate 数据库实现快速定位给定的文件。Locate 指令无需遍历整个文件系统，查询速度较快。为了保 证查询结果的准确度，管理员必须定期更新 locate 时刻。
> + 基本语法
>   + locate 搜索文件 
> + 特别说明 
>   + `由于 locate 指令基于数据库进行查询，所以第一次运行前，必须使用 updatedb 指令创建 locate 数 据库`。

### 23.grep命令和管道符号 |

> + 基本语法
>   + grep [选项] 查找内容 源文件

<img src="D:\user\notes\picture\grep01.png">

### 24.gzip/gunzip命令

> + 基本语法
>   + gzip 文件名 (压缩文件，只能将文件压缩为*.gz 文件)
>   + gunzip 文件.gz (解压缩文件命令)
> + 细节说明
>   + 当我们使用 gzip 对文件进行压缩后，不会保留原来的文件

### 24.zip/unzip命令

> + 基本语法
>   + zip [选项] XXX.zip 将要压缩的内容（压缩文件和目录的命令） 
>   + unzip [选项] XXX.zip （解压缩文件） 
>   + zip 常用选项 
>     + -r：递归压缩，即压缩目录
>   + unzip 的常用选项 
>     + -d<目录> ：指定解压后文件的存放目录 

### 25.tar命令

> + 功能描述：**是打包指令**，最后打包后的文件是 .tar.gz 的文件。
> + 基本语法 
>   + tar [选项] XXX.tar.gz 打包的内容 （打包目录，压缩后的文件格式.tar.gz)
> + 选项说明：

<img src="D:\user\notes\picture\tar01.png">

> + 案例：
>   + 将xxx.tar.gz解压到目录/opt
>   + tar -zxvf xxx.tar.gz -C /opt

### 26.chown命令

> + 功能描述 ：修改文件所有者
> + 基本语法 ：chown 用户名 文件名 
> + chown newowner:newgroup file 改变用户的所有者和所有组 

### 27.groupadd 命令

> + 功能描述 : 添加一个组
> + 基本语法 ：groupadd 组名

### 28.chgrp 命令

> + 功能描述 ：修改文件的组
> + 基本语法 ：chgrp 组名 文件名
> + 选项说明 ：-R 将文件下的所有文件及目录修改组

### 29.usermod命令

> + 功能描述 ：改变用户所在组
> + 基本语法 ：usermod -g 组名 用户名
> + `usermod -d 目录名 用户名 改变用户登录的初始目录`

### 30.chmod命令

> + 功能描述：修改文件的权限
> + 第一种方式：
>   + chmod  u=rwx,g=rx,o=x 文件目录名
>   + chmod o+w 文件目录名
>   + chmod a-x 文件目录名
> + 第二种方式：
>   + 规则：r=4 w=2 x=1 ,rwx=4+2+1=7 
>   + chmod u=rwx,g=rx,o=x 文件目录名 
>   + 相当于 chmod 751 文件目录名
>

## 八、crontab任务调度

> + 功能描述 ：任务调度，定时任务
>
> + 基本语法 ：crontab [选项]
> + 选项说明 ：

<img src="D:\user\notes\picture\crontab01.png">

> + 操作步骤 ：
>   + 设置任务调度文件：/etc/crontab 
>   + 设置个人任务调度。执行 crontab –e 命令。 
>   + 接着输入任务到调度文件 
>   + 如：*/1 * * * * ls –l /etc/ > /tmp/to.txt 
>   + 意思说每小时的每分钟执行 ls –l /etc/ > /tmp/to.txt 命令
>   + service crond restart 重启调度任务

> + 案例：定时任务，每分钟都将当前时间写到/tmp/date.txt中
>   + 编写shell脚本date.sh
>     + date "+%Y-%m-%d %H:%M:%S >> /tmp/date.txt
>   + 给date.sh可执行权限
>     + chmod 774 date.sh
>   + 编写任务调度
>     + crontab -e
>     + 添加 */1 * * * * /home/date.sh

## 九、Linux 磁盘分区、挂载

### 1.lsblk -f 指令

> + 功能描述：查看磁盘分区的挂载情况

### 2.增加一块硬盘并挂载

> + 虚拟机添加硬盘 
> + 分区 ： fdisk /dev/sdb 
> + 格式化  ：mkfs -t ext4 /dev/sdb1 
> + 挂载 先创建一个 /home/newdisk , 挂载 mount /dev/sdb1 /home/newdisk 
> + 设置可以自动挂载(永久挂载，当你重启系统，仍然可以挂载/home/newdisk)
> + vim /etc/fstab 
> + 添加 ：/dev/sdb1 /home/newdisk ext4 defaults 0 0

### 3.分区详细步骤：

> 分区命令 fdisk /dev/sdb 
>
> 开始对/sdb 分区 
>
> + m 显示命令列表 
>
> + p 显示磁盘分区 同 fdisk –l 
>
> + n 新增分区 
>
> + d 删除分区 
>
> + w 写入并退出 
>
> 说明： 开始分区后输入 n，新增分区，然后选择 p ，分区类型为主分区。两次回车默认剩余全部空间。最后输入 w 写入分区并退出，若不保存退出输入 q。
>
> umount 设备名称 或者 挂载目录 取消挂载

### 4.查询磁盘整体使用情况

> + 基本语法 ：df -h

### 5.查询指定目录的磁盘占用情况

> + 基本语法 ：du -h /目录
>
> + 查询指定目录的磁盘占用情况，默认为当前目录 
>
>   -s 指定目录占用大小汇总 
>
>   -h 带计量单位 
>
>   -a 含文件 
>
>   --max-depth=1 子目录深度
>
>   -c 列出明细的同时，增加汇总值

### 6.常用磁盘指令

#### 1.统计/home 文件夹下文件的个数

> ls -l /home | grep "^-" | wc -l

### 2.统计/home 文件夹下目录的个数 

> ls -l /home | grep "^d" | wc -l

### 3.统计/home 文件夹下文件的个数，包括子文件夹里的 

> ls -lR /home | grep "^-" | wc -l

### 4.统计文件夹下目录的个数，包括子文件夹里的

> ls -lR /home | grep "^d" | wc -l

## 十、网络配置

### 1.修改配置文件

> vi /etc/sysconfig/network-scripts/ifcfg-eth0 

<img src="D:\user\notes\picture\network.png">

> 重启服务 ：service network restart

## 十一、进程管理

### 1.ps 指令

> + 常用指令 ：ps -aux | grep xxx
> + 选项说明 ：
>   + ps -a : 显示当前终端所有进程
>   + ps -u : 以用户的格式显示进程信息
>   + ps -x  : 显示后台进程的参数  

### 2.ps -aux详情

<img src="D:\user\notes\picture\ps01.png">

> STAT：进程状态，其中 S-睡眠，s-表示该进程是会话的先导进程，N-表示进程拥有比普通优先 
>
> 级更低的优先级，R-正在运行，D-短期等待，Z-僵死进程，T-被跟踪或者被停止等等

### 3.ps -ef | more

> + 功能描述：以全格式显示所有进程
> + 参数说明 : 
>   + -e 显示所有进程
>   + -f 全格式
> + 思考题：如果我们希望查看 sshd 进程的父进程号是多少，应该怎样查询 ？
>   + ps -ef | grep sshd

### 4.kill/killall命令

> + 基本语法 : 
>   + kill [选项] 进程号（通过进程号杀死进程） 
>   + killall 进程名称 （通过进程名称杀死进程，也支持通配符，这在系统因负载过大而变 得很慢时很有用）
> + 参数说明 ：
>   + -9 :表示强迫进程立即停止

### 5.pstree命令

> + 功能描述 ：查看进程树
> + 基本语法 ： pstree [选项]
> + 参数说明 ：
>   + -p :显示进程的 PID
>   + -u :显示进程的所属用户 

### 6.服务管理

> + 基本语法 ：service 服务名 [start | stop | restart | reload | status] 
> + service iptables status 查看防火墙的状态
> + service iptables stop 关闭防火墙
> + service iptables start 开启防火墙
> + 说明 ：
>   + service命令是立即生效，但是系统关机后就失效了
>   + 在 CentOS7.0 后 不再使用 service ,而是 systemctl
> + 思考题 ：如何验证端口是否真的被启用了？
>   + 在window系统的dos命令窗口输入：
>     + telnet ip 端口号
> + 列出系统有那些服务：ls -l /etc/init.d/
> + setup (修改服务)

### 7.开机的流程说明：

> + 开机 --- > BOIS --- > /boot --- > init进程1 --- > 运行级别 --- > 运行级别对应的服务

### 8.chkconfig命令

> + 查询每个运行级别对应的服务状态
>   + chkconfig --list
> + 查询指定服务运行级别的服务状态
>   + chconfig --list | grep sshd
>   + chconfig 服务名 --list
> + 修改指定运行级别的状态
>   + chkconfig -level 运行级别 服务名 [off|on]
> + 修改所有运行级别的运行状态
>   + chkconfig 服务名 [off|on]
> + 注意 ：chkconfig 重新设置服务后自启动或关闭，需要重启机器 reboot 才能生效

### 9.top命令

> + 功能描述 ：显示正在执行的进程
> + 基本语法 ： top [选项]
> + 与ps命令的区别：Top 与 ps 最大的不同之处，在于 top 在 执行一段时间可以更新正在运行的的进程

#### 1.参数说明 ：

<img src="D:\user\notes\picture\top02.png">

<img src="D:\user\notes\picture\top01.png">

#### 2.交互操作说明：

<img src="D:\user\notes\picture\top03.png">

#### 3.监听特定的用户

> + top：输入此命令，按回车键，查看执行的进程。 
>
> + u：然后输入“u”回车，再输入用户名，即可

#### 4.终止指定的进程

> + top：输入此命令，按回车键，查看执行的进程。 
>
> + k：然后输入“k”回车，再输入要结束的进程 ID 号

#### 5.指定系统状态更新的时间(每隔 10 秒自动更新， 默认是 3 秒)

> + top -d 10

### 10.netstat命令

> + 功能描述 ：查看系统网络情况
> + 基本语法 ：netstat [选项] 
> + 常用选项 ：netstat -anp （查看所有网络服务）
> + 参数说明 ： 
>   + -an 按一定顺序排列输出 
>   + -p 显示哪个进程在调用

<img src="D:\user\notes\picture\netstat01.png">

## 十二、RPM和YUM

### 1.rpm命令

> + rpm -qa (查询所有安装)
> + 说明：
>   + 名称:firefox 
>   + 版本号：45.0.1-1
>   + 适用操作系统: el6.centos.x86_64 
>   + 表示 centos6.x 的 64 位系统 
>   + 如果是 i686、i386 表示 32 位系统，noarch 表示通用

<img src="D:\user\notes\picture\rpm01.png">

> + 常用命令 ：
>   + rpm -q 软件包名 （查询软件包是否安装）
>   + rpm -qi 软件包名 （查询软件包信息）
>   + rpm -ql 软件包名 （查询软件中的文件）
>   + rmp -qf  文件全路径名 （查询文件所属的软件包）
>   + rpm -e RPM的包的名称 （卸i软件）
>     + 细节说明：
>       + 如果其他软件包依赖于你卸载的软件包，卸载时则会产生错误信息
>       + 需 加上--nodeps 强制删除
>   + rpm -ivh RPM包全路径名称
>     + 参数说明：
>       + i=install 安装 
>       + v=verbose 提示 
>       + h=hash 进度条 

### 2.安装firefox案例

<img src="D:\user\notes\picture\rpm02.png">

<img src="D:\user\notes\picture\rpm03.png">

<img src="D:\user\notes\picture\rpm04.png">

<img src="D:\user\notes\picture\rpm05.png">

### 3.yum命令

#### 1.CentOS6 的yum源说明

> + ###### `CentOS6`停止维护更新日期2020年11月30日
>
> + ###### 2020年12月2日下架了包括官方所有的`CentOS6`源（包括国内的镜像站）
>
> + ###### 所以这里使用`centos-vault`作为更新源

#### 2.CentOS6的yum源配置

> + 进入：cd /etc/yum.repo.d
> + 编辑：vim CentOS-Base.repo

```tex
[base]
name=CentOS-6.10 - Base - mirrors.aliyun.com
failovermethod=priority
baseurl=http://mirrors.aliyun.com/centos-vault/6.10/os/$basearch/
gpgcheck=1
gpgkey=http://mirrors.aliyun.com/centos-vault/RPM-GPG-KEY-CentOS-6
#released updates
[updates]
name=CentOS-6.10 - Updates - mirrors.aliyun.com
failovermethod=priority
baseurl=http://mirrors.aliyun.com/centos-vault/6.10/updates/$basearch/
gpgcheck=1
gpgkey=http://mirrors.aliyun.com/centos-vault/RPM-GPG-KEY-CentOS-6
#additional packages that may be useful
[extras]
name=CentOS-6.10 - Extras - mirrors.aliyun.com
failovermethod=priority
baseurl=http://mirrors.aliyun.com/centos-vault/6.10/extras/$basearch/
gpgcheck=1  
gpgkey=http://mirrors.aliyun.com/centos-vault/RPM-GPG-KEY-CentOS-6
#additional packages that extend functionality of existing packages
[centosplus] 
name=CentOS-6.10 - Plus - mirrors.aliyun.com
failovermethod=priority 
baseurl=http://mirrors.aliyun.com/centos-vault/6.10/centosplus/$basearch/
gpgcheck=1
enabled=0
gpgkey=http://mirrors.aliyun.com/centos-vault/RPM-GPG-KEY-CentOS-6
#contrib - packages by Centos Users
[contrib]
name=CentOS-6.10 - Contrib - mirrors.aliyun.com
failovermethod=priority  
baseurl=http://mirrors.aliyun.com/centos-vault/6.10/contrib/$basearch/
gpgcheck=1
enabled=0
gpgkey=http://mirrors.aliyun.com/centos-vault/RPM-GPG-KEY-CentOS-6

```

#### 3.清楚yum缓存

> + yum clean all

#### 4.建立缓存

> + yum makecache

#### 5.查询

> + yum list | grep xxx (查询yum源是否有xxx)
> + eg. yum list | grep firefox

#### 6.安装

> + yum install 复制查询出来的带版本的包
> + eg. yum install firefox.x86_64