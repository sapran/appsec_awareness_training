# Mobile Application Security

## Setup the lab

Create x86 AVD with API 23.

Run the emulator
``emulator -avd x86_AppSec_API22 -http-proxy 127.0.0.1:8080 -writable-system``

Dounload and uncompress SuperSU
- https://forum.xda-developers.com/apps/supersu/stable-2016-09-01supersu-v2-78-release-t3452703

Root the AVD

```
$ cat emulator_root_x86.sh
#!/bin/bash

adb root
adb remount
adb -e push SuperSU-v2.82-201705271822/x86/su.pie /system/xbin/su
adb shell chmod 06755 /system/xbin/su
adb shell su --install
adb shell su --daemon&
adb shell setenforce 0

# Optionally: install SuperSU app.
#adb install apks/SuperSU_2.82_apk-dl.com.apk
```

Push Burp PEM certificate.

```
openssl x509 -inform der -in cacert.der -out cacert.cer
adb push cacert.cer /sdcard/
```

Install Xposed Framework
- Before Andorid 5.0 http://repo.xposed.info/module/de.robv.android.xposed.installer
- After Android 5.0 https://forum.xda-developers.com/showthread.php?t=3034811

``adb install XposedInstaller_3.1.5.apk``

Forward ports

``adb forward tcp:8008 tcp:8008``

Access Inspeckage web interface 
- http://localhost:8008

## Test vulnerable apps

Install InsecureBankv2
- InsecureBankv2 Readme https://github.com/dineshshetty/Android-InsecureBankv2

Install tools
- dex2jar https://github.com/pxb1988/dex2jar
- apktool https://ibotpeaches.github.io/Apktool/
- jd-gui https://github.com/java-decompiler/jd-gui
- Luyten https://github.com/deathmarine/Luyten
- jadx https://github.com/skylot/jadx