---
title: bat案例
date: 2021-10-20T13:10:57.000Z
updated: '2024-07-01 20:19:59'
categories:
  - windows
tags:
  - bat
---

## 指定文件夹遍历文件
```bash
@echo off
set dir=A
for /f "delims=" %%i in ('dir /ad/b/s "%dir%"') do (echo %%i)
pause

@echo off
set dir=D:\testt
for /r %dir% %%i in (*.xlsx) do (
find "IBEAP007" %%i>nul
if %errorlevel% == 0
echo %%i
 )
pause
```

## 指定目录文件复制后上传
```plsql
@echo off
rem コピー目録創建
echo *****************      コピーバッチ執行             *******************
choice /c YN /m "コピー黙認目録変更かどうか(黙認目録--%cd%\)"
if %errorlevel%==1 set /p copyto="パス入力(絶対パス):
if %errorlevel%==2 set copyto=%cd%\tmpcopy
if not exist %copyto% md %copyto%
set copyfrom=D:
set compress=%cd%
rem 全コピー
echo *****************      全項目コピー開始             *******************
xcopy %copyfrom%\*.txt %copyto% /e /y

echo *****************         圧縮開始                  *******************
choice /c YN /m "圧縮名入力か(黙認名--upload)"
if %errorlevel%==1 set /p filename=圧縮名:
if %errorlevel%==2 set filename=upload
rem 圧縮
7z a -r %compress%\%filename%.zip %copyto%\*
echo *****************         圧縮終わり                *******************
echo ***********************************************************************
echo ***********************************************************************
echo *****************        アプロード開始             *******************
rem ftp_upload
choice /c YN /m "アップロードかどうか"
if %errorlevel%==1 goto begin
if %errorlevel%==2 goto end
:begin
set /p adr=IP アドレス入力:
set upload=put %compress%\%filename%.zip
set ed=bye
rem ftp命令創建
set FtpFile=%cd%\ftp.txt
set FtpLog=%cd%\ftplog.log
:login
set /p un=ユーザー入力:
set /p pd=パスワード入力:
echo open %adr%>%FtpFile%
echo %un%>>%FtpFile%
echo %pd%>>%FtpFile%
echo %upload%>>%FtpFile%
echo %ed%>>%cd%\ftp.txt
ftp -s:%FtpFile%>%FtpLog%
findstr /c:"200 PORT command successful" %FtpLog%>nul && goto success || goto fail
:success
echo *****************       アプロード成功              *******************
goto end
:fail
echo *****************       アプロード失敗              *******************
findstr /c:"530-User cannot log in" %FtpLog%>nul && goto error530 || goto other
:error530
echo *******    未登録（ユーザ名またはパスワードが間違っています)    *******
choice /c YN /m "*****************           再登録か                *******************"
if %errorlevel%==1 goto login
if %errorlevel%==2 goto end
:other
findstr /c:"552-Requested file action aborted" %FtpLog%>nul && goto error552 || goto end
:error552
echo*********    アプロードされたファイルの操作が中止されました、スペースが足りません    ***************************
:end
echo *****************      コピーバッチ執行終了         *******************
echo *****************    何かキーを押して終了します     *******************
del /q  %FtpFile%
rd  /s /q  %copyto%\
del /q  %compress%\%filename%.zip
pause>nul
```
