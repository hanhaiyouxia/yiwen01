dirsearch -u http://xxxx   -e php
--random-user,可以在扫描目录时使用随机请求头来降低被防守方发现的概率
dirsearch -u http://xxxx   -e php --random-user
-r代表递归查询，-R(1,2,3)代表深度的等级
dirsearch -u http://xxxx   -e php -r -R