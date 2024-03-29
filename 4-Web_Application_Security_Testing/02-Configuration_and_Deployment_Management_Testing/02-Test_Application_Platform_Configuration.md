# Test Application Platform Configuration (fa-IR)

آزمایش پیکربندی پلتفرم برنامه (فارسی)

|شناسه          |
|------------|
|WSTG-CONF-02|

## خلاصه

پیکربندی مناسب عناصر منفرد که معماری برنامه را تشکیل می دهند به منظور جلوگیری از اشتباهاتی که ممکن است امنیت کل معماری را به خطر بیندازند، مهم است.

بررسی و آزمایش پیکربندی یک کار حیاتی در ایجاد و حفظ یک معماری است. این به این دلیل است که بسیاری از سیستم‌های مختلف معمولاً با پیکربندی‌های عمومی ارائه می‌شوند که ممکن است برای کاری که در سایت خاصی که در آن نصب شده‌اند انجام دهند، مناسب نباشد.

در حالی که نصب سرور وب و برنامه معمولی دارای عملکردهای زیادی است (مانند نمونه های برنامه، مستندات، صفحات آزمایشی)، آنچه ضروری نیست باید قبل از استقرار حذف شود تا از سوء استفاده پس از نصب جلوگیری شود.

## اهداف آزمایش

- مطمئن شوید که فایل های پیش فرض و شناخته شده حذف شده اند.
- تأیید کنید که هیچ کد یا برنامه افزودنی اشکال زدایی در محیط های تولید باقی نمانده است.
- مکانیسم‌های گزارش‌گیری را که برای برنامه تنظیم شده است، مرور کنید.

## چگونه آزمایش کنیم

### آزمایش جعبه سیاه (Black-Box Testing)

#### فایل ها و فهرست های نمونه و شناخته شده (Sample and Known Files and Directories)

