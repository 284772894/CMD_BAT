
## windows bat�Զ���װapk����
* ʹ�÷���ֱ����app��bat�ļ�����

```
@ECHO OFF
ECHO [��װAPK,��Ҫ���ú�Android sdk����]
ECHO -------------------------------
ECHO [�ȴ������ֻ�...]
adb wait-for-device
ECHO [��װ] %~nx1
adb install -r %1
ECHO [��ͣ3���Զ��ر�...]
ping -n 3 127.0.0.1>nul
```

## �鿴apk�Ļ�����Ϣ

```
@ECHO OFF
ECHO [�鿴APK����Ϣ]
ECHO -------------------------------
ECHO aapt dump badging %~nx1
aapt dump badging %1 > %~dp0%~n1.txt
ECHO [��ͣ3���Զ��ر�...]
ping -n 3 127.0.0.1>nul
@ECHO ON 

```

## ����monkey��log

```
@ECHO OFF

ECHO.:::::::::::::::::::::::::::::::::::::::::::::::::

ECHO.::             ����Monkey��־                  ::

ECHO.::             ���ߣ�Findyou                   ::

ECHO.::             �汾  V1.0.1                    ::

ECHO.::             ʱ�䣺2014.08.26                ::

ECHO.:::::::::::::::::::::::::::::::::::::::::::::::::

REM ����һ���ֶ�����Monkey��־·��

SET monkeyLogFile=F:\Monkey\20140808\FindyouV1.0.0\20140825181801_monkey.log



REM ��������ֱ�ӽ�Monkey��־�ϵ���bat�ļ���

IF NOT "%1"=="" SET monkeyLogFile=%1



ECHO.[ INFO ] Monkey��־: %monkeyLogFile%

ECHO.[ INFO ] ��ʼ����

SET blnException=0

ECHO.

ECHO.

REM ������÷���̫�죬û�ио���������ע��ȥ����װ�����У���ͣ�ٸ�

REM ping -n 2 127.0.0.1>nul



::ANR��־

FOR /F "delims=" %%a IN ('FINDSTR /C:"ANR" %monkeyLogFile%') DO ( 

    SET strANR=%%a

)



::������־

FOR /F "delims=" %%a IN ('FINDSTR /C:"CRASH" %monkeyLogFile%') DO ( 

    SET strCRASH=%%a

)

    

::�쳣��־

FOR /F "delims=" %%a IN ('FINDSTR /C:"Exception" %monkeyLogFile%') DO ( 

    SET strException=%%a

)



::����

FOR /F "delims=" %%a IN ('FINDSTR /C:"Monkey finished" %monkeyLogFile%') DO ( 

    SET strFinished=%%a

)



IF NOT "%strANR%" == "" (

    ECHO.[ INFO ] ����Monkey��־����: ANR

    ECHO.[ INFO ] ------------------------------------

    ECHO.         "%strANR%"

    SET /a blnException+=1

    ECHO.

)



IF NOT "%strCRASH%" == "" (

    ECHO.[ INFO ] ����Monkey��־����: CRASH

    ECHO.[ INFO ] ------------------------------------

    ECHO.         "%strCRASH%"

    SET /a blnException+=1

    ECHO.

)



IF NOT "%strException%" == "" (

    ECHO.[ INFO ] ����Monkey��־����: �쳣

    ECHO.[ INFO ] ------------------------------------

    ECHO.         "%strException%"

    SET /a blnException+=1

)



IF NOT "%strFinished%" == "" (

    ECHO.[ INFO ] ����Monkey��־����: ִ�гɹ����

    ECHO.[ INFO ] ------------------------------------

    ECHO.         "%strFinished%"

    ECHO.

) ELSE (

    IF %blnException% EQU 0 ECHO.[ INFO ] ����Monkey��־���: Monkeyִ���쳣�жϣ�������ִ��Monkey�ű�!

    ECHO.

)



REM ���blnException��Ϊ0��˵�������쳣���ı�����Ϊ����ɫ

IF %blnException% NEQ 0 ( 

    Color 0D

    ECHO.[ INFO ] ����Monkey��־���:�����쳣��־�����ֹ�����ϸ��飡

    ECHO.

) ELSE (

    ECHO.[ INFO ] ����Monkey��־���:����

    ECHO.

)

ECHO.

ECHO.[ EXIT ] ��������رմ���...

PAUSE>nul
```

## ���activity����ʱ��

```
@ECHO OFF

ECHO.:::::::::::::::::::::::::::::::::::::::::::::::::

ECHO.::                                             ::

ECHO.::     Activity������תʱ���� V1.0           ::

ECHO.::                                             ::

ECHO.:::::::::::::::::::::::::::::::::::::::::::::::::

:::::::             ���ߣ�Findyou                    ::

:::::::      �汾:V1.0       ʱ�䣺2014.08.25       ::

::::::::::::::::::::::::::::::::::::::::::::::::::::::

ECHO.[ INFO ] �����־

adb logcat -c

ECHO.[ INFO ] ��ؿ�ʼ(Ctrl+C����)

ECHO.[ INFO ] �����뿽���������

adb logcat -s ActivityManager|Findstr /C:": Displayed"



```

