
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
