# 🛡️ Ethical Hacking Arabic


```
  _____  _   _     _            _   _    _            _    _             
 | ____|| | | |   (_)  ___  __ | | | |  | |  __ _  __| |  (_) _ __   __ _ 
 |  _|  | |_| |   | | / __|/ _|| |_| |  | | / _` |/ _` |  | || '_ \ / _` |
 | |___ |  _  | _ | || (__|  _||  _  |  | || (_| || (_| |  | || | | || (_| |   
 |_____||_| |_|(_)|_| \___|\__||_| |_|  |_| \__,_|\__,_|  |_||_| |_| \__, |
                                                                     |___/ 
    ___                     _      _        
   / _ \  ___   __ _ _   _| |_ __(_) _____ 
  / /_\/ / _ \ / _` | | | | '__| |/ / _ \ \/ /
 / /_\\ | (_) | (_| | |_| | |  |   <  __/>  < 
 \____/ \___/ \__, |\__,_|_|  |_|\_\___/_/\_\
                 |_|                         
```




> مستودع شامل لشروحات ودروس الـEthical Hacking باللغة العربية، صُمّم خصيصًا لمشاركة شغفي وتجاربي في عالم الأمن السيبراني 🚀

---ethical-hacking-course/
│
├── 01_lab-setup/
│   ├── install-kali.md
│   ├── kali-overview.md
│   └── linux-commands.md
│
├── 02_network-basics/
│   ├── what-is-network.md
│   └── network-part2.md
│
├── 03_wifi-hacking/
│   ├── adapters.md
│   ├── wifi-cracking.md
│   ├── crunch-usage.md
│   └── fern-wifi-crack.md
│
├── 04_network-recon/
│   ├── netdiscover.md
│   ├── nmap.md
│   ├── zenmap.md
│   └── install-win10.md
│
├── 05_mitm-attacks/
│   ├── bettercap-basics.md
│   ├── dns-spoofing.md
│   ├── https-downgrade.md
│   └── wireshark-analysis.md
│
├── 06_server-attacks/
│   ├── metasploitable-install.md
│   ├── nexpose-usage.md
│   └── metasploit-exploits.md
│
├── 07_client-attacks/
│   ├── msfvenom.md
│   ├── veil-evasion.md
│   ├── thefatrat.md
│   └── payload-icon.md
│
├── 08_social-engineering/
│   ├── maltego.md
│   ├── email-spoofing.md
│   └── sherlock.md
│
├── 09_post-exploitation/
│   ├── meterpreter-basics.md
│   ├── pivoting.md
│   └── persistence.md
│
├── 10_android-hacking/
│   ├── apk-payload.md
│   ├── signing-apk.md
│   └── exploitation.md
│
├── 11_website-hacking/
│   ├── info-gathering.md
│   ├── file-upload.md
│   └── remote-code-execution.md
│
├── 12_sql-injection/
│   ├── sql-injection-basics.md
│   ├── extract-users.md
│   ├── sqlmap.md
│   └── sql-injection-defense.md
│
├── 13_xss/
│   ├── xss-basics.md
│   ├── stored-vs-reflected.md
│   └── xss-beef.md
│
└── README.md


## 🔥 عن هذا المشروع

- هذا الريبو هو **مذكراتي** الشخصية وتعليمي المصوَّر لكل ما أتعلمه في مجال اختبار الاختراق.
- هدفه:  
  1. جمع **المفاهيم الأساسية** والمتقدمة بالعربية.  
  2. مشاركة **الأدوات**, **السكريبتات**, و**التقارير** التي أعدّها بنفسي.  
  3. بناء مجتمع صغير يتعلّم مع بعض ويطوّر مهاراته خطوة بخطوة.

---

## 📚 المحتويات

1. [docs/](#docs) — تقارير ومقالات مفصّلة  
2. [lab/](#lab) — بيئات الاختبار (Metasploitable, DVWA…)  
3. [scripts/](#scripts) — سكريبتات وأدوات مساعدة  
4. [challenges/](#challenges) — تحدّيات CTF وحلولها  
5. [resources/](#resources) —##




---

## 💡 كيف تبدأ

1. **استنساخ الريبو**  
   ```bash
   git clone https://github.com/YourUsername/ethical-hacking-arabic.git
   cd ethical-hacking-arabic

   ```
                     ,----------------,              ,---------,
                ,-----------------------,          ,"        ,"|
              ,"                      ,"|        ,"        ,"  |
             +-----------------------+  |      ,"        ,"    |
             |  .-----------------.  |  |     +---------+      |
             |  |                 |  |  |     | -==----'|      |
             |  |    ETHICAL      |  |  |     |         |      |
             |  |    HACKING      |  |  |/----|`---=    |      |
             |  |    العربي       |  |  |   ,/|==== ooo |      ;
             |  |                 |  |  |  // |(((( [33]|    ,"
             |  `-----------------'  |," .;'| |((((     |  ,"
             +-----------------------+  ;;  | |         |,"
                /_)______________(_/  //'   | +---------+
           ___________________________/___  `,
          /  oooooooooooooooo  .o.  oooo /,   \,"-----------
         / ==ooooooooooooooo==.o.  ooo= //   ,`\--{)B     ,"
        /_==__==========__==_ooo__ooo=_/'   /___________,"
        `-----------------------------'
        
         _______ _______ __   _ __   _ _______ ______  
         |______ |______ | \  | | \  | |______ |     \ 
         |______ |       |  \_| |  \_| |______ |_____/
                           
                     _    _ _     _             
                    | |  | | |   (_)            
                    | |__| | |__  _  ___ _   _ 
                    |  __  | '_ \| |/ __| | | |
                    | |  | | |_) | | (__| |_| |
                    |_|  |_|_.__/| |\___|\__, |
                               _/ |       __/ |
                              |__/       |___/ 
```

