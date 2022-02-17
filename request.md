
## متد های کلاس ریکوئست 
#### متدpath

این متد، مسیر هدف درخواست را برمی‌گرداند. برای مثال اگر آدرس هدف 127.0.0.1/admin/panel باشد، این متد admin/panel را بر‌می‌گرداند:

```php
Route::get('/admin/panel', function (Request $request) {
    return $request->path();
});

// admin/panel
```
آیا کاربر در مرورگر این آدرس را وارد کرده است یا خیر:
```php
Route::get('/admin/panel', function (Request $request) {
    if ($request->is('admin/*')) {
        return 'Admin Page!';
    }
    return 'User Page!';
});

// Admin Page!
```

با متد routeIs روت فعلی درخواست را چک کرد:
```php
Route::get('/admin/panel', function (Request $request) {
    if ($request->routeIs('admin.panel')) {
        return 'Admin Page!';
    }
    return 'User Page!';
})->name('admin.panel');

// Admin Page!
```

متدهای url و fullUrl: متد url آدرس URL هدف را بدون Query String و متد fullUrl آدرس URL هدف را با Query String (درصورت وجود) برمی‌گرداند:

```php
Route::get('/admin/panel', function (Request $request) {
    return $request->url();
});

// localhost/admin/panel

Route::get('/admin/panel', function (Request $request) {
    return $request->fullUrl();
});

// localhost:8000/admin/panel?active=true
```


متد method: این متد HTTP Verb مربوط به ریکوئست را برمی‌گرداند:

```php
Route::any('/admin/panel', function (Request $request) {
    if ($request->isMethod('post')) {
        return 'Not Allowed';
    }
    return $request->method();
});

// GET
```

متد ip: این متد، آی‌پی کاربر را برمی‌گرداند:
```php
Route::get('/admin/panel', function (Request $request) {
    return $request->ip();
});

// User IP
```

تد header: از این متد برای دریافت مقدار یک هدر خاص از ریکوئست استفاده می‌شود:
```php
Route::get('/admin/panel', function (Request $request) {
    if ($request->hasHeader('HEADER-NAME')) {
        return 'Header Was Found';
    }
    return $request->header('ANOTHER-HEADER-NAME', 'default');
});

// default
```

متد cookie: از این متد برای دسترسی به مقدار یک cookie از ریکوئست استفاده می‌شود:
```php
$nameCookie = $request->cookie('name');
```

