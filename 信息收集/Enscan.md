1. 安装依赖 (部分依赖有点问题，需要手动安装下)
    
    `pip install -r requirements.txt`
    
2. 命令使用
    
    `python ENScan.py`
    
    输入关键词即可查询
    
3. 批量查询
    
    `python ENScan.py -k keyword.txt` keyword.txt 为你自己制定的企业关键词，每行一个
    

若使用出现 “【风险校验】需要更新Cookie” 信息需要使用浏览器打开[https://aiqicha.baidu.com/](https://aiqicha.baidu.com/) 滑动验证码校验后复制所有COOKIE信息替换脚本中的 self.cookie （若不知道替换哪些请看视频教程演示）

`self.invest_is_rd = False` 是否选出不清楚投资比例的，比如未公示投资多少的，开启会增加发现几率，但可能会不对应

**请求错误会自动尝试重连20次** ，若替换Cookie无果可以尝试开启代理，速度会偏慢