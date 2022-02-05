# نصب پکیج nwidart/laravel-modules
```php
composer require nwidart/laravel-modules
php artisan vendor:publish --provider="Nwidart\Modules\LaravelModulesServiceProvider"
```



# اضافه کردن پکیج به composer.json
```php
{
  "autoload": {
    "psr-4": {
      "App\\": "app/",
      "Modules\\": "Modules/"
    }
  }
}
```

# در نهایت، باید دستور زیر را اجرا کرد تا autoloader بر اساس محتویات جدید فایل composer.json ساخته شود:

```php
composer dump-autoload
```

# اخت ماژول جدید
```php
php artisan module:make Blog

```
# ساخت کنترلر در ماژول
```php
php artisan module:make-controller PostsController Blog
```

