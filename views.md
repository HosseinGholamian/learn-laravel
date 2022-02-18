
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
### view Compsoer

ویو کامپوزرها (view composers) توابع یا کلاس‌هایی هستند که هنگام رندر (render) شدن ویوها فراخوانی شده و اجرا می‌شوند. کاربرد ویو کامپوزرها در مواردی است که می‌خواهیم یک داده را هر بار که یک ویو رندر می‌شود به آن پاس دهیم. برای مثال، هنگامی که یک ویو در چند روت یا متد استفاده می‌شود و هر بار به یک مقدار مشخص نیاز دارد، می‌توان آن مقدار را در ویو کامپوزر به ویو پاس داد.

ویو کامپوزرها باید در یک service provider رجیستر شوند. مثالی از هر دو حالت تابع و کلاس را در کد زیر می‌بینید:
```php
<?php

namespace App\Providers;

use Illuminate\Support\Facades\View;

class AppServiceProvider extends ServiceProvider
{
    public function boot()
    {
        // Class
        View::composer('menu', MenuComposer::class);

        // Function
        View::composer('menu', function ($view) {
            //
        });
    }
}
```
پیاده‌سازی
در ادامه طی یک مثال، یک ویو کامپوزر برای منوی یک سایت خواهیم ساخت. ابتدا پوشه‌ی app/Http/View/Composers را می‌سازیم تا تمام ویو کامپوزرها را در آن قرار دهیم. سپس، کلاس MenuComposer را در این پوشه ایجاد کرده و محتوای زیر را در آن قرار می‌دهیم:
```php
namespace App\Http\View\Composers;

use App\Models\MenuItems;

class MenuComposer
{
    public function compose($view)
    {
        $view->with('menu', MenuItems::all());
    }
}
```
سپس، این ویو کامپوزر را در AppServiceProvider رجیستر می‌کنیم:
```php
namespace App\Providers;

use App\Http\View\Composers\MenuComposer;
use App\Models\MenuItems;
use Illuminate\Support\Facades\View;
use Illuminate\Support\ServiceProvider;

class ComposerServiceProvider extends ServiceProvider
{
    public function boot()
    {
        View::composer('menu', MenuComposer::class);
    }
}
```
نکته: بهتر است برای ویو کامپوزرها یک service provider جدا ساخته شود.

پاس دادن داده به چند ویو
هنگام رجیستر کردن ویو بکامپوزرها می‌توان یک ویو کامپوزر را به چند ویو متصل کرد. برای این کار می‌توان نام ویوها را در یک آرایه قرار داد:
```php
View::composer(['index', 'show'], MenuComposer::class);
```
و یا از wildcard استفاده کرد:
```php
View::composer('layout.*', MenuComposer::class);
```
و یا
```php
View::composer('*', MenuComposer::class);
```
