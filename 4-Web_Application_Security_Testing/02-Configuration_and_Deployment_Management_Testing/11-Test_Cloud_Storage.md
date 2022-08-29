# Test Cloud Storage (fa-IR)

آزمایش فضای ذخیره سازی ابری (فارسی)

|شناسه          |
|------------|
|WSTG-CONF-11|

## خلاصه

سرویس‌های ذخیره‌سازی ابری برنامه‌ها و سرویس‌های وب را برای ذخیره و دسترسی به اشیاء در سرویس ذخیره‌سازی تسهیل می‌کنند. با این حال، پیکربندی نادرست کنترل دسترسی ممکن است منجر به قرار گرفتن در معرض اطلاعات حساس، دستکاری داده ها یا دسترسی غیرمجاز شود.

یک مثال شناخته شده جایی است که یک سطل S3 آمازون (Amazon S3 bucket) به اشتباه پیکربندی شده است، اگرچه سایر سرویس های ذخیره سازی ابری نیز ممکن است در معرض خطرات مشابهی باشند. به‌طور پیش‌فرض، تمام سطل‌های S3 خصوصی هستند و فقط برای کاربرانی که صراحتاً به آنها اجازه دسترسی داده شده است، می‌توان به آنها دسترسی داشت. کاربران می توانند هم به خود سطل و هم به اشیاء جداگانه ذخیره شده در آن سطل دسترسی عمومی بدهند. این ممکن است منجر به این شود که یک کاربر غیرمجاز بتواند فایل‌های جدید را آپلود کند، فایل‌های ذخیره شده را تغییر دهد یا بخواند.

## اهداف آزمایش

- ارزیابی کنید که پیکربندی کنترل دسترسی برای سرویس‌های ذخیره‌سازی به درستی در جای خود قرار دارد.

## چگونه آزمایش کنیم

ابتدا URL را برای دسترسی به داده ها در سرویس ذخیره سازی شناسایی کنید و سپس آزمایش های زیر را در نظر بگیرید:

- داده های غیرمجاز را بخوانید
- یک فایل دلخواه جدید آپلود کنید

می‌توانید از curl برای آزمایش‌ها با دستورات زیر استفاده کنید و ببینید آیا اقدامات غیرمجاز را می‌توان با موفقیت انجام داد.

برای آزمایش توانایی خواندن یک شی:

```bash
curl -X GET https://<cloud-storage-service>/<object>
```

برای آزمایش توانایی آپلود فایل:

```bash
curl -X PUT -d 'test' 'https://<cloud-storage-service>/test.txt'
```

### آزمایش پیکربندی نادرست سطل S3 آمازون (Testing for Amazon S3 Bucket Misconfiguration)

ا URL های سطل S3 آمازون از یکی از دو فرمت پیروی می کنند، یا سبک میزبان مجازی (virtual host style) یا سبک مسیر (path-style).

- دسترسی به سبک میزبانی مجازی

```text
https://bucket-name.s3.Region.amazonaws.com/key-name
```

در مثال زیر، `my-bucket` نام سطل، `us-west-2` منطقه، و `puppy.png` نام کلید است:

```text
https://my-bucket.s3.us-west-2.amazonaws.com/puppy.png
```

- دسترسی به سبک مسیر

```text
https://s3.Region.amazonaws.com/bucket-name/key-name
```

مانند بالا، در مثال زیر،` my-bucket` نام سطل، `us-west-2` منطقه و `puppy.png` نام کلید است:

```text
https://s3.us-west-2.amazonaws.com/my-bucket/puppy.jpg
```

برای برخی از مناطق، نقطه پایانی جهانی قدیمی که یک نقطه پایانی خاص منطقه را مشخص نمی کند، می تواند استفاده شود. فرمت آن نیز به سبک میزبانی مجازی یا به سبک مسیر است.

- دسترسی به سبک میزبانی مجازی

```text
https://bucket-name.s3.amazonaws.com
```

- دسترسی به سبک مسیر

```text
https://s3.amazonaws.com/bucket-name
```

#### <div dir="rtl" align="right">URL سطل را شناسایی کنید (Identify Bucket URL)</div>

برای آزمایش جعبه سیاه، URL های S3 را می توان در پیام های HTTP یافت. مثال زیر نشان می دهد که یک URL سطلی در تگ `img` در یک پاسخ HTTP ارسال شده است.

```html
...
<img src="https://my-bucket.s3.us-west-2.amazonaws.com/puppy.png">
...
```

برای آزمایش جعبه خاکستری، می توانید URL های سطلی را از رابط وب آمازون، اسناد، کد منبع یا هر منبع موجود دیگری دریافت کنید.

#### آزمایش با AWS-CLI &#x202b;(Testing with AWS-CLI)

علاوه بر آزمایش با کرل، می توانید با ابزار خط فرمان AWS نیز آزمایش کنید. در این مورد از پروتکل `//:s3` استفاده می شود.

##### فهرست کنید (List)

دستور زیر تمام اشیاء سطل را هنگامی که به صورت عمومی پیکربندی می شود فهرست می کند.

```bash
aws s3 ls s3://<bucket-name>
```

##### بارگذاری (Upload)

دستور زیر برای بارگذاری (آپلود) یک فایل است

```bash
aws s3 cp arbitrary-file s3://bucket-name/path-to-save
```

این مثال زمانی که آپلود با موفقیت انجام شده است نتیجه را نشان می دهد.

```bash
$ aws s3 cp test.txt s3://bucket-name/test.txt
upload: ./test.txt to s3://bucket-name/test.txt
```

این مثال نتیجه را هنگامی که آپلود ناموفق است نشان می دهد.

```bash
$ aws s3 cp test.txt s3://bucket-name/test.txt
upload failed: ./test2.txt to s3://bucket-name/test2.txt An error occurred (AccessDenied) when calling the PutObject operation: Access Denied
```

##### برداشتن (حذف کردن) (Remove)

دستور زیر برای حذف یک شی است

```bash
aws s3 rm s3://bucket-name/object-to-remove
```

## ابزارها

- [AWS CLI](https://aws.amazon.com/cli/)

## منابع

- [Working with Amazon S3 Buckets](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingBucket.html)
- [flAWS 2](http://flaws2.cloud)
