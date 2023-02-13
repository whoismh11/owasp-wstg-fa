# OWASP Web Security Testing Guide (fa-IR)

راهنمای آزمایش امنیت وب OWASP (فارسی)

## معرفی

راهنمای آزمایش امنیت وب OWASP، یک راهنمای جامع برای آزمایش امنیت برنامه ها و سرویس های وب است. OWASP-WSTG چارچوبی از بهترین شیوه های مورد استفاده توسط آزمایش کنندگان نفوذ و سازمان ها در سراسر جهان را ارائه میدهد.

### نکته

به دلیل فارسی (راست چین) بودن متن پروژه در گیت هاب، ممکن است هنگام کپی کردن متنی از پروژه، بعضی از کلمات انگلیسی که ابتدا یا انتهای آنها کاراکتر های متفرقه (مانند `/`، `.`، `~` و غیره) باشد، بصورت وارونه کپی شوند. فقط در صورتی این اتفاق میوفتد که متن انگلیسی همراه با متن فارسی در یک پاراگراف باشد. پس لطفاً بعد از کپی کردن، به صحیح بودن متن های انگلیسی دقت کنید (تمام متن ها در پروژه بصورت صحیح نمایش داده شده اند، تنها هنگام کپی کردن ممکن است این اتفاق بیوفتد).

درصورت ایجاد تغییرات یا انتشار بروزرسانی های جدید در پروژه منبع، این پروژه نیز در سریع ترین زمان ممکن بروزرسانی خواهد شد.

## فهرست مطالب

## 4. [Web Application Security Testing (آزمایش امنیت برنامه وب)](4-Web_Application_Security_Testing/)

### 4.0 [Introduction and Objectives (مقدمه و اهداف)](4-Web_Application_Security_Testing/00-Introduction_and_Objectives/README.md)

### 4.1 [Information Gathering (جمع آوری اطلاعات)](4-Web_Application_Security_Testing/01-Information_Gathering/README.md)

#### 4.1.1 [Conduct Search Engine Discovery Reconnaissance for Information Leakage (شناسایی و کشف موتورهای جستجو برای نشت اطلاعات)](4-Web_Application_Security_Testing/01-Information_Gathering/01-Conduct_Search_Engine_Discovery_Reconnaissance_for_Information_Leakage.md)

#### 4.1.2 [Fingerprint Web Server (انگشت نگاری وب سرور)](4-Web_Application_Security_Testing/01-Information_Gathering/02-Fingerprint_Web_Server.md)

#### 4.1.3 [Review Webserver Metafiles for Information Leakage (بررسی متافایل های وب سرور برای نشت اطلاعات)](4-Web_Application_Security_Testing/01-Information_Gathering/03-Review_Webserver_Metafiles_for_Information_Leakage.md)

#### 4.1.4 [Enumerate Applications on Webserver (شمارش برنامه ها در وب سرور)](4-Web_Application_Security_Testing/01-Information_Gathering/04-Enumerate_Applications_on_Webserver.md)

#### 4.1.5 [Review Webpage Content for Information Leakage (بررسی محتوای صفحه وب برای نشت اطلاعات)](4-Web_Application_Security_Testing/01-Information_Gathering/05-Review_Webpage_Content_for_Information_Leakage.md)

#### 4.1.6 [Identify Application Entry Points (شناسایی نقاط ورودی برنامه)](4-Web_Application_Security_Testing/01-Information_Gathering/06-Identify_Application_Entry_Points.md)

#### 4.1.7 [Map Execution Paths Through Application (مسیرهای اجرای نقشه از طریق برنامه)](4-Web_Application_Security_Testing/01-Information_Gathering/07-Map_Execution_Paths_Through_Application.md)

#### 4.1.8 [Fingerprint Web Application Framework (انگشت نگاری چارچوب برنامه وب)](4-Web_Application_Security_Testing/01-Information_Gathering/08-Fingerprint_Web_Application_Framework.md)

#### 4.1.9 [Fingerprint Web Application (انگشت نگاری برنامه وب)](4-Web_Application_Security_Testing/01-Information_Gathering/09-Fingerprint_Web_Application.md)

#### 4.1.10 [Map Application Architecture (معماری برنامه نقشه)](4-Web_Application_Security_Testing/01-Information_Gathering/10-Map_Application_Architecture.md)

### 4.2 [Configuration and Deployment Management Testing (آزمایش مدیریت پیکربندی و استقرار)](4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/README.md)

#### 4.2.1 [Test Network Infrastructure Configuration (آزمایش پیکربندی زیرساخت شبکه)](4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/01-Test_Network_Infrastructure_Configuration.md)

