# Test for Subdomain Takeover (fa-IR)

آزمایش تصاحب زیردامنه (فارسی)

|شناسه          |
|------------|
|WSTG-CONF-10|

## خلاصه

بهره برداری موفقیت آمیز از این نوع آسیب پذیری به دشمن اجازه می دهد تا زیردامنه قربانی را ادعا کرده و کنترل کند. این حمله متکی بر موارد زیر است:

1. رکورد زیردامنه سرور DNS خارجی قربانی طوری پیکربندی شده است که به منبع / سرویس خارجی / نقطه پایانی موجود یا غیر فعال اشاره کند. گسترش محصولات XaaS (هر چیزی به عنوان سرویس) و خدمات ابری عمومی، اهداف بالقوه زیادی را برای در نظر گرفتن ارائه می دهد.
2. ارائه‌دهنده سرویس میزبان منبع/سرویس خارجی/نقطه پایانی تأیید مالکیت زیردامنه را به درستی انجام نمی‌دهد.

در صورت موفقیت‌آمیز بودن تصاحب زیردامنه‌ها، طیف گسترده‌ای از حملات ممکن است (ارائه محتوای مخرب، فیشینگ، سرقت کوکی‌های جلسه کاربر، اطلاعات کاربری، و غیره). این آسیب پذیری می تواند برای طیف گسترده ای از سوابق منابع DNS از جمله: `A`، `CNAME`، `MX`، `NS`، `TXT` و غیره مورد سوء استفاده قرار گیرد. از نظر شدت حمله، تصاحب زیردامنه `NS` (اگرچه احتمال کمتری دارد) بیشترین تأثیر را دارد زیرا یک حمله موفقیت آمیز می تواند منجر به کنترل کامل بر کل ناحیه DNS &#x202b;(DNS zone) و دامنه قربانی شود.

### گیت هاب (GitHub)

1. قربانی (victim.com) از GitHub برای توسعه استفاده می کند و یک رکورد DNS (`coderepo.victim.com`) برای دسترسی به آن پیکربندی کرده است.
2. قربانی تصمیم می گیرد مخزن کد خود را از GitHub به یک پلتفرم تجاری منتقل کند و `coderepo.victim.com` را از سرور DNS خود حذف نمی کند.
3. یک دشمن متوجه می شود که `coderepo.victim.com` در GitHub میزبانی شده است و از صفحات GitHub برای ادعای `coderepo.victim.com` با استفاده از حساب GitHub خود استفاده می کند.

### دامنه منقضی شده (Expired Domain)

1. قربانی (victim.com) صاحب دامنه دیگری است (victimotherdomain.com) و از یک رکورد CNAME (www) برای ارجاع به دامنه دیگر استفاده می کند (`www.victim.com` --> `victimotherdomain.com`)
2. در برخی مواقع، victimotherdomain.com منقضی می‌شود و برای هر کسی در دسترس است. از آنجایی که رکورد CNAME از ناحیه DNS ا victim.com حذف نمی‌شود، هرکسی که `victimotherdomain.com` را ثبت می‌کند کنترل کاملی بر `www.victim.com` دارد تا زمانی که رکورد DNS وجود داشته باشد.

## اهداف آزمایش

- تمام دامنه های ممکن (قبلی و فعلی) را برشمارید.
- دامنه های فراموش شده یا پیکربندی نادرست را شناسایی کنید.

## چگونه آزمایش کنیم

### آزمایش جعبه سیاه (Black-Box Testing)

اولین قدم، شمارش سرورهای DNS قربانی و سوابق منابع است. راه های متعددی برای انجام این کار وجود دارد، به عنوان مثال شمارش DNS با استفاده از فهرستی از فرهنگ لغات زیردامنه های رایج، DNS brute force یا استفاده از موتورهای جستجوی وب و سایر منابع داده OSINT.

با استفاده از دستور dig، آزمایش‌کننده پیام‌های پاسخ سرور DNS زیر را جستجو می‌کند که مستلزم بررسی بیشتر است:

- `NXDOMAIN`
- `SERVFAIL`
- `REFUSED`
- `no servers could be reached.`

#### آزمایش DNS A، تصاحب زیردامنه رکورد CNAME &#x202b;(Testing DNS A, CNAME Record Subdomain Takeover)

یک شمارش اولیه DNS در دامنه قربانی (`victim.com`) با استفاده از `dnsrecon`:

