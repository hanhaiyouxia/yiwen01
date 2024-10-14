一、Cobalt Strike是一款美国Red Team开发的渗透测试神器。这个工具成为了渗透测试中不可缺少的利
器。其拥有多种协议主机上线方式，集成了提权，凭据导出，端口转发，socket代理，office攻击，文件捆绑，钓鱼等功能。同时，Cobalt Strike还可以调用Mimikatz等其他知名工具，因此广受黑客喜爱。
Cobalt Strike它分为客户端与服务端，服务端是一个，客户端可以有多个，可被团队进行分布式协团
操作。当然还有一点就是这款工具主要用于内网渗透以及APT攻击。

二、搭建环境演示：服务端linux，客户端Windows
kali:  
`cd server #切换到server目录`
`chmod +x teamserver`
`chmod +x TeamServerImage #赋予服务端运行权限`
`./teamserver 10.0.0.201 test #启动teamserver（服务端）IP地址为kali的地址，`



=============================================================
内部的Listener：
windows/beacon_dns/reverse_dns_txt
windows/beacon_http/reverse_http
windows/beacon_https/reverse_https
windows/beacon_bind_tcp
windows/beacon_bind_pipe
外部的Listener：
windows/foreign/reverse_http
windows/foreign/reverse_https
External
windows/beacon_ext

Beacon为内置的Listener，即在目标主机执行相应的payload，获取shell到CS上;其中包含DNS、HTTP、HTTPS、SMB。Beacon可以选择通过DNS还是HTTP协议出口网络，你甚至可以在使用Beacon通讯过程中切换HTTP和DNS。其支持多主机连接，部署好Beacon后提交一个要连回的域名或主机的列表，Beacon将通过这些主机轮询。目标网络的防护团队必须拦截所有的列表中的主机才可中断和其网络的通讯。通过种种方式获取shell以后(比如直接运行生成的exe)，就可以使用Beacon了。

Foreign为外部结合的Listener，常用于MSF的结合，例如获取meterpreter到MSF上。

Payload Staging
作为背景信息，一个值得一提的主题是 payload staging （分阶段传送 payload ）。在很多攻击框架的设计中，解耦了攻击和攻击执行的内容。payload 就是攻击执行的内容。 payload 通常被分为两部分： payload stage 和 payload stager 。 stager 是一个小程序 ，通常是手工优化的汇编指令，用于 下载一个 payload stage 、把它注入内存，然后对其传达执行命令。这个过程被称为 staging （分阶段）。
staging （分阶段）过程在一些攻击行动中是必要的。很多攻击中对于能加载进内存并在成功漏洞利用 后执行的数据大小存在严格限制。这会极大地限制你的后渗透选择，除非你分阶段传送你的后渗透 payload。
Cobalt Strike 在它的用户驱动攻击中使用 staging （分阶段）。大多数这类项目在 Attacks → Packages 和 Attacks → Web Drive - by 选项下。使用什么样的 stager 取决于与攻击配对 的 payload 。比如， HTTP Beacon 有一个 HTTP stager 。 DNS Beacon 有一个 DNS TXT 记录 stager 。 不是所有的 payload 都有 stager 选项。没有 stager 的 Payload 不能使用这些攻击选项投递。 如果你不需要 payload staging （分阶段），通过在你的 C2 拓展文件里把 host_stage 选项设为 false，你可以关闭这个选项。这会阻止 Cobalt Strike 在其 web 和 DNS 服务器上托管 payload stage。这种设置有助于提升行为安全（避免反溯源），因为如果开启了 staging （分阶段），任何人都能连。
HTTP Beacon 和 HTTPS Beacon
默认设置情况下， HTTP 和 HTTPS Beacon 通过 HTTP GET 请求来下载任务。这些 Beacon 通过 HTTP POST 请求传回数据
要起一个 HTTP 或 HTTPS Beacon 监听器，通过 Cobalt Stike → Listeners 。点击 Add 按钮，选择 Beacon HTTP 作为你的 payload 选项。

Windows Executable & Windows Executable(S)
这两个模块直接用于生成可执行的exe文件或dll文件。Windows Executable是生成Stager类型的马，而Windows Executable(S)是生成Stageless类型的马。那Stager和Stageless有啥区别呢?

Stager是分阶段传送Payload。分阶段啥意思呢?就是我们生成的Stager马其实是一个小程序，用于从服务器端下载我们真正的shellcode。分阶段在很多时候是很有必要的，因为很多场景对于能加载进内存并成功漏洞利用后执行的数据大小存在严格限制。所以这种时候，我们就不得不利用分阶段传送了。如果不需要分阶段的话，可以在C2的扩展文件里面把 host_stage选项设置为false。
而Stageless是完整的木马，后续不需要再向服务器端请求shellcode。所以使用这种方法生成的木马会比Stager生成的木马体积要大。但是这种木马有助于避免反溯源，因为如果开启了分阶段传送，任何人都能连接到你的C2服务器请求payload，并分析payload中的配置信息。在CobaltStrike4.0及以后的版本中，后渗透和横向移动绝大部分是使用的Stageless类型的木马。

原文链接：https://blog.csdn.net/qq_44159028/article/details/118157559
==============================================================


客户端：
sleep 1
shell whoami

cs插件
