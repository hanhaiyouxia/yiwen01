Ollydbg
windows函数GetDlgItemTextA()和lsstrcom()是关键代码
GetDlgItemTextA()
断点到了后，Alt+F9返回到用户代码，004011AE下断点，Alt+B调出断点窗口，禁止GetDlgItemTextA()
004011AE----004011F5，将JE指令NOP掉，即用机器码90代替74和37。
