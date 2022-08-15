# Test HTTP Strict Transport Security (fa-IR)

آزمایش امنیت حمل و نقل سخت HTTP (فارسی)

|شناسه          |
|------------|
|WSTG-CONF-07|

## خلاصه

ویژگی HTTP Strict Transport Security (HSTS) به یک برنامه وب اجازه می دهد تا از طریق استفاده از سربرگ پاسخ ویژه به مرورگر اطلاع دهد که هرگز نباید با استفاده از HTTP رمزگذاری نشده با سرورهای دامنه مشخص شده ارتباط برقرار کند. در عوض، باید به طور خودکار تمام درخواست های اتصال را برای دسترسی به سایت از طریق HTTPS ایجاد کند. همچنین از نادیده گرفتن خطاهای گواهی توسط کاربران جلوگیری می کند.

با توجه به اهمیت این اقدام امنیتی، عاقلانه است که بررسی شود که وب سایت از این هدر HTTP استفاده می کند تا اطمینان حاصل شود که همه داده ها به صورت رمزگذاری شده بین مرورگر وب و سرور جابجا می شوند.

هدر امنیتی حمل و نقل سخت HTTP از دو دستورالعمل استفاده می کند:

- ا `max-age`: برای نشان دادن تعداد ثانیه هایی که مرورگر باید به طور خودکار تمام درخواست های HTTP را به HTTPS تبدیل کند.
- ا `includeSubDomains`: نشان می دهد که همه زیر دامنه های مرتبط باید از HTTPS استفاده کنند.
- ا `preload` غیر رسمی: برای نشان دادن اینکه دامنه(ها) در لیست(های) پیش بارگذاری قرار دارند و مرورگرها هرگز نباید بدون HTTPS متصل شوند.
  - این توسط همه مرورگرهای اصلی پشتیبانی می شود اما بخشی رسمی از مشخصات نیست. ( برای اطلاعات بیشتر به [hstspreload.org](https://hstspreload.org/) مراجعه کنید.)

در اینجا مثالی از اجرای هدر HSTS آورده شده است:

`Strict-Transport-Security: max-age=31536000; includeSubDomains`

استفاده از این هدر توسط برنامه های وب باید بررسی شود تا مشخص شود که آیا می توان مشکلات امنیتی زیر را ایجاد کرد:

- مهاجمان ترافیک شبکه را استشمام (sniffing) می کنند و به اطلاعات منتقل شده از طریق یک کانال رمزگذاری نشده دسترسی پیدا می کنند.
- مهاجمان از یک دستکاری کننده در حمله میانی به دلیل مشکل پذیرش گواهینامه هایی که مورد اعتماد نیستند سوء استفاده می کنند.
- کاربرانی که به اشتباه آدرسی را در مرورگر وارد کرده‌اند که HTTP را به جای HTTPS قرار می‌دهد، یا کاربرانی که روی پیوندی در یک برنامه وب کلیک می‌کنند که به اشتباه استفاده از پروتکل HTTP را نشان می‌دهد.

## اهداف آزمایش

- هدر HSTS و اعتبار آن را بررسی کنید.

## چگونه آزمایش کنیم

وجود هدر HSTS را می توان با بررسی پاسخ سرور از طریق یک پروکسی رهگیری یا با استفاده از curl به شرح زیر تأیید کرد:

```bash
$ curl -s -D- https://owasp.org | grep -i strict
Strict-Transport-Security: max-age=31536000
```

## منابع

- [OWASP HTTP Strict Transport Security](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html)
- [OWASP Appsec Tutorial Series - Episode 4: Strict Transport Security](https://www.youtube.com/watch?v=zEV3HOuM_Vw)
- [HSTS Specification](https://tools.ietf.org/html/rfc6797)
- [Enable HTTP Strict Transport Security In Apache](https://https.cio.gov/hsts/)
- [Enable HTTP Strict Transport Security In Nginx](https://www.nginx.com/blog/http-strict-transport-security-hsts-and-nginx/)
