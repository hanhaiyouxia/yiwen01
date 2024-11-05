![[Pasted image 20241102174810.png]]

![[Pasted image 20241102192535.png]]



1、检测目标是否存在cdn


ping、nslookup

2、子域名收集:在线网站、subDomainsBrute、Oneforall
```
使用在线工具收集
https://tool.chinaz.com/subdomain/vivo.com.cn
https://tool.chinaz.com/subdomain/baidu.com

https://site.ip138.com/vivo.com.cn/domain.htm

https://chaziyu.com/vivo.com.cn/


subDomainsBrute 工具
安装
注意需要使用 Python2 的环境，建议使用 python-2.7.14.msi
pip.exe install dnspython gevent -i https://pypi.douban.com/simple
参数介绍
#-version 显示版本号并退出

#-h, --help 帮助

#-f FILE 指定暴力猜解字典（默认使用 subnames.txt）

#-full 全扫描，使用 subnames_full.txt

#-i, --ignore-intranet 忽略不想采集的 IP 地址

#-t THREADS, --threads=THREADS 扫描线程数，默认 200

#-p PROCESS, --process=PROCESS 扫描进程数，默认为 6

#-o OUTPUT, --output=OUTPUT

输出文件名称 {target}.txt


使用
E:\Python27\python.exe subDomainsBrute.py -t 20 vivo.com.cn -o xx.txt




Oneforall
https://github.com/shmilylty/OneForAll
python3 oneforall.py --target ym.all95.cn  --show True run

```


3、利用子域名解析获取真实ip
    1.使用在线工具或 DNS 解析工具（如 `nslookup` 或 `dig`）解析目标域的子域。
    2.有些子域不通过 CDN，可能直接指向源 IP 地址。
    3.查找与“admin”或“dev”类似的子域，有时可以找到未被 CDN 覆盖的地址。
    4.网站可获取真实ip:  https://get-site-ip.com/
    





```


**方法：**

1）Google输入
site:baidu.com -www
就能查看除www外的子域名

2）微步在线([https://x.threatbook.cn/](https://x.threatbook.cn/))

3）钟馗之眼，[shodan](https://www.shodan.io/)，[fofa搜索]([https://fofa.so/](https://fofa.so/)

##### 2、邮件服务

1）QQ邮箱  

##### 3、国外地址请求

很多的企业没有在国外部署CDN，要是用国外的地址请求、就容易找到他的真实地址。

##### 4、网站漏洞查找

1）目标敏感文件泄露，例如：phpinfo[浅谈“phpinfo()信息泄漏”_phpinfo函数泄露的信息有哪些-CSDN博客](https://blog.csdn.net/weixin_39934520/article/details/107022162)之类的探针、GitHub信息泄露等。  
2）XSS盲打，命令执行反弹shell，SSRF等。  
3）无论是用社工还是其他手段，拿到了目标网站管理员在CDN的账号，从而在从CDN的配置中找到网站的真实IP。


```

![[Pasted image 20241102222617.png]]

