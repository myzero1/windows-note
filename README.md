# windows-note

##windows定时锁屏脚本
```pre
定时锁屏任务

rundll32.exe user32.dll,LockWorkStation


当 Microsoft Windows 安全审核。  4648时调用定时锁屏任务

查看事件：控制面板\所有控制面板项>管理工具》事件查看器》window日志》安全》 Microsoft Windows 安全审核  ------  4648


添加计划任务：控制面板\所有控制面板项>管理工具》任务计划程序
在计划任务中执行lock脚本


-----------start_lock.cmd------------

@echo off 
if "%1"=="h" goto begin 
start mshta vbscript:createobject("wscript.shell").run("""%~f0"" h",0)(window.close)&&exit 
:begin

title=startlock

TASKKILL /F /FI "WINDOWTITLE eq 管理员: =mylock"

start "=mylock" %~dp0lock.cmd


--------------lock.cmd---------------

@echo off 
if "%1"=="h" goto begin 
start mshta vbscript:createobject("wscript.shell").run("""%~f0"" h",0)(window.close)&&exit 
:begin

title=mylock

ping 127.1 -n 3600 >nul
rundll32.exe user32.dll,LockWorkStation


```



##win7 该任务映像已损坏或已篡改。（异常来自HRESULT:0x80041321）
```pre
如上面所示，如SessionAgent这个任务损坏

①我们需要到C:\Windows\System32\Tasks 这个文件夹里面进行搜索，把它所在的文件夹记录下面。

对应在任务计划里面的位置是：Microsoft\Windows\SideShow。

②我们把这个SessionAgent的文件，剪切到别的地方，对的！ 是剪切，不是复制 。比如剪切到桌面。

③把SessionAgent这个文件改成SessionAgent.xml，把文件改成XML文件。

④ 再到任务计划里面定位到刚才的位置：Microsoft\Windows\SideShow


点击导入任务，把刚才桌面的SessionAgent.xml导入即可。如果导入时弹出错误对话框，一般是版本号不对，我们直接打开文件，把里面的版本号修改一下即可。


如上图，把1.5改成1.3就可以了。记得保存文件(Ctrl+S)！然后再次导入，然后就成功把任务导入进去了。

然后对每一个损坏的任务都需要这样的操作。不小心导入错位置其实也可以的。任务计划里面的文件夹的位置其实只是为了方便管理而已。

```
