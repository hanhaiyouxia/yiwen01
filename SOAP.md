漏洞一般出现利用SOAP服务的站点，即通过访问网站后，其中的一些数据通过xml格式来调用，一般存在目录浏览器或可以通过目录浏览器漏洞来验证。
1、列目录漏洞
一种是网站直接列目录漏洞；
另一种是通过程序构造，如KindEditor的列目录漏洞，通过这些漏洞来寻找扫描目标站点中的asmx文件。
2、site:ym.all95.cn filetype:asmx
3、确认asmx利用了SOAP服务
4、利用AWVS进行Web Service 扫描
5、利用SQLzMap进行注入测试确认
