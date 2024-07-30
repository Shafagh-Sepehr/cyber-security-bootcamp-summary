# Introduction to Cyber Security

## Intro to Offensive Security

امنیت تهاجمی یا offensive security، تلاش برای نفوذ به سیستم،‌استفاده از باگ های نرم افزاری ویا آسیب های امنیتی جهت بدست آوردن دسترسی غیرمجاز است. برای جلوگیری از حمله ی یک هکر باید مثل یک هکر فکر و رفتار کرد و آسیب های امنیتی را شناسایی و برطرف کرد قبل از اینکه یک هکر آن را پیدا کند.

امنیت دفاعی نیز تلاش برای حفاظت از شبکه و سیستم های یک سازمان با بررسی و برطرف کردن تهدید های احتمالی است.\
\
\
\
برخی سایت ها صفحات مخفی و غیرعمومی برای دسترسی ادمین ها یا کارکنان آن سازمان دارند، با برنامه gobuster میتوان برای پیدا کردن و استفاده از این صفحات تلاش کرد:

`gobuster -u https://website_url.com -w words_to_search.txt dir`

در دستور بالا بعد از `-u` آدرس وبسایت و بعد از `-w` فایل حاوی مسیر های احتمالی است که توسط gobuster جستجو شده و در صورت وجود چنین صفحه ای در خروجی نمایش داده می شود.

\
\
\
\
در امنیت شغل های زیادی از قبیل peneteration tester (که برای نفوذ به سیستم تلاش میکند) و Red Teamer که (مانند یک مجرم سایبری تلاش به نفوذ میکند و به سازمان بازخورد ارائه میدهد) وجود دارد.

***

## Intro to Defensive Security

#### امنیت تدافعی:

امنیت تدافعی به دو بخش اصلی تقسیم میشود: ۱- جلوگیری از نفوذ با اقدامات پیشگیرانه ۲- پایان دادن و مقابله با نفوذ در حال جریان

امنیت تدافعی شامل چندین عمل است از جمله:

* افزایش اطلاعات و آگاهی کاربران در مورد امنیت سایبری
* مستند سازی و جمع آوری اطلاعات در مورد دستگاه ها و سیستم هایی که باید از آنها حفاظت کنیم
* آپدیت نرم افزار ها و اعمال patch های امنیتی برای رفع آسیب پذیری های شناخته شده
* نصب فایروال و IPS (سیستم جلوگیری از نفوذ)، که فایروال ترافیک اینترنت ورودی و خروجی را کنترل میکند و IPS طبق قوانین و پترن های حمله شناخته شده جلوی حمله را میگیرد.
* راه اندازی سیستم لاگ اندازی و بررسی سیستم

#### کار های تیم SOC:

* رفع آسیب پذیری های شناخته شده
* اعمال سیاست ها و قوانین سازمان، برای مثال یک کاربر نباید اطلاعات محرمانه سازمان را در یک cloud storage ذخیره کند.
* جلوگیری از ورود های بی اجازه،‌ برای مثال اگر یوزرنیم و رمز عبور یک کاربر دزدیده شود و با استفاده از آنها ورود به سیستم انجام شود، باید جلوی آن گرفته بشود.
* جلوگیری از نفوذ به شبکه، جلوگیری از نفوذ یک هکر که ممکن است از طریق یک سرور عمومی انجام شود.

#### آگاهی تهدید:

یک سازمان باید درمورد تهدید های ممکن تحقیق کند و روش، دلیل و هدف حمله هکر را بفهمد، برای مثال یک شرکت نگهداری داده ممکن است توسط کسانی که داده ها را میخواهند بدزدند مورد حمله قرار بگیرد اما یک کارخانه توسط حمله های خرابکارانه قرار میگیرد.

#### تیم DFIR:

به جرم یابی دیجیتال، واکنش به حادثه و بررسی بدافزار ها می پردازد.

**جرم یابی دیجیتال:**

* با استفاده از داده های ذخیره شده مربوط به جرم یابی دیجیتال میتوان مموری و حافظه یک سیستم را از لحاظ برنامه های نصب شده،‌برنامه های بازنویسی شده، برنامه های غیر معمول در حال اجرا بررسی کرد.
* لاگ های سیستم و شبکه نیز به جرم یابی دیجیتال کمک میکنند زیرا که نفوذگر هرچقدر برای پاک کردن ردپای خود تلاش کند در لاگ های سیستم باز اطلاعاتی یافت می شود و همچنین بررسی لاگ های پکت های دریافتی و ارسالی به شناسایی کمک میکنند.

**واکنش به حادثه:**

این مورد برای نفوذ های مخرب است مانند ویروسی شدن سیستم یا از کار افتادن بخشی از آن

**مراحل واکنش به حادثه:**

* آماده بودن تیم واکنش به حادثه
* شناسایی و بررسی حمله یا ویروس
* جلوگیری از گسترش حمله و سرکوب کردن آن،‌از بین بردن تهدید و بازیابی سیستم
* جلوگیری از حمله های شبیه به آن در آینده

**انواع بدافزار(malware):**

* ویروس ها: هدفشان گسترش و انتقال بین سیستم ها است و اثر مخربش تخریب فایل ها، اشغال حافظه و... می باشد.
* تروجان ها: در ظاهر برنامه سالم و دور از هدف اصلی هستند ولی بعد از نصب دسترسی بی اجازه به سیستم خواهند داشت. مانند video playerـی که از سایت غیر معتبر دانلود می شود و هنگام نصب کنترل کل سیستم را میگیرد.
* ـ باج افزار: فایل های سیستم را رمزنگاری کرده و هکر ها با دریافت پول رمز دسترسی به فایل ها را به قربانی میدهند.

**بررسی بدافزار ها:**

* بررسی استاتیک: بدون اجرای آن بررسی میشود که به دانش حرفه ای زبان اسمبلی نیاز دارد.
* بررسی داینامیک: با اجرا در محیط کنترل شده رفتار بد افزار بررسی می شود.

***

## Careers in Cyber

careers in cyber are high paying, exciting and highly demanded

here are some career roles:

### 1- Security Analyst:

mentions security of data across the organization.

1. working with others to analyze the cyber security in the company
2. gathers reports about network safety of the company and documents security issues and taken measures
3. develops security plans, research about new defensive and offensive security tools and&#x20;

### 2- Security Engineer:

creating and mentioning security controls, network and systems to prevent attacks

1. tests and shows software security&#x20;
2. monitors network and reports in order to update systems and lower vulnerabilities
3. implements needed system and software for security

### 3- Incident Responder:

acts to end and mitigate attacks as they are still ongoing

1. designing and practicing a complete response plan
2. mentioning security best practices
3. post-incident reporting and getting ready for future attacks, while learning from incidents

### 4- Digital Forensics Examiner

uses digital forensics to investigate incidents and cyber crimes

1. collects evidence
2. analyzes evidence
3. reports his findings

### 5- Malware Analyst

analyzes malware to find how they work and what they do

1. statically analyzes malicious programs and with the help of reverse engineering
2. dynamically analyzes malwares in a controlled environment
3. documents and reports findings

### 6- Penetration Tester

test tech products for security vulnerabilities

1. tests computer systems, networks and web-based apps
2. Perform security assessments and analyze policies
3. reports on insights and recommending actions for attack prevention

### 7- Red Teamer

plays the role of the cyber attacker and tries attacking the organization and gives feedback from the enemy perspective

1. tries to uncover exploitable vulnerabilities, maintain access and avoid detection
2. assesses organisations' security controls, threat intelligence, and incident response procedures
3. gathers actionable data for companies to avoid real-world instances

