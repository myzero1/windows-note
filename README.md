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