#### 4.2.2 [Test Application Platform Configuration (آزمایش پیکربندی پلتفرم برنامه)](4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/02-Test_Application_Platform_Configuration.md)

#### 4.2.3 [Test File Extensions Handling for Sensitive Information (آزمایش مدیریت پسوند فایل برای اطلاعات حساس)](4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/03-Test_File_Extensions_Handling_for_Sensitive_Information.md)

#### 4.2.4 [Review Old Backup and Unreferenced Files for Sensitive Information (مرور فایل های پشتیبان قدیمی و بدون مرجع برای اطلاعات حساس)](4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/04-Review_Old_Backup_and_Unreferenced_Files_for_Sensitive_Information.md)

#### 4.2.5 [Enumerate Infrastructure and Application Admin Interfaces (شمارش زیرساخت و رابط های مدیریت برنامه)](4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/05-Enumerate_Infrastructure_and_Application_Admin_Interfaces.md)

#### 4.2.6 [Test HTTP Methods &#x202b;(آزمایش روش های HTTP)](4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/06-Test_HTTP_Methods.md)

#### 4.2.7 [Test HTTP Strict Transport Security &#x202b;(آزمایش امنیت حمل و نقل سخت HTTP)](4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/07-Test_HTTP_Strict_Transport_Security.md)

#### 4.2.8 [Test RIA Cross Domain Policy &#x202b;(آزمایش سیاست دامنه متقابل RIA)](4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/08-Test_RIA_Cross_Domain_Policy.md)

#### 4.2.9 [Test File Permission (آزمایش مجوز فایل)](4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/09-Test_File_Permission.md)

#### 4.2.10 [Test for Subdomain Takeover (آزمایش تصاحب زیردامنه)](4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/10-Test_for_Subdomain_Takeover.md)

#### 4.2.11 [Test Cloud Storage (آزمایش فضای ذخیره سازی ابری)](4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/11-Test_Cloud_Storage.md)

#### 4.2.12 [Test for Content Security Policy (آزمایش برای سیاست امنیت محتوا)](4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/12-Test_for_Content_Security_Policy.md)

#### 4.2.13 [Test for Path Confusion (آزمایش برای سردرگمی مسیر)](4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/13-Test_for_Path_Confusion.md)

### 4.3 [Identity Management Testing (آزمایش مدیریت هویت)](4-Web_Application_Security_Testing/03-Identity_Management_Testing/README.md)

#### 4.3.1 [Test Role Definitions (آزمایش تعاریف نقش)](4-Web_Application_Security_Testing/03-Identity_Management_Testing/01-Test_Role_Definitions.md)

#### 4.3.2 [Test User Registration Process (آزمایش فرآیند ثبت نام کاربر)](4-Web_Application_Security_Testing/03-Identity_Management_Testing/02-Test_User_Registration_Process.md)

#### 4.3.3 [Test Account Provisioning Process (آزمایش فرآیند ارائه حساب)](4-Web_Application_Security_Testing/03-Identity_Management_Testing/03-Test_Account_Provisioning_Process.md)

#### 4.3.4 [Testing for Account Enumeration and Guessable User Account (آزمایش برای شمارش حساب و حساب کاربری قابل حدس زدن)](4-Web_Application_Security_Testing/03-Identity_Management_Testing/04-Testing_for_Account_Enumeration_and_Guessable_User_Account.md)

#### 4.3.5 [Testing for Weak or Unenforced Username Policy (آزمایش برای سیاست نام کاربری ضعیف یا غیرقابل اجرا)](4-Web_Application_Security_Testing/03-Identity_Management_Testing/05-Testing_for_Weak_or_Unenforced_Username_Policy.md)

### 4.4 [Authentication Testing (آزمایش احراز هویت)](4-Web_Application_Security_Testing/04-Authentication_Testing/README.md)

#### 4.4.1 [Testing for Credentials Transported over an Encrypted Channel (آزمایش برای اعتبارنامه های منتقل شده از طریق یک کانال رمزگذاری شده)](4-Web_Application_Security_Testing/04-Authentication_Testing/01-Testing_for_Credentials_Transported_over_an_Encrypted_Channel.md)

## [OWASP Top 10 &#x202b;(10 مورد برتر OWASP)](OWASP_Top10/)

## مشارکت

- [Discord](https://discord.gg/2JjvhAk)

## منبع

- [owasp.org/www-project-web-security-testing-guide](https://owasp.org/www-project-web-security-testing-guide/)
