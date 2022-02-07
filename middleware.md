
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
