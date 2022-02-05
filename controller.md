
# ساخت کنترلر
```
php artisan make:controller HomeController
php artisan make:controller HomeController --resource  //with 7 method
```

```php
Route::resource('posts', PostController::class);  //make route for seven method

Route::resource('users', UserController::class)->only([
    'index', 'show'
]);

Route::resource('categories', CategoryController::class)->except([
    'show'
]);
```
همان‌طور که در کنترلر بالا هم مشاهده کردید، می‌توان در یک کنترلر چندین متد برای پاسخ به ریکوئست‌های مختلف قرار داد اما گاهی اوقات لازم است کنترلری را فقط برای مدیریت یک درخواست ایجاد کرد. چنین کنترلری را می‌توان با دستور Artisan زیر ساخت: ‍‍‍
```
php artisan make:controller TestController --invokable

```
برای تعریف روت متناسب با این نوع کنترلر‌ها نیازی به مشخص‌کردن متد برای آن‌ها نیست، زیرا به‌طور پیشفرض متد invoke__ در نظر گرفته خواهد شد:
```
Route::get('/test', TestController::class);

```