```bash
$ ./dnsrecon.py -d victim.com
[*] Performing General Enumeration of Domain: victim.com
...
[-] DNSSEC is not configured for victim.com
[*]      A subdomain.victim.com 192.30.252.153
[*]      CNAME subdomain1.victim.com fictioussubdomain.victim.com
...
```

شناسایی کنید که کدام سوابق منبع DNS مرده است و به سرویس های غیرفعال/استفاده نشده اشاره کنید. با استفاده از دستور dig برای رکورد `CNAME`:

```bash
$ dig CNAME fictioussubdomain.victim.com
; <<>> DiG 9.10.3-P4-Ubuntu <<>> ns victim.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 42950
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1
```

پاسخ‌های DNS زیر مستلزم بررسی بیشتر `NXDOMAIN` است :

برای آزمایش رکورد `A`، آزمایش کننده یک جستجوی پایگاه داده whois انجام می دهد و GitHub را به عنوان ارائه دهنده خدمات شناسایی می کند:

```bash
$ whois 192.30.252.153 | grep "OrgName"
OrgName: GitHub, Inc.
```

آزمایش‌کننده از `subdomain.victim.com` بازدید می‌کند یا یک درخواست HTTP GET صادر می‌کند که پاسخ "404 - File not found" را برمی‌گرداند که نشانه واضحی از آسیب‌پذیری است.

![GitHub 404 File Not Found response](images/subdomain_takeover_ex1.jpeg)\
*شکل 1-4.2.10: پاسخ GitHub 404 File Not Found*

آزمایش‌کننده دامنه را با استفاده از صفحات GitHub ادعا می‌کند:

![GitHub claim domain](images/subdomain_takeover_ex2.jpeg)\
*شکل 2-4.2.10: ادعای دامنه GitHub*

#### آزمایش تصاحب زیردامنه رکورد NS &#x202b;(Testing NS Record Subdomain Takeover)

همه سرورهای نام (nameservers) دامنه را در محدوده شناسایی کنید:

```bash
$ dig ns victim.com +short
ns1.victim.com
nameserver.expireddomain.com
```

در این مثال ساختگی، آزمایش کننده با جستجوی ثبت کننده دامنه، فعال بودن دامنه `expireddomain.com` را بررسی می کند. اگر دامنه برای خرید در دسترس باشد، زیردامنه آسیب پذیر است.

پاسخ‌های DNS روبرو مستلزم بررسی بیشتر است: `SERVFAIL` یا `REFUSED`.

### آزمایش جعبه خاکستری (Gray-Box Testing)

آزمایش کننده فایل ناحیه DNS را در دسترس دارد که به این معنی است که شمارش DNS ضروری نیست. روش آزمایش یکسان است.

## اصلاح

برای کاهش خطر تصاحب زیردامنه، رکورد(های) منبع آسیب پذیر DNS باید از ناحیه DNS حذف شود. نظارت مستمر و بررسی های دوره ای به عنوان بهترین عمل توصیه می شود.

## ابزارها

- [dig - man page](https://linux.die.net/man/1/dig)
- [recon-ng - Web Reconnaissance framework](https://github.com/lanmaster53/recon-ng)
- [theHarvester - OSINT intelligence gathering tool](https://github.com/laramies/theHarvester)
- [Sublist3r - OSINT subdomain enumeration tool](https://github.com/aboul3la/Sublist3r)
- [dnsrecon - DNS Enumeration Script](https://github.com/darkoperator/dnsrecon)
- [OWASP Amass DNS enumeration](https://github.com/OWASP/Amass)
- [OWASP Domain Protect](https://owasp.org/www-project-domain-protect)

## منابع

- [HackerOne - A Guide To Subdomain Takeovers](https://www.hackerone.com/blog/Guide-Subdomain-Takeovers)
- [Subdomain Takeover: Basics](https://0xpatrik.com/subdomain-takeover-basics/)
- [Subdomain Takeover: Going beyond CNAME](https://0xpatrik.com/subdomain-takeover-ns/)
- [can-i-take-over-xyz - A list of vulnerable services](https://github.com/EdOverflow/can-i-take-over-xyz/)
- [OWASP AppSec Europe 2017 - Frans Rosén: DNS hijacking using cloud providers – no verification needed](https://2017.appsec.eu/presos/Developer/DNS%20hijacking%20using%20cloud%20providers%20%E2%80%93%20no%20verification%20needed%20-%20Frans%20Rosen%20-%20OWASP_AppSec-Eu_2017.pdf)
