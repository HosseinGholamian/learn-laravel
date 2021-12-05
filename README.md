# دوره ی سریع لاراول

# Routes

### HTTP Verbs

get  ->  هر وقت بخوایم یک صفحه ای رو به کاربر نشون بدیم از این متد استفاده می کنیم
<br />
post -> هر وقت بخوایم یک ردیف به دیتا بیس اضافه کنیم از این متد استفاده می کنیم 
<br />
put  -> هر وقت بخوایم یک ردیف از دیتابیس رو اپدیت کنیم از این متد استفاده می کنیم 
<br />
delete -> هر وقت بخوایم یک ردیف از دیتا بیس رو پاک کنیم از این متد استفاده می کنیم
<br />
match -> وقتی استفاده میشه که میخوایم هم زمان از دو عملیات یا از دو متد استفاده کنیم 
<br />
any -> اگر یک مسیر داشته باشیم که بخوایم همه ی متد هارو قبول کنه
<br />
برای مثال :
```php 
Route::match(['get', 'post'], '/', function () {
    //
});

Route::any('/', function () {
    //
});
```
### Methods in  controller

index -> HTTP Verb : get -> نشان دادن صفحه ی اول که کل اطلاعات در اون صفحه قرار داره مثلا صفحه ی نمایش تمام دسته بندی ها
<br />
show($id) -> HTTP Verb : get -> وظیفه ی این متد نشان دادن اطلاعات یک دسته بندی مخصوص است مثلا دسته بندی با ایدی 1
<br />
create() -> HTTP Verb : get -> نشان دادن صفحه ی مخصوص  اضافه کردن یک ردیف 
<br />
store() -> HTTP Verb : post -> انجام عملیات اضافه کردن یک ردیف جدید 
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
گروه بندی مسیر ها:
```php 
 Route::group(function () {
     Route::get('/', function () {
                 return 'dashboard';
            });
      Route::get('menu', function () {
                 return 'dashboard/menu';
            });
 });
```
کاربرد گروه بندی مسیر ها:
<br />
1- middleware
2-prefix

1- middleware
<br />
در مثال زیر فقط کاربرانی میتونن به ادرس های گروپ شده برن که احراز هویت شده باشن
```php
Route::middleware('auth')->group(function () {
     Route::get('dashboard', function () {
         return view('dashboard');
     });
     Route::get('account', function () {
         return view('account');
     });
 });
```
2-prefix
<br />
بعضی از ادرس ها پیشوند های یکسانی دارند برای مثال 
<br />
/admin/category
 و 
 <br />
 /admin/posts
 <br />
در این صورت استفاده از گروه ها مفید می باشد
<br />
استفاده از پیشوند ها دو روش دارد
<br />
روش اول:
```php
 Route::group(['prefix' => 'admin'], function () {
         Route::get('/', function () {
                 return 'dashboard';
             });
         Route::get('menu', function () {
                     return 'dashboard/menu';
                });
 });
```
روش دوم:
```php
 Route::prefix('admin')->group(function () {
     Route::get('/', function () {
         return 'dashboard';
     });
     Route::get('menu', function () {
         return 'dashboard/menu';
     });
 });
```
استفاده از ساب دامین در سیستم روتینگ 
```php 
Route::domain('test.myapp.com')->group(function (){
     Route::get("/" , function(){
         return "hi";
     });
 });
 
 Route::domain('{account}.myapp.com')->group(function () {
     Route::get('user/{id}', function ($account, $id) {
     });
 });
```
استفاده از پیشوند برای نام های چند مسیر
```php
Route::name('admin.')->group(function () {
    Route::get('/users', function () {
        // Route assigned name "admin.users"...
    })->name('users');
});
```
ویرایش صفحه ی 404 با استفاده از متد 
fallback :
این متد زمانی اجرا میشه که ادرسی که کاربر وارد کرده وجود نداشته باشه این متد باید اخرین متد یا اخرین خطی باشه که توی سیستم روتینگ می نویسیم
```php
Route::fallback(function () {
    //
});
```
##ارسلا متغیر به یک 
view

```php
Route::get('/home', function(){
     //return view('welcome')->with('var' , 'test');
     //return view('welcome',['var'=>'test']);
    $var = 'test';
    return view('welcome', compact('var'));
});
```


## برای ساخت کنترلر جدید دستور زیر را در ترمینال می زنیم
```php
php artisan make:controller HomeController
```
##ایجاد کنترلر با هفت متد 
index create store edite update destroy
```
php artisan make:controller AdminController --resource
```
###دریافت تمام مسیر های تعریف شده 
```php
 php artisan route:list
```
###ساخت ادرس های هفت متد برای کنترلر دسته بندی
```php
 Route::get('/category' , 'CategoryController@index');
 Route::get('/category/create' , 'CategoryController@create');
 Route::post('/category' , 'CategoryController@store');
 Route::get('/category/show/{id}' , 'CategoryController@show');
 Route::get('/category/edit/{id}' , 'CategoryController@edit');
 Route::put('/category/update/{id}' , 'CategoryController@update');
 Route::delete('/category/destroy/{id}' , 'CategoryController@destroy');
```

بجای نوشتن این هفت ادرس فقط با کد زیر میتونیم تمام مسیر های بالا را تعریف کنیم
```php
Route::resource('/category', 'CategoryController');
```
ساخت همان مسیر ها برای 
api
 تفاوت در این است که در 
 api 
 دیگر متد های
 create , edit
 را نداریم
 ```php
 Route::apiResource('/category', "CategoryController");

 ```
##Single action Controller <br>
این نوع کنترلر فقط وظیفه ی انجام یک کار را دارد برای مثال نمایش عکس پروفایل کاربران برای ساخت این نوع کنترلر فقط کافیست خط زیر را در ترمینال اجرا کنیم
```php
 php artisan make:controller ShowProfile --invokable
```
