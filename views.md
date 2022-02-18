
پاس دادن یک مقدار به تمام ویوها
گاهی ممکن است نیاز باشد یک مقدار (برای مثال کاربر) را به تمام ویوها پاس دهید. برای این کار از تابع share از Facade view استفاده می‌کنیم. به این صورت که این تابع را در تابع boot یک Service Provider قرار می‌دهیم. برای مثال:
```php
<?php

namespace App\Providers;

use Illuminate\Support\Facades\View;

class AppServiceProvider extends ServiceProvider
{
    public function boot()
    {
        View::share('key', 'value');
    }
}
```
