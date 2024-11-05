安装
注意需要使用 Python2 的环境，建议使用 python-2.7.14.msi
Python27\Scripts
pip.exe install dnspython gevent -i https://pypi.douban.com/simple

参数介绍
#-version 显示版本号并退出
#-h, --help 帮助
#-f FILE 指定暴力猜解字典（默认使用 subnames.txt）
#-full 全扫描，使用 subnames_full.txt
#-i, --ignore-intranet 忽略不想采集的 IP 地址
#-t THREADS, --threads=THREADS 扫描线程数，默认 200
#-p PROCESS, --process=PROCESS 扫描进程数，默认为 6
#-o OUTPUT, --output=OUTPUT
输出文件名称 {target}.txt


E:\Python27\python.exe subDomainsBrute.py -t 20 vivo.com.cn -o xx.txt