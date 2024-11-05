"pikachu" && country="CN" && title="Get the pikachu"
domain="xiangxueketang.cn"
body="内容"

https://quake.360.cn
https://www.shodan.io
https://www.zoomeye.org

社工信息
爱企查
https://aiqicha.baidu.com/

企查查
https://www.qcc.com/

备案信息
https://beian.miit.gov.cn/


# 资产收集
enscan+oneforall
灯塔


## 01-公益src挖掘

```
资产搜集：
Googlehacking语法
https://www.cnblogs.com/xyztank/articles/12638138.html
资产搜索引擎
fofa、quake、hunter、zoomeye
1、漏洞点sql注入
2、源码泄露
3、thinkphp框架漏洞
header="thinkphp" && title="后台管理"
header="thinkphp" && country="CN"
"index/login/login"
body="十年磨一剑" && body="ThinkPHP" && country="CN"
header="thinkphp" && country="CN" && title="后台管理"
加上时间，现在新网站有thinkphp日志泄露的有很多。
header="thinkphp" && country="CN" && title="后台管理" && after="2021-01-01"
app="ThinkPHP"

4、xss漏洞挖掘

```




# 02-教育src
```
一. 利用fofa 语句：“xx” && org=“China Education and Research Network Center”
1、后台挖掘思路
第一个就是弱口令操作：admin /admin admin/123456 admin/admin888 这样的，最好的方法就是自己积累一个常用字典，当然还可以去全网寻找这一套模板的管理员手册，在后面就是github去寻找一下，最后都没有办法的话,那就放弃
第二个自然就是top10：万能密码（sql）、xss漏洞的挖掘
第三个：逻辑漏洞分析
首先还是先使用f12查看页面源码，说不定管理员密码写在页面中的！
然后可以注意到功能点是密码重置点
1. 第一个获取登录数据包进行修改返回包看看是否可以成功登录，如果登录成功就是逻辑一个，
不能登录成功那就绕开进行下一步测试
2. 第二个操作就是我们分析js，查找重置密码的接口，看看接口是否存在未授权



二 利用信息收集获取重要信息进入门户
1.Googlehacking语法
site:xxxxxx.edu.com sfz（手机号、账号、学号、密码......） filetype:.doc | .docx | .xls | .xlsx | .ppt| .pptx | .odt | .pdf | .rtf | .sxw | .psw | .csv
2.表白墙、抖音、微信公众、教育局公开信息...
3.利用谷歌语法查找脆弱的系统获取信息
site:xxx.edu.cn 初始密码
扩大范围：
思路一、登录vpn，干内网
思路二、尝试使用爆出来的弱密码都登录其它系统

3.代码审计
一般的源码获取地方都在是咸鱼，百度网盘，github等地方可以看看是否能获取
直接审计各类oa，这些oa的源码通常都可以在官网获取https://vuls.vercel.app/PeiQi/readme/






```


03-企业src

```
一、逻辑漏洞
##注册功能点
1、任意验证码注册
2、只要验证码正确随便输入手机号注册
3、删除验证码或验证码参数造成任意注册
4、短信轰炸

##登录功能点
1、任意验证码登录
2、只要验证码正确随便输入手机号登录
3、删除验证码或验证码参数造成任意登录


##修改密码功能点
1、两个页面的未授权
2、越权

##未授权
1、修改或删除cookie值
2、扫目录

##文件上传
1、上传html类型xss
2、pdf类型xss


二、springboot接口泄漏
springboot作用：解决一些复杂业务的框架
特征：Whitelabel Error Page
/actuator               查看有哪些 Actuator端点是开放的。
/actuator/auditevent    auditevents端点提供有关应用程序审计事件的信息。
/actuator/beans         beans端点提供有关应用程序 bean 的信息。
/actuator/conditions    conditions端点提供有关配置和自动配置类条件评估的信息。
/actuator/configprops   configprops端点提供有关应用程序@ConfigurationPropertiesbean的信息。

/actuator/env           查看全部环境属性，可以看到 SpringBoot 载入哪些 properties，以及 properties 的值（会自动用*替换 key、password、secret 等关键字的 properties 的值）。

/actuator/flyway        flyway端点提供有关 Flyway 执行的数据库迁移的信息。
/actuator/health        端点提供有关应用程序运行状况的health详细信息。
/actuator/heapdump      heapdump端点提供来自应用程序 JVM 的堆转储。(通过分析查看/env端点被*号替换到数据的具体值。)
/actuator/httptrace     httptrace端点提供有关 HTTP 请求-响应交换的信息。（包括用户HTTP请求的Cookie数据，会造成Cookie泄露等）。

/act                       info端点提供有关应用程序的一般信息。
/actuator/integrationgraph integrationgraph端点公开了一个包含所有 Spring Integration 组件的图。
/actuator/liquibase         liquibase端点提供有关 Liquibase 应用的数据库更改集的信息。
/actuator/logfile           logfile端点提供对应用程序日志文件内容的访问。
/actuator/loggers           loggers端点提供对应用程序记录器及其级别配置的访问。
/actuator/mappings          mappings端点提供有关应用程序请求映射的信息。

/actuator/metrics           metrics端点提供对应用程序指标的访问。/actuator/prometheus端点以prometheusPrometheus 服务器抓取所需的格式提供 Spring Boot 应用程序的指标。

/actuator/quartz            quartz端点提供有关由 Quartz 调度程序管理的作业和触发器的信息。
/actuator/scheduledtasks    scheduledtasks端点提供有关应用程序计划任务的信息。
/actuator/sessions          sessions端点提供有关由 Spring Session 管理的应用程序 HTTP 会话的信息。

/actuator/startup           startup端点提供有关应用程序启动顺序的信息。
/actuator/shutdown          shutdown端点用于关闭应用程序

三、从目标拓展挖掘的资产
dirsearch路径的扫描
webpacker
sql的fuzz获取heapdump

获取heapdump
通过泄露信息扩大危害



```


04-证书系列（cnvd和cve）
```
挖掘cnvd
条件：
对于有证书的收录标准事件型
事件型漏洞必须是三大运营商（移动、联通、电信）的中高危漏洞，或者党政机关、重要行业单位、科研院所、重要企事业单位（如：中央国有大型企业、部委直属事业单位等）的高危事件型漏洞才会颁发
原创漏洞证书

通用型
中危及中危以上的通用性漏洞（CVSS2.0基准评分超过4.0）
软件开发商注册资金大于等于5000万人民币或者涉及党政机关、重要行业单位、科研院所、重要企事业
单位（如：中央国有大型企业、部委直属事业单位等）的高危事件型漏洞
通用型漏洞得十个URL案例
资产收集：
1、https://www.zol.com.cn/
2、https://aiqicha.baidu.com/
3、fofa、hunter、quaker.......


挖掘cve
找源码网站：
https://down.chinaz.com/
https://github.com/
提交教程：http://cn-sec.com/archives/337616.html
新建个github账号，新建项目把复现过程写到里面
https://github.com/
查cve编号：https://www.cnnvd.org.cn/home/globalSearch?keyword=semcms
用：Acunetix扫描全部网站也可以用Seay源代码审计系统审计也可以，挖个sql注入就可以交





```



x05-企业src打排名
```
找一些小的src平台
https://security.zhipin.com/
https://src.ucloud.cn/
```