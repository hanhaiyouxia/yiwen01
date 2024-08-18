1、XXE漏洞由XML文档引起。
2、发现漏洞：
抓包查看登录请求，发现登录的用户名和密码以XML格式进行传递，而服务器可以接受XML格式的信息，就表明可能存在XXE漏洞。

创建验证代码：

<?php  
libxml_disable_entity_loader(false);  
$xmlfile = file_get_contents('php://input');  //获取文件内容
//添加异常处理
$dom = new DOMDocument();  
$dom->loadXML($xmlfile, LIBXML_NOENT | LIBXML_DTDLOAD);  
$creds = simplexml_import_dom($dom);  
header('Content-Type: text/html; charset=utf-8');  
echo $creds;  
?>


防御手段：
1、打开PHP的libxml_disable_entity_loader(true),不允许加载外部实体。
2、过滤用户提交的XML数据，如ENTITY、SYSTEM。



