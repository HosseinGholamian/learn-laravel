# learn-laravel

# Routes

### HTTP Verbs

get  ->  هر وقت بخوایم یک صفحه ای رو به کاربر نشون بدیم از این متد استفاده می کنیم
<br />
post -> هر وقت بخوایم یک ردیف به دیتا بیس اضافه کنیم از این متد استفاده می کنیم 
<br />
put  -> هر وقت بخوایم یک ردیف از دیتابیس رو اپدیت کنیم از این متد استفاده می کنیم 
<br />
delete -> هر وقت بخوایم یک ردیف از دیتا بیس رو پاک کنیم از این متد استفاده می کنیم

### Methods in  controller

index -> HTTP Verb : get -> نشان دادن صفحه ی اول که کل اطلاعات در اون صفحه قرار داره مثلا صفحه ی نمایش تمام دسته بندی ها
<br />
show($id) -> HTTP Verb : get -> وظیفه ی این متد نشان دادن اطلاعات یک دسته بندی مخصوص است مثلا دسته بندی با ایدی 1
<br />
create() -> HTTP Verb : get -> نشان دادن صفحه ی مخصوص  اضافه کردن یک ردیف 
<br />
store($id) -> HTTP Verb : post -> انجام عملیات اضافه کردن یک ردیف جدید 
<br />
edit($id) -> HTTP Verb : get -> نشان دادن صفحه ی مخصوص ادیت کردن 
<br />
update($id) -> HTTP Verb : put -> انجام عملیات اپدایت کردن 
<br />
destroy($id)-> HTTP Verb : delete -> عملیات حذف یک ردیف در دیتا بیس


### routes
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

برای اینکه مسیر با کنترلر ارتباط برقرار کنه مسیر رو به شکل زیر تعریف میکنیم:
```php
Route::get('Domain', 'ControllerName@methodName')->name('name');
```
 که ارگمان اول مسیر مورد نظرمون هست ارگمان دوم نام کنترلر و متد مورد نظر است که با علامت 
 @ 
 از هم جدا شده
در مثال زیر کاربر هر وقت ادرس 
members/1
رو بزنه متد 
show
در کلاس یا کنترلر 
MembersController
اجرا میشه

```php
Route::get('/members/{id}', 'MembersController@show')->name('members.show');
```
### route helper
این فانکشن وقتی استفاده میشه که میخوایم یک لینک درست کنیم به عنوان مثال
<br />
که ارگمان اول اسمی هست که برای مسیر تعریف کردیم
و ارگمان دوم مقدار دهی متغیر ها در یک ارایه است

```php 
route('members.show',['id' => 1, 'opt' =>'a'])
```
که خروجی مثال بالا به شکل زیره:
```php
 http://127.0.0.1:8000/members/1?opt=a 
```



