如果你已经安装了 Windows，也无需重新安装，[看这里](#WHAT-IF)

如果你已经制作了 Windows 安装盘，也无需重新制作，[看这里](#WHAT-IF)

## 纯净安装盘制作步骤

注意要用 CMD 而不是 PowerShell

首先用`Media Creation Tool`下载[官方安装镜像](https://www.microsoft.com/zh-cn/software-download/windows11)

右键挂载官方镜像 ISO 文件

### 准备工作

设定临时目录（按需修改）

`set "MNT=D:\ISOTemp"`

`set "SCR=D:\scratch"`

`md %MNT%`

`md %SCR%`

复制所有文件到临时目录

`xcopy.exe /E /I /H /R /Y /J %挂载的镜像分区:% c:\tiny11 >nul`

### 挂载 WIM 用于清理

查看 WIM 信息

`dism /Get-WimInfo /wimfile:%MNT%\sources\install.wim`

`dism /mount-image /imagefile:%MNT%\sources\install.wim /index:%安装的版本的序号% /mountdir:%SCR%`

如果不存在，则可能是 ESD，需要转换

查看 ESD 信息

`dism /Get-WimInfo /wimfile:%MNT%\sources\install.esd`

转换为 WIM

`dism /Export-Image /SourceImageFile:%MNT%\sources\install.esd /SourceIndex:%安装的版本的序号% /DestinationImageFile:%MNT%\sources\install-src.wim /Compress:Max /CheckIntegrity`

检查转换后的 WIM

`dism /Get-WimInfo /wimfile:%MNT%\sources\install-src.wim`

`dism /mount-image /imagefile:%MNT%\sources\install-src.wim /index:1 /mountdir:%SCR%`

### 清洗 WIM 安装镜像

获取预安装的软件包列表

`dism /image:%SCR% /Get-Packages`

按需删除，使用 `dism /image:%SCR% /Remove-ProvisionedAppxPackage /PackageName:[包名称]`

以下常见的包名，来自 ntdevlabs/tiny11builder.git

```batch
echo Removing Clipchamp...
dism /image:%SCR% /Remove-ProvisionedAppxPackage /PackageName:Clipchamp.Clipchamp_2.2.8.0_neutral_~_yxz26nhyzhsrt 
echo Removing News...
dism /image:%SCR% /Remove-ProvisionedAppxPackage /PackageName:Microsoft.BingNews_4.2.27001.0_neutral_~_8wekyb3d8bbwe
echo Removing Weather...
dism /image:%SCR% /Remove-ProvisionedAppxPackage /PackageName:Microsoft.BingWeather_4.53.33420.0_neutral_~_8wekyb3d8bbwe
echo Removing Xbox...
dism /image:%SCR% /Remove-ProvisionedAppxPackage /PackageName:Microsoft.GamingApp_2021.427.138.0_neutral_~_8wekyb3d8bbwe
echo Removing GetHelp...
dism /image:%SCR% /Remove-ProvisionedAppxPackage /PackageName:Microsoft.GetHelp_10.2201.421.0_neutral_~_8wekyb3d8bbwe
echo Removing GetStarted...
dism /image:%SCR% /Remove-ProvisionedAppxPackage /PackageName:Microsoft.Getstarted_2021.2204.1.0_neutral_~_8wekyb3d8bbwe
echo Removing Office Hub...
dism /image:%SCR% /Remove-ProvisionedAppxPackage /PackageName:Microsoft.MicrosoftOfficeHub_18.2204.1141.0_neutral_~_8wekyb3d8bbwe
echo Removing Solitaire...
dism /image:%SCR% /Remove-ProvisionedAppxPackage /PackageName:Microsoft.MicrosoftSolitaireCollection_4.12.3171.0_neutral_~_8wekyb3d8bbwe
echo Removing PeopleApp...
dism /image:%SCR% /Remove-ProvisionedAppxPackage /PackageName:Microsoft.People_2020.901.1724.0_neutral_~_8wekyb3d8bbwe
echo Removing PowerAutomate...
dism /image:%SCR% /Remove-ProvisionedAppxPackage /PackageName:Microsoft.PowerAutomateDesktop_10.0.3735.0_neutral_~_8wekyb3d8bbwe
echo Removing ToDo...
dism /image:%SCR% /Remove-ProvisionedAppxPackage /PackageName:Microsoft.Todos_2.54.42772.0_neutral_~_8wekyb3d8bbwe
echo Removing Alarms...
dism /image:%SCR% /Remove-ProvisionedAppxPackage /PackageName:Microsoft.WindowsAlarms_2022.2202.24.0_neutral_~_8wekyb3d8bbwe
echo Removing Mail...
dism /image:%SCR% /Remove-ProvisionedAppxPackage /PackageName:microsoft.windowscommunicationsapps_16005.14326.20544.0_neutral_~_8wekyb3d8bbwe
echo Removing Feedback Hub...
dism /image:%SCR% /Remove-ProvisionedAppxPackage /PackageName:Microsoft.WindowsFeedbackHub_2022.106.2230.0_neutral_~_8wekyb3d8bbwe
echo Removing Maps...
dism /image:%SCR% /Remove-ProvisionedAppxPackage /PackageName:Microsoft.WindowsMaps_2022.2202.6.0_neutral_~_8wekyb3d8bbwe
echo Removing Sound Recorder...
dism /image:%SCR% /Remove-ProvisionedAppxPackage /PackageName:Microsoft.WindowsSoundRecorder_2021.2103.28.0_neutral_~_8wekyb3d8bbwe
echo Removing XboxTCUI...
dism /image:%SCR% /Remove-ProvisionedAppxPackage /PackageName:Microsoft.Xbox.TCUI_1.23.28004.0_neutral_~_8wekyb3d8bbwe
echo Removing XboxGamingOverlay...
dism /image:%SCR% /Remove-ProvisionedAppxPackage /PackageName:Microsoft.XboxGamingOverlay_2.622.3232.0_neutral_~_8wekyb3d8bbwe
echo Removing XboxGameOverlay...
dism /image:%SCR% /Remove-ProvisionedAppxPackage /PackageName:Microsoft.XboxGameOverlay_1.47.2385.0_neutral_~_8wekyb3d8bbwe
echo Removing XboxSpeechToTextOverlay...
dism /image:%SCR% /Remove-ProvisionedAppxPackage /PackageName:Microsoft.XboxSpeechToTextOverlay_1.17.29001.0_neutral_~_8wekyb3d8bbwe
echo Removing Your Phone...
dism /image:%SCR% /Remove-ProvisionedAppxPackage /PackageName:Microsoft.YourPhone_1.22022.147.0_neutral_~_8wekyb3d8bbwe
echo Removing Music...
dism /image:%SCR% /Remove-ProvisionedAppxPackage /PackageName:Microsoft.ZuneMusic_11.2202.46.0_neutral_~_8wekyb3d8bbwe
echo Removing Video...
dism /image:%SCR% /Remove-ProvisionedAppxPackage /PackageName:Microsoft.ZuneVideo_2019.22020.10021.0_neutral_~_8wekyb3d8bbwe
echo Removing Family...
dism /image:%SCR% /Remove-ProvisionedAppxPackage /PackageName:MicrosoftCorporationII.MicrosoftFamily_2022.507.447.0_neutral_~_8wekyb3d8bbwe
echo Removing QuickAssist...
dism /image:%SCR% /Remove-ProvisionedAppxPackage /PackageName:MicrosoftCorporationII.QuickAssist_2022.414.1758.0_neutral_~_8wekyb3d8bbwe
echo Removing Teams...
dism /image:%SCR% /Remove-ProvisionedAppxPackage /PackageName:MicrosoftTeams_23002.403.1788.1930_x64__8wekyb3d8bbwe
echo Removing Cortana...
dism /image:%SCR% /Remove-ProvisionedAppxPackage /PackageName:Microsoft.549981C3F5F10_4.2204.13303.0_neutral_~_8wekyb3d8bbwe
echo Now proceeding to removal of system packages...
timeout /t 1 /nobreak > nul
echo Removing Internet Explorer...
dism /image:%SCR% /Remove-Package /PackageName:Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~en-US~11.0.22621.1 > nul
dism /image:%SCR% /Remove-Package /PackageName:Microsoft-Windows-InternetExplorer-Optional-Package~31bf3856ad364e35~amd64~~11.0.22621.525 > nul
echo Removing LA57:
dism /image:%SCR% /Remove-Package /PackageName:Microsoft-Windows-Kernel-LA57-FoD-Package~31bf3856ad364e35~amd64~~10.0.22621.525 > nul
echo Removing Handwriting:
dism /image:%SCR% /Remove-Package /PackageName:Microsoft-Windows-LanguageFeatures-Handwriting-en-us-Package~31bf3856ad364e35~amd64~~10.0.22621.525 > nul
echo Removing OCR:
dism /image:%SCR% /Remove-Package /PackageName:Microsoft-Windows-LanguageFeatures-OCR-en-us-Package~31bf3856ad364e35~amd64~~10.0.22621.525 > nul
echo Removing Speech:
dism /image:%SCR% /Remove-Package /PackageName:Microsoft-Windows-LanguageFeatures-Speech-en-us-Package~31bf3856ad364e35~amd64~~10.0.22621.525 > nul
echo Removing TTS:
dism /image:%SCR% /Remove-Package /PackageName:Microsoft-Windows-LanguageFeatures-TextToSpeech-en-us-Package~31bf3856ad364e35~amd64~~10.0.22621.525 > nul
echo Removing Media Player Legacy:
dism /image:%SCR% /Remove-Package /PackageName:Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.22621.525 > nul
dism /image:%SCR% /Remove-Package /PackageName:Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~wow64~en-US~10.0.22621.1 > nul
dism /image:%SCR% /Remove-Package /PackageName:Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.22621.525 > nul
dism /image:%SCR% /Remove-Package /PackageName:Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~wow64~~10.0.22621.1 > nul
echo Removing Tablet PC Math:
dism /image:%SCR% /Remove-Package /PackageName:Microsoft-Windows-TabletPCMath-Package~31bf3856ad364e35~amd64~~10.0.22621.525 > nul
echo Removing Wallpapers:
dism /image:%SCR% /Remove-Package /PackageName:Microsoft-Windows-Wallpaper-Content-Extended-FoD-Package~31bf3856ad364e35~amd64~~10.0.22621.525 > nul
```

清洗注册表

```batch
reg load HKLM\zCOMPONENTS "%SCR%\Windows\System32\config\COMPONENTS" >nul
reg load HKLM\zDEFAULT "%SCR%\Windows\System32\config\default" >nul
reg load HKLM\zNTUSER "%SCR%\Users\Default\ntuser.dat" >nul
reg load HKLM\zSOFTWARE "%SCR%\Windows\System32\config\SOFTWARE" >nul
reg load HKLM\zSYSTEM "%SCR%\Windows\System32\config\SYSTEM" >nul
echo Bypassing system requirements(on the system image):
Reg add "HKLM\zDEFAULT\Control Panel\UnsupportedHardwareNotificationCache" /v "SV1" /t REG_DWORD /d "0" /f >nul 2>&1
Reg add "HKLM\zDEFAULT\Control Panel\UnsupportedHardwareNotificationCache" /v "SV2" /t REG_DWORD /d "0" /f >nul 2>&1
Reg add "HKLM\zNTUSER\Control Panel\UnsupportedHardwareNotificationCache" /v "SV1" /t REG_DWORD /d "0" /f >nul 2>&1
Reg add "HKLM\zNTUSER\Control Panel\UnsupportedHardwareNotificationCache" /v "SV2" /t REG_DWORD /d "0" /f >nul 2>&1
Reg add "HKLM\zSYSTEM\Setup\LabConfig" /v "BypassCPUCheck" /t REG_DWORD /d "1" /f >nul 2>&1
Reg add "HKLM\zSYSTEM\Setup\LabConfig" /v "BypassRAMCheck" /t REG_DWORD /d "1" /f >nul 2>&1
Reg add "HKLM\zSYSTEM\Setup\LabConfig" /v "BypassSecureBootCheck" /t REG_DWORD /d "1" /f >nul 2>&1
Reg add "HKLM\zSYSTEM\Setup\LabConfig" /v "BypassStorageCheck" /t REG_DWORD /d "1" /f >nul 2>&1
Reg add "HKLM\zSYSTEM\Setup\LabConfig" /v "BypassTPMCheck" /t REG_DWORD /d "1" /f >nul 2>&1
Reg add "HKLM\zSYSTEM\Setup\MoSetup" /v "AllowUpgradesWithUnsupportedTPMOrCPU" /t REG_DWORD /d "1" /f >nul 2>&1
echo Disabling Teams:
Reg add "HKLM\zSOFTWARE\Microsoft\Windows\CurrentVersion\Communications" /v "ConfigureChatAutoInstall" /t REG_DWORD /d "0" /f >nul 2>&1
echo Disabling Sponsored Apps:
Reg add "HKLM\zNTUSER\SOFTWARE\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v "OemPreInstalledAppsEnabled" /t REG_DWORD /d "0" /f >nul 2>&1
Reg add "HKLM\zNTUSER\SOFTWARE\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v "PreInstalledAppsEnabled" /t REG_DWORD /d "0" /f >nul 2>&1
Reg add "HKLM\zNTUSER\SOFTWARE\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v "SilentInstalledAppsEnabled" /t REG_DWORD /d "0" /f >nul 2>&1
Reg add "HKLM\zSOFTWARE\Policies\Microsoft\Windows\CloudContent" /v "DisableWindowsConsumerFeatures" /t REG_DWORD /d "1" /f >nul 2>&1
Reg add "HKLM\zSOFTWARE\Microsoft\PolicyManager\current\device\Start" /v "ConfigureStartPins" /t REG_SZ /d "{\"pinnedList\": [{}]}" /f >nul 2>&1
echo Enabling Local Accounts on OOBE:
Reg add "HKLM\zSOFTWARE\Microsoft\Windows\CurrentVersion\OOBE" /v "BypassNRO" /t REG_DWORD /d "1" /f >nul 2>&1
echo Disabling Reserved Storage:
Reg add "HKLM\zSOFTWARE\Microsoft\Windows\CurrentVersion\ReserveManager" /v "ShippedWithReserves" /t REG_DWORD /d "0" /f >nul 2>&1
echo Disabling Chat icon:
Reg add "HKLM\zSOFTWARE\Policies\Microsoft\Windows\Windows Chat" /v "ChatIcon" /t REG_DWORD /d "3" /f >nul 2>&1
Reg add "HKLM\zNTUSER\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v "TaskbarMn" /t REG_DWORD /d "0" /f >nul 2>&1
echo Unmounting Registry...
reg unload HKLM\zCOMPONENTS >nul 2>&1
reg unload HKLM\zDRIVERS >nul 2>&1
reg unload HKLM\zDEFAULT >nul 2>&1
reg unload HKLM\zNTUSER >nul 2>&1
reg unload HKLM\zSCHEMA >nul 2>&1
reg unload HKLM\zSOFTWARE >nul 2>&1
reg unload HKLM\zSYSTEM >nul 2>&1
```

做 Cleanup 操作

`dism /image:%SCR% /Cleanup-Image /StartComponentCleanup /ResetBase`

卸载并保存镜像

`dism /unmount-image /mountdir:%SCR% /commit`

如果是 ESD 转换来的 WIM，就转换回 ESD

`dism /Export-Image /SourceImageFile:%MNT%\sources\install-src.wim /SourceIndex:1 /DestinationImageFile:%MNT%\sources\install.esd /compress:recovery /CheckIntegrity`

否则就使用 WIM，可以重新压缩

`ren %MNT%\sources\install.wim install-src.wim`

`dism /Export-Image /SourceImageFile:%MNT%\sources\install-src.wim /SourceIndex:%安装的版本的序号% /DestinationImageFile:%MNT%\sources\install.wim /compress:max`

删除临时文件

`del %MNT%\Sources\install-src.wim`

### 清洗 Boot 镜像和安装配置

`dism /mount-image /imagefile:%MNT%\sources\boot.wim /index:2 /mountdir:%SCR%`

清洗注册表

```batch
reg load HKLM\zCOMPONENTS "%SCR%\Windows\System32\config\COMPONENTS" >nul
reg load HKLM\zDEFAULT "%SCR%\Windows\System32\config\default" >nul
reg load HKLM\zNTUSER "%SCR%\Users\Default\ntuser.dat" >nul
reg load HKLM\zSOFTWARE "%SCR%\Windows\System32\config\SOFTWARE" >nul
reg load HKLM\zSYSTEM "%SCR%\Windows\System32\config\SYSTEM" >nul
echo Bypassing system requirements(on the setup image):
Reg add "HKLM\zDEFAULT\Control Panel\UnsupportedHardwareNotificationCache" /v "SV1" /t REG_DWORD /d "0" /f >nul 2>&1
Reg add "HKLM\zDEFAULT\Control Panel\UnsupportedHardwareNotificationCache" /v "SV2" /t REG_DWORD /d "0" /f >nul 2>&1
Reg add "HKLM\zNTUSER\Control Panel\UnsupportedHardwareNotificationCache" /v "SV1" /t REG_DWORD /d "0" /f >nul 2>&1
Reg add "HKLM\zNTUSER\Control Panel\UnsupportedHardwareNotificationCache" /v "SV2" /t REG_DWORD /d "0" /f >nul 2>&1
Reg add "HKLM\zSYSTEM\Setup\LabConfig" /v "BypassCPUCheck" /t REG_DWORD /d "1" /f >nul 2>&1
Reg add "HKLM\zSYSTEM\Setup\LabConfig" /v "BypassRAMCheck" /t REG_DWORD /d "1" /f >nul 2>&1
Reg add "HKLM\zSYSTEM\Setup\LabConfig" /v "BypassSecureBootCheck" /t REG_DWORD /d "1" /f >nul 2>&1
Reg add "HKLM\zSYSTEM\Setup\LabConfig" /v "BypassStorageCheck" /t REG_DWORD /d "1" /f >nul 2>&1
Reg add "HKLM\zSYSTEM\Setup\LabConfig" /v "BypassTPMCheck" /t REG_DWORD /d "1" /f >nul 2>&1
Reg add "HKLM\zSYSTEM\Setup\MoSetup" /v "AllowUpgradesWithUnsupportedTPMOrCPU" /t REG_DWORD /d "1" /f >nul 2>&1
echo Unmounting Registry...
reg unload HKLM\zCOMPONENTS >nul 2>&1
reg unload HKLM\zDRIVERS >nul 2>&1
reg unload HKLM\zDEFAULT >nul 2>&1
reg unload HKLM\zNTUSER >nul 2>&1
reg unload HKLM\zSCHEMA >nul 2>&1
reg unload HKLM\zSOFTWARE >nul 2>&1
reg unload HKLM\zSYSTEM >nul 2>&1
```

卸载并保存镜像

`dism /unmount-image /mountdir:%SCR% /commit`

### 打包干净的安装镜像

导出 ISO 文件，注意最后是输出位置

`oscdimg.exe -m -o -u2 -udfver102 -bootdata:2#p0,e,b%MNT%\boot\etfsboot.com#pEF,e,b%MNT%\efi\microsoft\boot\efisys.bin %MNT% Windows11-clean.iso`

### 清理临时文件

`rd %MNT% /s /q`
`rd %SCR% /s /q`

## WHAT-IF

已经安装了官方版？

查看本机安装的 Appx 包

`dism /Online /Get-Packages`

删除不需要的软件包

`dism /Online /Remove-ProvisionedAppxPackage /PackageName:[APPX name]`

在安装官方版的时候受限制？

「不满足运行 Win11 的硬件要求」：

`Shift+F10`

`regedit`

在 HKEY_LOCAL_MACHINE\SYSTEM\Setup\LabConfig\

新增 DWORD BypassTPMCheck = 1

新增 DWORD BypassSecureBootCheck = 1

新增 DWORD BypassRAMCheck = 1

新增 DWORD BypassCPUCheck = 1

关闭，返回上一步，就可以继续安装

「不联网登录就不让你用」：

`Shift+F10`

`oobe\bypassnro`

自动重启后点击 我没有 Internet

## 只需要绕过限制

考虑使用 Rufus 制作安装盘

`choco install rufus -y` or `winget install -i Rufus.Rufus`
