
## ساختن میدلویر
```
php artisan make:middleware CheckUserStatus
```

```php
class CheckUserStatus
{
    public function handle(Request $request, Closure $next)
    {
        if (auth()->user()->status != 'active') {
            return 'Please Activate Your Account';
        }
        return $next($request);
    }
}
```
## اضافه کردن میدلویر به روت 
```php
Route::get('/', [HomeController::class, 'index'])->middleware("terminate");
```
## اضافه کردن میدلویر به گروهی از روت ها
```
Route::middleware('terminate')->group(function () {
    Route::get('/', function () {
        //
    });

    Route::get('/profile', function () {
        //
    })->withoutMiddleware('terminate');
});
```
### رجیستر کردن میدلویر ها

برای رجیستر‌کردن میدلور‌ها باید از کلاس App\Http\Kernel استفاده کنیم و این کار را به دو صورت گلوبال (global) و اختصاص برای روت‌ها می‌توان انجام داد
.
#### اگر می‌خواهید میدلوری که ساختید بر‌روی همه ریکوئست‌ها لحاظ شوند، می‌توانید کلاس میدلور خودتان را در پراپرتی middleware در کلاس Kernel ذکر شده قرار دهید
#### برای اختصاص میدلویر به روت ها باید ان را به پراپرتی routeMiddleware در کلاس karnel اضافه کرد که باید برای ان یک key برای اختصاص داردن به مسیر مورد نظر نیز بسازیم
```
protected $routeMiddleware = [
    'terminate' => \App\Http\Middleware\TerminateMiddleware::class,
    // ...
];
```
```
Route::get('/', [HomeController::class, 'index'])->middleware(['terminate', 'anotherMiddleWare']);
```

## گروه‌بندی میدلور‌ها
در بعضی اوقات نیاز است برای راحتی اختصاص چند میدلور به یک روت خاص آن‌ها را گروه‌بندی کنید و به روت مدنظرتان اختصاص دهید، برای این کار نیاز است تا شما آن میدلور‌ها را در پراپرتی middlewareGroups قرار دهید
```
protected $middlewareGroups = [
    'web' => [
        \App\Http\Middleware\EncryptCookies::class,
        \Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse::class,
        \Illuminate\Session\Middleware\StartSession::class,
        // \Illuminate\Session\Middleware\AuthenticateSession::class,
        \Illuminate\View\Middleware\ShareErrorsFromSession::class,
        \App\Http\Middleware\VerifyCsrfToken::class,
        \Illuminate\Routing\Middleware\SubstituteBindings::class,
    ],

    'api' => [
        'throttle:api',
        \Illuminate\Routing\Middleware\SubstituteBindings::class,
    ],
];
```
اگر نگاهی به متد boot در کلاس App\Providers\RouteServiceProvider بیندازید، متوجه می‌شوید که به‌صورت پیش‌فرض گروه‌های میدلور api و web به ترتیب روی روت‌های موجود در فایل‌های routes/api.php و routes/web.php اعمال می‌شوند و نیازی به اختصاص دادن مجدد آن‌ها نیست


## میدلور ها می توانند قبل از پردازش رکوئست یا بعد از پردازش رکوئست اجرا شوند برای مثال
```php
class BeforeMiddleware
{
    public function handle($request, Closure $next)
    {
        // قبل از پردازش ریکوئست
        return $next($request);
    }
}
```
```php
class AfterMiddleware
{
    public function handle($request, Closure $next)
    {
        $response = $next($request);
        // بعد از پردازش ریکوئست و قبل از ارسال ریسپانس
        return $response;
    }
}
```

## اگر بخواهیم میدلویری را بعد از ارسال پاسخ به کاربر انجام دهیم از متد terminate استفاده می کنیم 
```
 public function terminate($request, $response)
    {
        // ...
    }
```
