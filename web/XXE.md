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
http://localhost/check.php
Post data:
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


check.php
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


http://localhost/check.php
Post data:
xml=<?xml version='1.0' ?>  
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





### **元素的分类**

· <!ELEMENT element-name EMPTY>//空元素

· <!ELEMENT element-name (#PCDATA)>//文本元素

· <!ELEMENT element-name (e1,e2)>//混合元素

### **ATTLIST属性定义**

![](file:///C:\Users\37622\AppData\Local\Temp\ksohtml21892\wps1.jpg) 

# **实体**

实体是用于定义**引用普通文本或特殊字符**的快捷方式的变量。

## **预定义实体**

|   |   |   |
|---|---|---|
|<|<|小于|
|>|>|大于|
|&|&|和号|
|'|'|省略号|
|"|"|引号|


无法运行访问：
<?xml version='1.0' ?>  
<users>  
  <user id='1'>  
    <name>zhangsan></name>  
    <age>23&</age>  
    <gender>male"</gender>  
  </user>  
</users>


改成下面能正确运行

<?xml version='1.0' ?>  
<users>  
  <user id='1'>  
    <name>zhangsan></name>  
    <age>23&</age>  
    <gender>male"</gender>  
  </user>  
</users>


自定义实体xml：
<?xml version='1.0' ?>  
<!DOCTYPE note[  
<!ELEMENT note (name)>  
<!ENTITY zhangsan "zs">  
]>  
<users>  
  <user id='1'>  
    <name>&zhangsan;</name>  
    <age>23</age>  
    <gender>male</gender>  
  </user>  
</users>


## **参数实体**

只能在DOCTYPE里面使用的实体
01:

<?xml  version="1.0" encoding="utf-8"?>  
<!DOCTYPE  ROOT [  
     <!ELEMENT ROOT (A|B)*>  
     <!ENTITY  % A_atrr SYSTEM "A.dtd">  
     <!ENTITY  % B_atrr SYSTEM "B.dtd">  
     %A_atrr;  
     %B_atrr;  
]>  
<ROOT>  
     <A>  
         <NAME_A>  
             I am A!  
         </NAME_A>  
     </A>  
     <B>  
         <NAME_B>  
             I am B;  
         </NAME_B>  
     </B>  
</ROOT>  


02：

<?xml  version="1.0" encoding="utf-8"?>  
<!ELEMENT  A (NAME_A)>  
<!ELEMENT  NAME_A (#PCDATA)>  

<?xml  version="1.0" encoding="utf-8"?>  
<!ELEMENT  B (NAME_B)>  
<!ELEMENT  NAME_B (#PCDATA)>



防御手段：
1、打开PHP的libxml_disable_entity_loader(true),不允许加载外部实体。
2、过滤用户提交的XML数据，如ENTITY、SYSTEM。



