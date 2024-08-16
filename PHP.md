1、高危函数：

| phpinfo()           | 返回php所有信息                |
| ------------------- | ------------------------ |
| exec()              | 执行外部程序或外部指令              |
| eval()              | 执行php代码                  |
| system()            | 执行外部程序，并且显示输出            |
| shell_exec()        | 执行外部程序或外部指令，与exec()类似    |
| array_map()         | 返回用户自定义函数作用后的数组          |
| include()           | 获取并运行外部程序                |
| include_once()      | 获取并运行外部程序，与include类似     |
| require()           | 包含函数，与include()区别是一开始就运行 |
| file_get_contents() | 把整个文件读入一个字符串中            |
| file()              | 把整个文件读入一个数组中             |
| proc_open()         | 执行一个命令，并打开指针             |
| extract()           | 对已存在的变量进行覆盖              |
|                     |                          |
|                     |                          |

2、常见传参函数：

| $_SERVER  | 服务器环境变量数组                     |
| --------- | ----------------------------- |
| $_GET     |                               |
| $_POST    |                               |
| $_COOKIE  | COOKIE变量数组                    |
| $_REQUEST | 包含了\$_GET、\$_POST、$_COOKIE等数组 |
| $_FILES   | 文件上传数组                        |
| $_ENV     | 环境变量数组                        |
|           |                               |
|           |                               |
|           |                               |


3、%00节段需要magic_quotes_gpc=off,PHP版本小于5.3.4时才能实现。