بسیاری از وب سرورها و سرورهای برنامه، در یک نصب پیش فرض، نمونه برنامه ها و فایل ها را به نفع توسعه دهنده و به منظور آزمایش درست کارکرد سرور بلافاصله پس از نصب ارائه می دهند. با این حال، بسیاری از برنامه های وب سرور پیش فرض بعداً آسیب پذیر شناخته شدند. این مورد، برای مثال، برای CVE-1999-0449 (انکار سرویس (Denial of Service) در IIS زمانی که سایت نمونه Exair نصب شده بود)، CAN-2002-1744 (آسیب‌پذیری پیمایش دایرکتوری در CodeBrws.asp در Microsoft IIS 5.0)، CAN-2002-1630 (استفاده از sendmail.jsp در Oracle 9iAS)، یا CAN-2003-1172 (پیمایش دایرکتوری در نمونه view-source در Apache's Cocoon).

اسکنرهای CGI شامل فهرستی دقیق از فایل‌های شناخته شده و نمونه‌های دایرکتوری هستند که توسط سرورهای مختلف وب یا برنامه ارائه شده‌اند و ممکن است راهی سریع برای تعیین وجود این فایل‌ها باشد. با این حال، تنها راه برای اطمینان واقعی این است که یک بررسی کامل از محتویات وب سرور یا سرور برنامه انجام دهید و مشخص کنید که آیا آنها به خود برنامه مربوط هستند یا خیر.

#### بررسی کامنت (Comment Review)

برای برنامه نویسان بسیار رایج است که هنگام توسعه برنامه های بزرگ مبتنی بر وب کامنت (نظر) های خود را اضافه کنند. با این حال، کامنت های موجود در کد HTML ممکن است اطلاعات داخلی را نشان دهد که نباید در دسترس مهاجم باشد. گاهی اوقات، حتی کد منبع نیز کامنت می‌شود، زیرا دیگر نیازی به عملکرد نیست، اما این کامنت به صفحات HTML که به طور ناخواسته به کاربران بازگردانده می‌شود، افشا می‌شود.

بررسی کامنت ها باید انجام شود تا مشخص شود که آیا اطلاعاتی از طریق کامنت ها درز کرده است یا خیر. این بررسی تنها از طریق تجزیه و تحلیل محتوای ایستا و پویا وب سرور و از طریق جستجوی فایل می تواند به طور کامل انجام شود. مرور سایت به صورت خودکار یا هدایت شده و ذخیره تمام محتوای بازیابی شده می تواند مفید باشد. سپس این محتوای بازیابی شده را می توان به منظور تجزیه و تحلیل هر کامنت HTML موجود در کد جستجو کرد.

#### پیکربندی سیستم (System Configuration)

ابزارها، اسناد، یا چک لیست‌های مختلفی را می‌توان برای ارزیابی دقیق انطباق سیستم‌های هدف با خطوط پایه پیکربندی یا معیارهای مختلف به متخصصان فناوری اطلاعات و امنیت ارائه داد. چنین ابزارهایی عبارتند از (اما نه محدود به):

- [CIS-CAT Lite](https://www.cisecurity.org/blog/introducing-cis-cat-lite/)
- [Microsoft's Attack Surface Analyzer](https://github.com/microsoft/AttackSurfaceAnalyzer) (آنالیز سطح حمله مایکروسافت)
- [NIST's National Checklist Program](https://nvd.nist.gov/ncp/repository) (برنامه ملی چک لیست)

### آزمایش جعبه خاکستری (Gray-Box Testing)

#### بررسی پیکربندی (Configuration Review)

پیکربندی وب سرور یا سرور برنامه نقش مهمی در محافظت از محتویات سایت دارد و باید به دقت بررسی شود تا اشتباهات رایج در پیکربندی مشخص شود. بدیهی است که پیکربندی توصیه شده بسته به سیاست سایت و عملکردی که باید توسط نرم افزار سرور ارائه شود متفاوت است. با این حال، در بیشتر موارد، دستورالعمل‌های پیکربندی (چه توسط فروشنده نرم‌افزار یا طرف‌های خارجی ارائه شده است) باید دنبال شود تا مشخص شود که آیا سرور به درستی ایمن شده است یا خیر.

به طور کلی نمی توان گفت که سرور چگونه باید پیکربندی شود، با این حال، برخی از دستورالعمل های رایج باید در نظر گرفته شود:

- فقط ماژول های سرور (پسوندهای ISAPI در مورد IIS) را که برای برنامه مورد نیاز هستند، فعال کنید. این سطح حمله را کاهش می دهد زیرا سرور از نظر اندازه و پیچیدگی با غیرفعال شدن ماژول های نرم افزار کاهش می یابد. همچنین از آسیب‌پذیری‌هایی که ممکن است در نرم‌افزار فروشنده ظاهر شوند، روی سایت تأثیر بگذارند، در صورتی که فقط در ماژول‌هایی وجود داشته باشند که قبلاً غیرفعال شده‌اند.
- خطاهای سرور (40x یا 50x) را با صفحات سفارشی به جای صفحات وب سرور پیش فرض مدیریت کنید. به طور خاص مطمئن شوید که خطاهای برنامه به کاربر نهایی بازگردانده نمی شود و هیچ کدی از طریق این خطاها درز نمی کند زیرا به مهاجم کمک می کند. در واقع فراموش کردن این نکته بسیار رایج است زیرا توسعه دهندگان به این اطلاعات در محیط های پیش تولید نیاز دارند.
- مطمئن شوید که نرم افزار سرور با حداقل امتیازات در سیستم عامل اجرا می شود. این از یک خطا در نرم افزار سرور جلوگیری می کند که مستقیماً کل سیستم را در معرض خطر قرار دهد، اگرچه یک مهاجم می تواند پس از اجرای کد به عنوان وب سرور، امتیازات را افزایش دهد.
- مطمئن شوید که نرم افزار سرور به درستی هم دسترسی قانونی و هم خطاها را ثبت می کند.
- اطمینان حاصل کنید که سرور به گونه ای پیکربندی شده است که بارهای اضافی را به درستی مدیریت کند و از حملات Denial of Service جلوگیری کند. اطمینان حاصل کنید که سرور به درستی تنظیم شده است.
- هرگز به شناسه های غیر مدیریتی (به استثنای `NT SERVICE\WMSvc`) دسترسی به applicationHost.config، redirection.config و Administration.config (دسترسی خواندن یا نوشتن) ندهید. این شامل `Network Service`, `IIS_IUSRS`, `IUSR` یا هر هویت سفارشی مورد استفاده توسط پول های برنامه IIS است (IIS application pools). فرآیندهای کارگر IIS برای دسترسی مستقیم به هیچ یک از این فایل ها نیست.
- هرگز applicationHost.config، redirection.config و Administration.config را در شبکه به اشتراک نگذارید. هنگام استفاده از پیکربندی اشتراکی، ترجیح دهید applicationHost.config را به مکان دیگری صادر کنید (به بخش با عنوان "تنظیم مجوزها برای پیکربندی اشتراکی" مراجعه کنید.
- به خاطر داشته باشید که همه کاربران به طور پیش فرض می توانند فایل های NET Framework `machine.config`. و روت `web.config` را بخوانند. اطلاعات حساس را در این فایل‌ها ذخیره نکنید، اگر فقط باید برای مدیر قابل دیدن باشد.
- اطلاعات حساسی را رمزگذاری کنید که فقط باید توسط فرآیندهای کارگر IIS خوانده شود و نه توسط سایر کاربران در دستگاه.
- به هویتی که سرور وب برای دسترسی به `applicationHost.config` اشتراک‌گذاری شده استفاده می‌کند، دسترسی نوشتن ندهید. این هویت باید فقط دسترسی خواندن داشته باشد.
- از یک هویت جداگانه برای انتشار applicationHost.config در اشتراک استفاده کنید. از این هویت برای پیکربندی دسترسی به پیکربندی مشترک در سرورهای وب استفاده نکنید.
- هنگام صادر کردن کلیدهای رمزگذاری برای استفاده با پیکربندی مشترک، از یک رمز عبور قوی استفاده کنید.
- دسترسی محدود به اشتراک‌گذاری حاوی کلیدهای پیکربندی و رمزگذاری مشترک را حفظ کنید. اگر این اشتراک به خطر بیفتد، مهاجم می تواند هر پیکربندی IIS را برای سرورهای وب شما بخواند و بنویسد، ترافیک وب سایت شما را به منابع مخرب هدایت کند، و در برخی موارد با بارگذاری کد دلخواه در فرآیندهای کارگر IIS، کنترل همه سرورهای وب را به دست آورد.
- محافظت از این اشتراک با قوانین فایروال و سیاست های IPsec را در نظر بگیرید تا فقط به سرورهای وب عضو اجازه اتصال داده شود.

#### لاگ (گزارش) کردن (Logging)

لاگینگ یک دارایی مهم امنیت معماری برنامه است، زیرا می توان از آن برای شناسایی نقص در برنامه ها (کاربرانی که دائماً در تلاش برای بازیابی فایلی که واقعاً وجود ندارد) و همچنین حملات مداوم از سوی کاربران سرکش استفاده شود. لاگ (گزارش) ها معمولاً توسط وب و سایر نرم افزارهای سرور به درستی تولید می شوند. یافتن برنامه‌هایی که به‌درستی اقدامات خود را در یک گزارش ثبت می‌کنند، معمول نیست و در صورت انجام، هدف اصلی گزارش های برنامه تولید خروجی اشکال‌زدایی است که می‌تواند توسط برنامه‌نویس برای تجزیه و تحلیل یک خطای خاص استفاده شود.

در هر دو مورد (گزارش های سرور و برنامه‌ها) چندین مسئله باید بر اساس محتویات گزارش آزمایش و تجزیه و تحلیل شوند:

1. آیا گزارش ها حاوی اطلاعات حساس هستند؟
2. آیا گزارش ها در یک سرور اختصاصی ذخیره می شوند؟
3. آیا استفاده از گزارش می تواند شرایط انکار سرویس را ایجاد کند؟
4. چرخش آنها چگونه است؟ آیا گزارش ها برای زمان کافی نگهداری می شوند؟
5. گزارش ها چگونه بررسی می شوند؟ آیا مدیران می توانند از این بررسی ها برای شناسایی حملات هدفمند استفاده کنند؟
6. پشتیبان‌گیری‌های گزارش چگونه حفظ می‌شوند؟
7. آیا داده‌های ثبت‌شده قبل از ثبت اعتبار (حداقل/حداکثر طول، کاراکترها و غیره) اعتبارسنجی می‌شوند؟

##### اطلاعات حساس در لاگ ها (Sensitive Information in Logs)

برای مثال، برخی از برنامه‌ها ممکن است از درخواست‌های GET (دریافت) برای ارسال داده‌های فرم که در گزارش‌های سرور دیده می‌شوند، استفاده کنند. این بدان معناست که گزارش‌های سرور ممکن است حاوی اطلاعات حساسی باشند (مانند نام‌های کاربری به عنوان رمز عبور یا جزئیات حساب بانکی). این اطلاعات حساس می‌تواند توسط مهاجم مورد سوء استفاده قرار گیرد، به‌عنوان مثال، از طریق رابط‌های مدیریتی یا آسیب‌پذیری‌های شناخته شده وب سرور یا پیکربندی نادرست (مانند پیکربندی نادرست معروف `server-status` در سرورهای HTTP مبتنی بر آپاچی).

گزارش‌های رویداد اغلب حاوی داده‌هایی هستند که برای یک مهاجم مفید هستند (نشت اطلاعات) یا می‌توانند مستقیماً در اکسپلویت‌ها استفاده شوند:

- اطلاعات اشکال زدایی (دیباگ)
- ردپاها
- نام های کاربری
- نام اجزای سیستم
- آدرس های IP داخلی
- داده های شخصی کمتر حساس (مانند آدرس های ایمیل، آدرس های پستی و شماره تلفن های مرتبط با افراد نام برده شده)
- داده های کسب و کار

همچنین، در برخی از حوزه‌های قضایی، ذخیره برخی اطلاعات حساس در فایل‌های گزارش، مانند داده‌های شخصی، ممکن است شرکت را ملزم به اعمال قوانین حفاظت از داده‌ها کند که در پایگاه‌های داده پشتیبان خود برای فایل‌های گزارش نیز اعمال می‌کنند. و عدم انجام این کار، حتی به صورت ناآگاهانه، ممکن است تحت قوانین حفاظت از داده های اعمال شده جریمه هایی را به همراه داشته باشد.

لیست گسترده تری از اطلاعات حساس عبارتند از:

- کد منبع (سورس کد) برنامه
- مقادیر شناسایی جلسه
- اکسس توکن ها
- داده های شخصی حساس و برخی از اشکال اطلاعات شناسایی شخصی (PII)
- رمزهای عبور احراز هویت
- رشته های اتصال پایگاه داده
- کلیدهای رمزگذاری
- اطلاعات دارنده حساب بانکی یا کارت پرداخت
- داده هایی با طبقه بندی امنیتی بالاتر از سیستم ورود به سیستم مجاز به ذخیره سازی هستند
- اطلاعات تجاری حساس
- اطلاعاتی که جمع آوری آن در حوزه قضایی مربوطه غیرقانونی است
- اطلاعاتی که کاربر از جمع‌آوری انصراف داده است، یا به آن رضایت نداده است، مثلاً از ردیابی نکنید، یا جایی که رضایت جمع‌آوری منقضی شده است.

#### محل لاگ (Log Location)

معمولاً سرورها گزارش‌ (لاگ) های محلی از اقدامات و خطاهای خود ایجاد می‌کنند و دیسک سیستمی را که سرور روی آن در حال اجراست مصرف می‌کند. با این حال، اگر سرور به خطر بیفتد، گزارش های آن می تواند توسط نفوذگر پاک شود تا تمام آثار حمله و روش های آن پاک شود. اگر این اتفاق بیفتد، مدیر سیستم هیچ اطلاعی از نحوه وقوع حمله یا محل قرار گرفتن منبع حمله نخواهد داشت. در واقع، اکثر کیت‌های ابزار مهاجم شامل یک "log zapper" هستند که می‌تواند هر گزارشی را که اطلاعات داده شده را در خود نگه می‌دارد (مانند آدرس IP مهاجم) پاک کند و به طور معمول در کیت‌های ریشه در سطح سیستم مهاجم استفاده می‌شود.

در نتیجه، عاقلانه تر است که گزارش ها را در یک مکان جداگانه و نه در خود وب سرور نگهداری کنید. این امر همچنین تجمیع گزارش‌ها را از منابع مختلف که به یک برنامه ارجاع می‌دهند (مانند موارد فارم وب سرور) آسان‌تر می‌کند و همچنین انجام تجزیه و تحلیل گزارش (که می‌تواند فشرده CPU باشد) را بدون تأثیر بر خود سرور آسان‌تر می‌کند.

#### ذخیره‌سازی لاگ (Log Storage)

اگر لاگ ها به‌درستی ذخیره نشده باشند، می‌توانند شرایط انکار سرویس را معرفی کنند. هر مهاجمی که منابع کافی داشته باشد می‌تواند تعداد کافی درخواست ایجاد کند که فضای اختصاص داده شده برای فایل‌های لاگ را پر کند، در صورتی که به طور خاص از انجام این کار جلوگیری نشود. با این حال، اگر سرور به درستی پیکربندی نشده باشد، فایل های گزارش (لاگ) در همان پارتیشن دیسکی که برای نرم افزار سیستم عامل یا خود برنامه استفاده می شود، ذخیره می شود. این بدان معناست که اگر دیسک پر شود، سیستم عامل ممکن است از کار بیفتد زیرا قادر به نوشتن روی دیسک نیست.

معمولاً در سیستم‌های یونیکس، لاگ‌ها در var/ قرار می‌گیرند (اگرچه برخی از نصب‌های سرور ممکن است در opt/ یا usr/local/ باشند) و مهم است که مطمئن شوید دایرکتوری‌هایی که لاگ‌ها در آن ذخیره می‌شوند در یک پارتیشن جداگانه هستند. در برخی موارد و به منظور جلوگیری از تحت تاثیر قرار گرفتن لاگ های سیستم، دایرکتوری log خود نرم افزار سرور (مانند var/log/apache/ در وب سرور آپاچی) باید در یک پارتیشن اختصاصی ذخیره شود.

این بدان معنا نیست که لاگ‌ها باید رشد کنند تا سیستم فایلی که در آن قرار دارند پر شود.

آزمایش این شرایط در محیط‌های تولیدی به آسانی و به همان اندازه خطرناک است، مانند شلیک تعداد کافی و پایدار درخواست‌ها برای دیدن اینکه آیا این درخواست‌ها ثبت شده‌اند و آیا امکان پر کردن پارتیشن گزارش از طریق این درخواست‌ها وجود دارد یا خیر. در برخی از محیط‌ها که پارامترهای QUERY_STRING نیز بدون توجه به اینکه از طریق درخواست‌های GET (دریافت) یا POST (ارسال) تولید می‌شوند، ثبت می‌شوند، می‌توان پرس‌و‌جوهای بزرگی را شبیه‌سازی کرد که گزارش‌ها را سریع‌تر پر می‌کند، زیرا به طور معمول، یک درخواست تنها باعث می شود که فقط مقدار کمی از داده ها ثبت شود، مانند تاریخ و زمان، آدرس IP منبع، درخواست URI و نتیجه سرور.

#### چرخش لاگ (Log Rotation)

اکثر سرورها (اما تعداد کمی از برنامه های سفارشی) گزارش ها را می چرخانند تا از پر کردن سیستم فایلی که در آن قرار دارند جلوگیری کنند. فرض در چرخش لاگ ها این است که اطلاعات موجود در آنها فقط برای مدت زمان محدودی ضروری است.

این ویژگی باید آزمایش شود تا اطمینان حاصل شود که:

- گزارش‌ها برای مدت زمانی که در سیاست امنیتی تعریف شده است، نه بیشتر و نه کمتر، نگهداری می‌شوند.
- گزارش‌ها پس از چرخش فشرده می‌شوند (این یک کار راحتی است، زیرا به این معنی است که گزارش‌های بیشتری برای همان فضای دیسک موجود ذخیره می‌شوند).
- مجوزهای سیستم فایل فایل‌های گزارش چرخش شده مشابه (یا سخت‌گیرانه‌تر) با مجوزهای خود فایل‌های گزارش است. به عنوان مثال، سرورهای وب باید در گزارش‌هایی که استفاده می‌کنند بنویسند، اما در واقع نیازی به نوشتن در گزارش‌های چرخانده ندارند، به این معنی که مجوزهای فایل‌ها را می‌توان پس از چرخش تغییر داد تا از تغییر آنها توسط فرآیند وب سرور جلوگیری شود.

برخی از سرورها ممکن است زمانی که به اندازه معینی رسیدند، گزارش‌ها را بچرخانند. اگر این اتفاق بیفتد، باید اطمینان حاصل شود که مهاجم نمی تواند گزارش ها را مجبور به چرخش کند تا ردهای خود را پنهان کند.

#### کنترل دسترسی لاگ (Log Access Control)

اطلاعات گزارش رویداد هرگز نباید برای کاربران نهایی قابل مشاهده باشد. حتی مدیران وب نیز نباید قادر به دیدن چنین گزارش‌هایی باشند، زیرا جداسازی کنترل‌های وظیفه را نقض می‌کند. اطمینان حاصل کنید که هر طرح کنترل دسترسی که برای محافظت از دسترسی به گزارش‌های خام استفاده می‌شود و هر برنامه‌ای که قابلیت مشاهده یا جستجوی گزارش‌ها را ارائه می‌کند، با طرح‌های کنترل دسترسی برای سایر نقش‌های کاربر برنامه مرتبط نباشد. همچنین نباید هیچ داده گزارشی توسط کاربران احراز هویت نشده قابل مشاهده باشد.

#### بررسی لاگ (Log Review)

بررسی گزارش‌ها می‌تواند برای چیزی بیش از استخراج آمار استفاده از فایل‌ها در سرورهای وب (که معمولاً همان چیزی است که اکثر برنامه‌های مبتنی بر گزارش روی آن تمرکز می‌کنند) استفاده می‌شود، اما همچنین برای تعیین اینکه آیا حملات در وب سرور انجام می‌شوند یا خیر.

برای تجزیه و تحلیل حملات وب سرور، فایل های ثبت خطای سرور باید تجزیه و تحلیل شوند. بررسی باید بر روی موارد زیر متمرکز شود:

- پیغام های خطای 40x (not found) (یافت نشد). مقدار زیادی از اینها از یک منبع ممکن است نشان دهنده استفاده از ابزار اسکنر CGI علیه وب سرور باشد.
- پیام های 50x (server error) (خطای سرور). اینها می تواند نشانه ای از سوء استفاده مهاجم از بخش هایی از برنامه باشد که به طور غیرمنتظره ای از کار می افتند. به عنوان مثال، فازهای اول یک حمله تزریق SQL زمانی که پرس و جوی SQL (query) به درستی ساخته نشده باشد و اجرای آن در پایگاه داده پشتیبان ناموفق باشد، این پیام خطا را ایجاد می کند.

آمار گزارش یا تجزیه و تحلیل نباید در همان سروری که گزارش‌ها را تولید می‌کند، تولید یا ذخیره شود. در غیر این صورت، یک مهاجم ممکن است، از طریق آسیب پذیری وب سرور یا پیکربندی نامناسب، به آنها دسترسی پیدا کند و اطلاعات مشابهی را که توسط خود فایل های گزارش فاش می شود، بازیابی کند.

## منابع

- Apache
    - Apache Security, by Ivan Ristic, O’reilly, March 2005.
    - [Apache Security Secrets: Revealed (Again), Mark Cox, November 2003](https://awe.com/mark/talks/apachecon2003us.html)
    - [Apache Security Secrets: Revealed, ApacheCon 2002, Las Vegas, Mark J Cox, October 2002](https://awe.com/mark/talks/apachecon2002us.html)
    - [Performance Tuning](https://httpd.apache.org/docs/current/misc/perf-tuning.html)
- Lotus Domino
    - Lotus Security Handbook, William Tworek et al., April 2004, available in the IBM Redbooks collection
    - Lotus Domino Security, an X-force white-paper, Internet Security Systems, December 2002
    - Hackproofing Lotus Domino Web Server, David Litchfield, October 2001
- Microsoft IIS
    - [Security Best Practices for IIS 8](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj635855(v=ws.11))
    - [CIS Microsoft IIS Benchmarks](https://www.cisecurity.org/benchmark/microsoft_iis/)
    - Securing Your Web Server (Patterns and Practices), Microsoft Corporation, January 2004
    - IIS Security and Programming Countermeasures, by Jason Coombs
    - From Blueprint to Fortress: A Guide to Securing IIS 5.0, by John Davis, Microsoft Corporation, June 2001
    - Secure Internet Information Services 5 Checklist, by Michael Howard, Microsoft Corporation, June 2000
- Red Hat’s (formerly Netscape’s) iPlanet
    - Guide to the Secure Configuration and Administration of iPlanet Web Server, Enterprise Edition 4.1, by James M Hayes, The Network Applications Team of the Systems and Network Attack Center (SNAC), NSA, January 2001
- WebSphere
    - IBM WebSphere V5.0 Security, WebSphere Handbook Series, by Peter Kovari et al., IBM, December 2002.
    - IBM WebSphere V4.0 Advanced Edition Security, by Peter Kovari et al., IBM, March 2002.
- General
    - [Logging Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html), OWASP
    - [SP 800-92](https://csrc.nist.gov/publications/detail/sp/800-92/final) Guide to Computer Security Log Management, NIST
    - [PCI DSS v3.2.1](https://www.pcisecuritystandards.org/document_library) Requirement 10 and PA-DSS v3.2 Requirement 4, PCI Security Standards Council

- Generic:
    - [CERT Security Improvement Modules: Securing Public Web Servers](https://resources.sei.cmu.edu/asset_files/SecurityImprovementModule/2000_006_001_13637.pdf)
