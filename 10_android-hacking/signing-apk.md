
<!-- signing-apk.md -->

             ____  _                   _             
            / ___|| |__   __ _ _ __ __| | ___  _ __  
            \___ \| '_ \ / _` | '__/ _` |/ _ \| '_ \ 
             ___) | | | | (_| | | | (_| | (_) | | | |
            |____/|_| |_|\__,_|_|  \__,_|\___/|_| |_|
                                                     

# توقيع APK (Signing APK)

## 1. لماذا التوقيع ضروري؟
- يضمن سلامة الـ APK  
- يسمح بتثبيته على أجهزة Android  
- يثبت هوية المطور

## 2. إنشاء KeyStore
```bash
keytool -genkey -v \
  -keystore my-key.keystore \
  -alias mon_alias \
  -keyalg RSA \
  -keysize 2048 \
  -validity 10000

توقيع الـ APK
jarsigner -verbose \
  -sigalg SHA1withRSA \
  -digestalg SHA1 \
  -keystore my-key.keystore \
  evil.apk mon_alias

4. التحقق
jarsigner -verify -verbose -certs evil.apk
