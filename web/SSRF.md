SSRF(Server-Side Request Forgery,服务器请求伪造)是一种由攻击者构造请求,由服务端发

起请求的安全漏洞,一般情况下,SSRF 攻击的目标是外网无法访问的内网系统(正因为请求时由

服务端发起的,所以服务端能请求到与自身相连而与外网隔绝的内部系统)。

攻击者通过篡改 URL 路径遍历或完全替换它来操纵 URL， 以向他们选择的任意域发出 HTTP

请求。 SSRF 攻击可以利用链接到组织基础架构内，不应向用户公开的内部服务或用户不应访问外部系统的URL。


新建ssrf.php：
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

file://C:\Users\86188\Desktop\tool\pwd.txt


防御手段：
1、禁止跳转。
2、限制协议。
3、内外网限制。
4、URL限制。