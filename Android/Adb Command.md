# Adb Command 

查看所有当前所有进程命令: `adb shell ps`

|列名|含义|
|:---|:---|
|USER	|所属用户|
|PID Process ID|	进程 ID|
|PPID Process Parent ID	|父进程 ID|
|VSIZE Virtual Size|	进程的虚拟内存大小|
|RSS Resident Set Size	|实际驻留”在内存中”的内存大小|
|WCHAN	|休眠进程在内核中的地址|
|NAME|进程名|

-  在Windows上筛选某个进程：`adb shell ps|findstr baidu`
- 在手机上筛选某个进程：`adb shell ps baidu 或者 adb shell ps|findstr -i baidu`
- android或者linux中的shell命令是grep：`adb shell ps|grepbaidu`