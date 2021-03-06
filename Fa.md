<div dir="rtl">

# ماژول ند جی اس درگاه پی دات آی آر

[![N|Solid](https://pay.ir/assets/img/logo.png.pagespeed.ce.DAyscoRFh0.png)](https://pay.ir)

با استفاده از این پکیج میتوانید در پلتفرم ند جی اس و فریمورک های مختلف سمت سرور مانند اکسپرس از خدمات وبسایت 
پی دات آی آر به راحتی و بسرعت استفاده نمایید.
لازم به ذکر است که این ماژول از ویژگی های نسخه 6 اکمااسکریپت بهره میبرد.

این پیکج به سفارش شرکت پرداخت الکترونیک سامان تهیه و توسعه داده شده است.

# قدم اول - نصب و آماده سازی

برای استفاده از این پکیج ابتدا میبایست پکیج را با استفاده از دستور زیر به پروژتان اضافه کنید:

</div>

```sh
$ npm install payir --save
```
<div dir="rtl">

سپس میبایست آن را ریکوایر کرده و یک شی از آن بسازید. ورودی کانستراکت این کلاس ای پی آی درگاه شماست:

</div>

```js
const Payir = require('payir');
const gateway = new Payir('YOUR API KEY');
```

<div dir="rtl">

# قدم دوم - ارسال درخواست

## متد send

توسط این متد میتوانید یک درخواست پرداخت ایجاد کنید.
این متد به ترتیب مبلغ تراکنش، آدرس بازگشت از بانک و شماره فاکتور (اختیاری) را از شما دریافت میکند.

این پکیج جهت اعلام نتیجه درخواست از پرامیس ها استفاده میکند. پس خیالتان راحت، از شر کال بک فانکشن ها راحت هستید!

</div>

```js
app.get('/', (req, res) => {
    gateway.send(1000, 'http://your-site.ir/verify')
            .then(link => res.redirect(link))
            .catch(error => res.end("<head><meta charset='utf8'></head>" + error));
});
```

<div dir="rtl">

در صورتی که عملیات موفقیت آمیز باشد، لینک پرداخت توسط بخش 

`then`

در دسترس خواهد بود و میتوانید کاربر را به درگاه بانکی هدایت کنید. درغیر این صورت متن خطا در بخش

`catch` 

در دسترس خواهد بود. ممکن است متن این خطا فارسی باشد، پس قبل از نمایش آن مطمئن شوید که صفحه شما از حروف فارسی پشتیبانی میکند تا در نمایش خطا به کاربر مشکلی نداشته باشید.

# قدم سوم - بررسی وضعیت تراکنش

## متد verify

با استفاده از این متد میتوانید وضعیت تراکنش را بررسی کنید. به یاد داشته باشید که این متد
حتما باید در درخواست پُست فراخوانی شود.

تنها پارامتر ورودی متد وریفای، آبجکتی که مقادیر پسُت را شامل میشود است که ممکن است در فریمورک های مختلف متفاوت باشد.
برای مثال در فریمورک اکسپرس و هپی بصورت زیر میتوان به مقادیر پست صفحه دسترسی داشت:

</div>

```js
// Express.js
console.log(request.body.var_name);
// Hapi.js
console.log(request.payload.var_name);
```

<div dir="rtl">

حال شما میبایست با توجه به فریمورک مورد استفاده خود، آبجکتی که شامل مقادیر پست هست را به این متد پاس دهید:

</div>

```js
app.post('/verify', (req, res) => {
    // Pass POST Data Payload (Request Body) to verify transaction
    gateway.verify(req.body)
        .then(data => res.end('Payment was successful.'))
        .catch(error => res.end("<head><meta charset='utf8'></head>" + error));
});
```

<div dir="rtl">

در صورتی که پرداخت موفقیت آمیز بوده باشد، یک آبجکت شامل مقادیر زیر در بخش
`then`
 در دسترس خواهد بود:

|                             توضیحات                            |      کلید     |
|:--------------------------------------------------------------:|:-------------:|
| شماره فاکتوری که در مرحله دوم و متد سند برای درگاه ارسال کردید |  factorNumber |
|                     آی دی یا شماره ی تراکنش                    | transactionId |
|                      مبلغ تراکنش انجام شده                     |     amount    |
|            شماره کارتی که پرداخت با آن انجام شده است           |   cardNumber  |

و در صورتی که پرداخت به درستی انجام نشده باشد توضیحات خطا در
`catch`
قابل دسترس خواهد بود.

# تست درگاه

در صورتی که ای پی آی ورودی کلاس را
`test`
وارد کنید، میتوانید از امکان تست درگاه پی.آی آر استفاده کنید.

# نمونه کد

به نمونه کد استفاده از پکیج احتیاج دارید؟ فایل `سمپل.جی اس` را بررسی کنید.

# مجوز استفاده

این پکیج با رعایت بند های مجوز آپاچی 2.0 قابل استفاده و توسعه میباشد.
