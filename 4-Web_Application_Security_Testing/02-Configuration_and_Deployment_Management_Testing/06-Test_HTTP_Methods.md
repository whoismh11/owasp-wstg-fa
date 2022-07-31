# Test HTTP Methods (fa-IR)

روش های HTTP را آزمایش کنید (فارسی)

|شناسه          |
|------------|
|WSTG-CONF-06|

## خلاصه

ا HTTP تعدادی روش (یا افعال) را ارائه می دهد که می توان از آنها برای انجام اقدامات در وب سرور استفاده کرد. در حالی که GET و POST با اختلاف رایج‌ترین روش‌هایی هستند که برای دسترسی به اطلاعات ارائه‌شده توسط وب سرور استفاده می‌شوند، روش‌های مختلفی نیز وجود دارد که ممکن است پشتیبانی شوند و گاهی اوقات می‌توانند توسط مهاجمان مورد سوء استفاده قرار گیرند.

ا [RFC 7231](https://datatracker.ietf.org/doc/html/rfc7231) روش های اصلی درخواست معتبر HTTP (یا افعال) را تعریف می کند، اگرچه روش های اضافی در RFC های دیگر مانند [RFC 5789](https://datatracker.ietf.org/doc/html/rfc5789) اضافه شده است. چندین مورد از این افعال برای اهداف مختلف در برنامه‌های [RESTful](https://en.wikipedia.org/wiki/Representational_state_transfer)، که در جدول زیر فهرست شده‌اند، دوباره استفاده شده‌اند.

| روش | هدف اصلی | RESTful هدف |
|--------|------------------|-----------------|
| [`GET`](https://datatracker.ietf.org/doc/html/rfc7231#section-4.3.1) | درخواست یک فایل | درخواست یک شی|
| [`HEAD`](https://datatracker.ietf.org/doc/html/rfc7231#section-4.3.2) | &#x202b;درخواست یک فایل، اما فقط هدر های HTTP را برمیگرداند | |
| [`POST`](https://datatracker.ietf.org/doc/html/rfc7231#section-4.3.3) | ارسال داده ها | |
| [`PUT`](https://datatracker.ietf.org/doc/html/rfc7231#section-4.3.4) | آپلود یک فایل | ایجاد یک شی |
| [`DELETE`](https://datatracker.ietf.org/doc/html/rfc7231#section-4.3.5) | حذف یک فایل | حذف یک شی |
| [`CONNECT`](https://datatracker.ietf.org/doc/html/rfc7231#section-4.3.6) | برقرار کردن ازتباط با یک سیستم دیگر | |
| [`OPTIONS`](https://datatracker.ietf.org/doc/html/rfc7231#section-4.3.7) | &#x202b;فهرست کردن روش های HTTP پشتیبانی شده | &#x202b;انجام دادن یک درخواست [CORS Preflight](https://developer.mozilla.org/en-US/docs/Glossary/Preflight_request) |
| [`TRACE`](https://datatracker.ietf.org/doc/html/rfc7231#section-4.3.8) | &#x202b;تکرار (Echo) کردن درخواست HTTP برای اهداف اشکال زدایی | |
| [`PATCH`](https://datatracker.ietf.org/doc/html/rfc5789#section-2) |  | تغییر دادن یک شی |

## اهداف آزمایش

- روش های HTTP پشتیبانی شده را برشمارید.
- آزمایش بای پس کنترل دسترسی.
- روش های غلبه بر روش HTTP را آزمایش کنید.

## چگونه آزمایش کنیم
