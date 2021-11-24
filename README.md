# learn-laravel

# Routes
مسیر های پروژه در پوشه ی 
routes/web.php
تعریف می شه 

نحوه ی تعریف یک مسیر جدید به شکل زیره :
```php
 Route::get('/users', function () {
     return "hi";
});
```

برای تعریف متغیر توی مسیر اونو توی اکولاد قرار میدیم مثلا وقتی میخوایم اطلاعات یک یوزر مشخص رو نمایش بدیم به شکل زیر:
```php
Route::get('/users/{id}', function ($id) {
//     return $id;
// });
```
اگر بخوایم بگیم که این متغیر توی روت اپشنال هست باید بعد از اسم متغیر یک علامت سوال بزاریم به صورت زیر
```php 

Route::get('/{id?}', function ($id=1) {
//     return $id;
// });

```

اگر بخوایم برای متغیرمون با استفاده از رجکس شرط بزاریم کافیه به شکل زیر عمل کنیم مثلا در مثال زیر ایدی فقط میتونه عددی بین 0 تا 9 باشه
```php
Route::get('/{id}', function ($id) {
//     return $id;
// })->where('id','[0-9]');
```

برای اینکه مسیر با کنترل ارتباط برقرار کنه مسیر رو به شکل زیر تعریف میکنیم:
```php
Route::get('Domain', 'ControllerName@methodName')->name('name');
```
که ارگمان اول مسیر مورد نظرمون هست ارگمان دوم نام کنترل و متد مورد نظر است 
در مثال زیر کاربر هر وقت ادرس 
members/1
رو بزنه متد 
show
در کلاس یا کنترل 
MembersController
اجرا میشه

```php
Route::get('/members/{id}', 'MembersController@show')->name('members.show');
```
