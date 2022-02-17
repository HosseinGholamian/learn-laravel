```php
$validation = $request->validate([
    'email' => 'required',
    'mobile' => 'required|max:11',
]);

```
یا
```php
$validation = $request->validate([
    'email' => ['required'],
    'mobile' => ['required', 'max:11'],
]);
```
اگر کاربر اینپوت‌ها را به درستی وارد کرده باشد، این اعتبار‌سنجی قبول می‌شود و ادامه‌ی پردازش انجام می‌شود، اما اگر اعتبار‌سنجی با شکست رو‌به‌رو شود، کاربر به صفحه‌ی قبل (صفحه فرم) هدایت می‌شود. پیش از هدایت، مقادیر اینپوت‌ها و خطا‌ها به‌صورت خودکار در سشن ذخیره شده و در صفحه‌ی قبل قابل دسترسی خواهد بود.

لازم به ذکر است که خطا‌ها توسط میدلور \Illuminate\View\Middleware\ShareErrorsFromSession با متغیر $errors در دسترس قرار می‌گیرد. بنابراین برای نمایش خطا‌ها، می‌توان به‌ صورت زیر عمل کرد:

```php
@if ($errors->any())
    <div class="alert alert-danger">
        <ul>
            @foreach ($errors->all() as $error)
                <li>{{ $error }}</li>
            @endforeach
        </ul>
    </div>
@endif
```
با اجرای کد قبل مشاهده می‌کنید که خطاهایی با زبان انگلیسی نمایش داده می‌شود. این‌ها، خطاهای پیش‌فرض لاراول هستند که در پوشه‌ی resources/lang/en/validation.php قرار گرفته‌اند که شما به دلخواه می‌توانید آن‌ها را شخصی‌سازی کنید.

در نظر داشته باشید که می‌توان این خطاها را به زبان فارسی نیز نمایش داد. برای این کار، کافی است که مراحل زیر را انجام دهید:

فایل ترجمه‌ی فارسی خطاها را از این لینک دریافت کنید
https://quera.ir/qbox/view/YtM67CNFgK/fa.zip.
سپس مقدار locale در فایل کانفیگ app.php را برابر fa قرار دهید.
حال، شما می‌توانید خطاهای فارسی را در resources/lang/fa/validation.php مشاهده و در صورت نیاز آن‌ها را تغییر دهید

هم‌چنین شما می‌توانید با استفاده از directive @error بررسی کنید که یک اینپوت خاص دچار خطا شده است یا نه:
```php
@error('mobile')
    <div class="alert alert-danger">{{ $message }}</div>
@enderror
```
#### روش دیگر
```php
use Illuminate\Support\Facades\Validator;

$validator = Validator::make($request->all(), [
    'email' => 'required',
    'mobile' => 'required|max:11',
]);
if ($validator->fails()) {
    return redirect()
        ->back()
        ->withErrors($validator)
        ->withInput();
}
```
برخلاف روش قبل که مستقیماً از متد validate بر‌ روی ریکوئست استفاده کردیم، در این‌جا از متد make در facade Validator استفاده می‌کنیم. همان‌طور که مشاهده می‌کنید، این متد دو آرگومان دارد که اولین آرگومان داده‌هایی هستند که قرار است اعتبار‌سنجی شوند و دومین آرگومان مجموعه‌ی قواعد است.

لازم به ذکر است که در ادامه نیز چک کرده‌ایم که اگر اعتبار‌سنجی با خطا مواجه شد، کاربر به صفحه قبل برگردد. هم‌چنین، با استفاده از متد withErrors خطاها را در سشن ذخیره می‌کنیم تا در صفحه قبل قابل دسترسی باشند.

می‌توان مرحله‌ی چک کردن را مانند روش درس‌نامه‌ی قبل به‌صورت خودکار انجام داد؛ یعنی
:
```php
Validator::make($request->all(), [
    'email' => 'required',
    'mobile' => 'required|max:11',
])->validate();
```
با این کار، مراحل ذکرشده نظیر انتقال به صفحه قبل، ذخیره‌ی خطاها در سشن و هم‌چنین ذخیره‌ی اینپوت‌ها به‌صورت خودکار انجام می‌شود.
.


### ایجاد فرم ریکوئست
```
php artisan make:request SignupRequest
```
پس از اجرای این دستور، کلاس SignupRequest در پوشه app/Http/Requests ایجاد خواهد شد. این کلاس دارای دو متد با نام‌های authorize و rules است. متد authorize وظیفه‌ی بررسی این‌که آیا کاربر مجاز به انجام این ریکوئست است یا نه را بر‌عهده دارد. متد rules قواعد اعتبار‌سنجی را به‌صورت یک آرایه بر می‌گرداند:
```
public function rules()
{
    return [
        'email' => 'required',
        'mobile' => 'required|max:11',
    ];
}
```
برای استفاده از این ریکوئست، تنها کافی است به جای Request، کلاس فرم ریکوئست را Type Hint کنید:
```
use App\Http\Requests\SignupRequest;

public function store(SignupRequest $request)
{
    //
}
```
