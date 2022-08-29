# Testing for Content Security Policy (fa-IR)

آزمایش برای سیاست امنیتی محتوا (فارسی)

|شناسه          |
|------------|
|WSTG-CONF-12|

## خلاصه

سیاست (خط‌مشی) امنیت محتوا (CSP) یک خط‌مشی فهرست مجاز اعلامی است که از طریق سرصفحه پاسخ `Content-Security-Policy` یا عنصر `<meta>` معادل آن اعمال می‌شود. این به توسعه دهندگان اجازه می دهد تا منابعی را که از آنها منابعی مانند جاوا اسکریپت، CSS، تصاویر، فایل ها و غیره بارگیری می شوند، محدود کنند. CSP یک تکنیک دفاعی موثر برای کاهش خطر آسیب‌پذیری‌هایی مانند Cross Site Scripting (XSS) و Clickjacking است.

سیاست امنیت محتوا از دستورالعمل‌هایی پشتیبانی می‌کند که اجازه می‌دهد تا جریان سیاست‌ها را کنترل کند. ( برای جزئیات بیشتر به [منابع](#منابع) مراجعه کنید.)

## اهداف آزمایش

- برای شناسایی پیکربندی‌های نادرست، سربرگ Content-Security-Policy یا متا را مرور کنید.

## چگونه آزمایش کنیم

برای آزمایش پیکربندی نادرست در CSP، با بررسی سرصفحه پاسخ HTTP ا `Content-Security-Policy` یا عنصر CSP ا `meta` در یک ابزار پراکسی، به دنبال تنظیمات ناامن باشید:

- دستورالعمل `unsafe-inline` اسکریپت ها یا استایل های درون خطی را فعال می کند که برنامه ها را مستعد حملات XSS می کند.
- دستورالعمل `unsafe-eval` اجازه می دهد تا `()eval` در برنامه استفاده شود.
- دستورالعمل `unsafe-hashes` استفاده از اسکریپت ها/سبک های درون خطی را با فرض مطابقت با هش های (hashes) مشخص شده امکان پذیر می کند.
- منابعی مانند اسکریپت ها را می توان از هر مبدا ای با استفاده از منبع عام (`*`) بارگیری (لود) کرد.
  - همچنین حروف عام را بر اساس مطابقت های جزئی در نظر بگیرید، مانند: `*//:https` یا `cdn.com.*`.
  - در نظر بگیرید که آیا منابع فهرست شده مجاز، نقاط پایانی JSONP را ارائه می دهند که ممکن است برای دور زدن CSP یا سیاست همان مبدا استفاده شوند.
- کادربندی را می توان برای همه مبداها با استفاده از منبع عام (`*`) برای دستورالعمل `frame-ancestors` فعال کرد.
- برنامه های حیاتی تجاری باید به استفاده از یک سیاست سختگیرانه نیاز داشته باشند.

## اصلاح

یک سیاست امنیتی محتوای قوی را پیکربندی کنید که سطح حمله برنامه را کاهش دهد. توسعه دهندگان می توانند با استفاده از ابزارهای آنلاین مانند [Google CSP Evaluator](https://csp-evaluator.withgoogle.com/)، قدرت سیاست امنیت محتوا را تأیید کنند.

### سیاست سختگیرانه (Strict Policy)

سیاست سختگیرانه سیاستی است که در برابر حملات کلاسیک ذخیره شده، منعکس شده و برخی از حملات DOM XSS محافظت می کند و باید هدف بهینه هر تیمی باشد که سعی در پیاده سازی CSP دارد.

گوگل پیش رفت و یک راهنمای برای اتخاذ یک CSP سختگیرانه بر اساس nonces تنظیم کرد. بر اساس یک ارائه در [LocoMocoSec](https://speakerdeck.com/lweichselbaum/csp-a-successful-mess-between-hardening-and-mitigation?slide=55)، دو سیاست زیر را می توان برای اعمال یک سیاست سخت استفاده کرد:

سیاست سختگیرانه متوسط:

```HTTP
script-src 'nonce-r4nd0m' 'strict-dynamic';
object-src 'none'; base-uri 'none';
```

سیاست سختگیرانه قفل شده:

```HTTP
script-src 'nonce-r4nd0m';
object-src 'none'; base-uri 'none';
```

## ابزارها

- [Google CSP Evaluator](https://csp-evaluator.withgoogle.com/)
- [CSP Auditor - Burp Suite Extension](https://portswigger.net/bappstore/35237408a06043e9945a11016fcbac18)
- [CSP Generator Chrome](https://chrome.google.com/webstore/detail/content-security-policy-c/ahlnecfloencbkpfnpljbojmjkfgnmdc) / [Firefox](https://addons.mozilla.org/en-US/firefox/addon/csp-generator/)

## منابع

- [OWASP Content Security Policy Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Content_Security_Policy_Cheat_Sheet.html)
- [Mozilla Developer Network: Content Security Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)
- [CSP Level 3 W3C](https://www.w3.org/TR/CSP3/)
- [CSP with Google](https://csp.withgoogle.com/docs/index.html)
- [Content-Security-Policy](https://content-security-policy.com/)
- [Google CSP Evaluator](https://csp-evaluator.withgoogle.com/)
- [CSP A Successful Mess Between Hardening And Mitigation](https://speakerdeck.com/lweichselbaum/csp-a-successful-mess-between-hardening-and-mitigation)
- [The unsafe-hashes Source List Keyword](https://content-security-policy.com/unsafe-hashes/)
