
# تعریف روت ها به دو روش:
```php
use App\Http\Controllers\HomeController;

Route::get('/', [HomeController::class, 'index']);
// یا
Route::get('/', 'HomeController@index');
```
# مشاهده ی لیست تمامی روت ها:
```shell
php artisan route:list
```
# match & any
گاهی لازم است برای یک آدرس به دو متد پاسخ داده شود. در این حالت ازmatch استفاده می‌شود:
```php
Route::match(['get', 'post'], '/', [HomeController::class, 'index']);

```
گاهی نیز لازم است برای یک آدرس به همه متد‌ها پاسخ داده شود. در این حالت باید از متد any به‌صورت زیر استفاده شود:
```php
Route::any('/', [HomeController::class, 'index']);

```

# پارامتر اختیاری
```php
Route::get('/posts/{post?}', function ($post = 'default') {
    return $post;
});
```
# شرط برای پارامتر ها :
```php
Route::get('/posts/{slug}', function ($slug) {
    //
})->where('slug', '[A-Za-z\-]+');
```

## اعمال چند شرط به یک پارامتر:
```php
Route::get('/user/{id}/{name}', function ($id, $name) {
    //
})->where(['id' => '[0-9]+', 'name' => '[a-z]+']);
```
## اعمال محدودیت برای همه ی مسیر ها:
برای محدودیت سراسری یعنی محدودیتی که روی همه پارامتر‌ها با نام مشخص اعمال شوند، باید محدودیت مورد نظر را به‌صورت زیر در متد boot در فایل App\Providers\RouteServiceProvider نوشت:

```php
public function boot()
{
    Route::pattern('id', '[0-9]+');
}
```
# نکته:
مقدار یک پارامتر، همه کاراکتر‌ها به غیر از / را می‌پذیرد. اگر می‌خواهید در پارامتر مسیری که تعریف می‌کنید مقدار / پذیرفته شود باید محدودیت زیر را اعمال کنید:
```php
Route::get('/search/{search}', function ($search) {
    return $search;
})->where('search', '.*');
```

# نامگذاری روت ها:
```php
Route::get('/', [HomeController::class, 'index'])->name('home');
```
استفاده از هلپر route و redirect با استفاده از نام مسیر ها:
```php
// Generating URLs...
$url = route('home');
// Generating Redirects...
return redirect()->route('home');
```
# تشخیص مسیر فعلی:
```php
if ($request->route()->named('home')) {
        //
    }
```

# گروه بندی روت ها :
```php 
Route::group([], function () {

    Route::get('/', function () {
        //
    });

    Route::get('/about-us', function () {
        //
    });

});
```

# استفاده از پیشوند در روت ها:
```php
Route::prefix('admin')->name('admin.')->group(function () {
    Route::get('/posts', function () {
        // Matches The "/admin/posts" URL that Have admin.posts Name
    })->name('posts');
    Route::get('/users', function () {
        // Matches The "/admin/users" URL that Have admin.users Name
    })->name('users');
    Route::get('/categories', function () {
        // Matches The "/admin/categories" URL that Have admin.categories Name
    })->name('categories');
});
```
# استفاده از subdomain 
```php
Route::domain('support.localhost')->group(function () {
    Route::get('/', function () {

    });
});
```


