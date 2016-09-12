
## windows bat自动安装apk命名
* 使用方法直接拖app到bat文件即可

```
@ECHO OFF
ECHO [安装APK,需要配置好Android sdk环境]
ECHO -------------------------------
ECHO [等待插入手机...]
adb wait-for-device
ECHO [安装] %~nx1
adb install -r %1
ECHO [暂停3秒自动关闭...]
ping -n 3 127.0.0.1>nul
```

## 查看apk的基本信息

```
@ECHO OFF
ECHO [查看APK包信息]
ECHO -------------------------------
ECHO aapt dump badging %~nx1
aapt dump badging %1 > %~dp0%~n1.txt
ECHO [暂停3秒自动关闭...]
ping -n 3 127.0.0.1>nul
@ECHO ON 

```

## 分析monkey的log

```
@ECHO OFF

ECHO.:::::::::::::::::::::::::::::::::::::::::::::::::

ECHO.::             分析Monkey日志                  ::

ECHO.::             作者：Findyou                   ::

ECHO.::             版本  V1.0.1                    ::

ECHO.::             时间：2014.08.26                ::

ECHO.:::::::::::::::::::::::::::::::::::::::::::::::::

REM 方法一：手动设置Monkey日志路径

SET monkeyLogFile=F:\Monkey\20140808\FindyouV1.0.0\20140825181801_monkey.log



REM 方法二：直接将Monkey日志拖到此bat文件上

IF NOT "%1"=="" SET monkeyLogFile=%1



ECHO.[ INFO ] Monkey日志: %monkeyLogFile%

ECHO.[ INFO ] 开始分析

SET blnException=0

ECHO.

ECHO.

REM 如果觉得分析太快，没有感觉，把下面注释去掉假装分析中，有停顿感

REM ping -n 2 127.0.0.1>nul



::ANR日志

FOR /F "delims=" %%a IN ('FINDSTR /C:"ANR" %monkeyLogFile%') DO ( 

    SET strANR=%%a

)



::崩溃日志

FOR /F "delims=" %%a IN ('FINDSTR /C:"CRASH" %monkeyLogFile%') DO ( 

    SET strCRASH=%%a

)

    

::异常日志

FOR /F "delims=" %%a IN ('FINDSTR /C:"Exception" %monkeyLogFile%') DO ( 

    SET strException=%%a

)



::正常

FOR /F "delims=" %%a IN ('FINDSTR /C:"Monkey finished" %monkeyLogFile%') DO ( 

    SET strFinished=%%a

)



IF NOT "%strANR%" == "" (

    ECHO.[ INFO ] 分析Monkey日志存在: ANR

    ECHO.[ INFO ] ------------------------------------

    ECHO.         "%strANR%"

    SET /a blnException+=1

    ECHO.

)



IF NOT "%strCRASH%" == "" (

    ECHO.[ INFO ] 分析Monkey日志存在: CRASH

    ECHO.[ INFO ] ------------------------------------

    ECHO.         "%strCRASH%"

    SET /a blnException+=1

    ECHO.

)



IF NOT "%strException%" == "" (

    ECHO.[ INFO ] 分析Monkey日志存在: 异常

    ECHO.[ INFO ] ------------------------------------

    ECHO.         "%strException%"

    SET /a blnException+=1

)



IF NOT "%strFinished%" == "" (

    ECHO.[ INFO ] 分析Monkey日志存在: 执行成功标记

    ECHO.[ INFO ] ------------------------------------

    ECHO.         "%strFinished%"

    ECHO.

) ELSE (

    IF %blnException% EQU 0 ECHO.[ INFO ] 分析Monkey日志结果: Monkey执行异常中断，请重新执行Monkey脚本!

    ECHO.

)



REM 如果blnException不为0，说明存在异常，改变字体为淡紫色

IF %blnException% NEQ 0 ( 

    Color 0D

    ECHO.[ INFO ] 分析Monkey日志结果:存在异常日志，请手工再仔细检查！

    ECHO.

) ELSE (

    ECHO.[ INFO ] 分析Monkey日志结果:正常

    ECHO.

)

ECHO.

ECHO.[ EXIT ] 按任意键关闭窗口...

PAUSE>nul
```

## 监控activity启动时间

```
@ECHO OFF

ECHO.:::::::::::::::::::::::::::::::::::::::::::::::::

ECHO.::                                             ::

ECHO.::     Activity启动跳转时间监控 V1.0           ::

ECHO.::                                             ::

ECHO.:::::::::::::::::::::::::::::::::::::::::::::::::

:::::::             作者：Findyou                    ::

:::::::      版本:V1.0       时间：2014.08.25       ::

::::::::::::::::::::::::::::::::::::::::::::::::::::::

ECHO.[ INFO ] 清空日志

adb logcat -c

ECHO.[ INFO ] 监控开始(Ctrl+C结束)

ECHO.[ INFO ] 保存请拷贝输出内容

adb logcat -s ActivityManager|Findstr /C:": Displayed"



```

