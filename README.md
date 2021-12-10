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
## ایجاد کنترلر با هفت متد 
index create store edite update destroy
```
php artisan make:controller AdminController --resource
```
### دریافت تمام مسیر های تعریف شده 
```php
 php artisan route:list
```
### ساخت ادرس های هفت متد برای کنترلر دسته بندی
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
## Single action Controller <br>
این نوع کنترلر فقط وظیفه ی انجام یک کار را دارد برای مثال نمایش عکس پروفایل کاربران برای ساخت این نوع کنترلر فقط کافیست خط زیر را در ترمینال اجرا کنیم
```php
 php artisan make:controller ShowProfile --invokable
```

## Redirect in Laravel
```php
// 1
 Route::get('redirect-with-helper' , function(){
   return redirect()->to('category/create');
 });
// 2
 Route::get('redirect-with-helper-shortcut' , function(){
   return redirect('category');
 });
//3
 Route::get('redirect-with-facade' , function(){
   return Redirect::to('category');
 });
//4
Route::redirect('redirect-by-route' , 'category');
```

ارسال متغیر همراه ریدایرکت دقت شود که در متد 
route 
باید از اسم مسیر ها استفاده کنیم نه ادرس ان ها
```php
 Route::get('redirect', function()
 {
   return redirect()->route('category.create',['id' => 1]);
 });
```
ریدارکت بک در لاراول
```php
 Route::get('redirect', function()
 {
    return redirect()->back();
 });
```

ریدایرکت کردن به یک کنترلر با متد مشخص با استفاده از
redirect()->action()
```php
 Route::get('redirect', function()
 {
   return redirect()->action('CategoryController@create');
 });
```

ست کردن سشن هنگام ریدابرکت
```php
Route::get("/category", function (){
    return redirect('/category/create')->with('status',"Enable");
});
```
گرفتن مقدار سشن ثبت شده:
```php
session("status");
```


ریدایرکت به صفحه ی خارج سایت
```php
Route::get("/category", function (){
    return redirect('/')->away("google.com");
});
```

## HTTP Response
نمایش صفحات ارور در بخش ویو <br>
You may publish Laravel's default error page templates using the vendor:publish Artisan command. Once the templates have been published,
you may customize them to your liking:
```
php artisan vendor:publish --tag=laravel-errors
```

The abort function throws an HTTP exception which will be rendered by the exception handler:
```php
abort(403);
```
You may also provide the exception's message and custom HTTP response headers that should be sent to the browser:
```php
abort(403, 'Unauthorized.', $headers);
```
abort_if() <br>
The abort_if function throws an HTTP exception if a given boolean expression evaluates to true:
```php
abort_if(! Auth::user()->isAdmin(), 403);
```
Like the abort method, you may also provide the exception's response text as the third argument and an array of custom response headers as the fourth argument to the function.<br>

abort_unless():<br>
The abort_unless function throws an HTTP exception if a given boolean expression evaluates to false:
```php
abort_unless(Auth::user()->isAdmin(), 403);
```
# Response
In addition to returning strings from your routes and controllers, you may also return arrays. The framework will automatically convert the array into a JSON response:
```php
Route::get('/', function () {
    return [1, 2, 3];
});
```

Returning a full Response instance allows you to customize the response's HTTP status code and headers. A Response instance inherits from the Symfony\Component\HttpFoundation\Response class, which provides a variety of methods for building HTTP responses:
```php
Route::get('/home', function () {
    return response('Hello World', 200)
                  ->header('Content-Type', 'text/plain');
});
```
Keep in mind that most response methods are chainable, allowing for the fluent construction of response instances. For example, you may use the header method to add a series of headers to the response before sending it back to the user:

```php
return response($content)
            ->header('Content-Type', $type)
            ->header('X-Header-One', 'Header Value')
            ->header('X-Header-Two', 'Header Value');
```
Or, you may use the withHeaders method to specify an array of headers to be added to the response:
```php
return response($content)
            ->withHeaders([
                'Content-Type' => $type,
                'X-Header-One' => 'Header Value',
                'X-Header-Two' => 'Header Value',
            ]);
```
## Attaching Cookies To Responses
You may attach a cookie to an outgoing Illuminate\Http\Response instance using the cookie method. You should pass the name, value, and the number of minutes the cookie should be considered valid to this method:
```php
return response('Hello World')->cookie(
    'name', 'value', $minutes
);
```

```php
Cookie::queue('name', 'value', $minutes);
```
If you would like to generate a Symfony\Component\HttpFoundation\Cookie instance that can be attached to a response instance at a later time, you may use the global cookie helper. This cookie will not be sent back to the client unless it is attached to a response instance:
```php
$cookie = cookie('name', 'value', $minutes);

return response('Hello World')->cookie($cookie);
```

## Expiring Cookies Early
You may remove a cookie by expiring it via the withoutCookie method of an outgoing response:
```php
return response('Hello World')->withoutCookie('name');
```
If you do not yet have an instance of the outgoing response, you may use the Cookie facade's expire method to expire a cookie:
```php
Cookie::expire('name');
```
## Cookies & Encryption
By default, all cookies generated by Laravel are encrypted and signed so that they can't be modified or read by the client. If you would like to disable encryption for a subset of cookies generated by your application, you may use the $except property of the App\Http\Middleware\EncryptCookies middleware, which is located in the app/Http/Middleware directory:
```php
protected $except = [
    'cookie_name',
];
```
## View Responses
If you need control over the response's status and headers but also need to return a view as the response's content, you should use the view method:
```php
return response()
            ->view('hello', $data, 200)
            ->header('Content-Type', $type);
```
## JSON Responses
The json method will automatically set the Content-Type header to application/json, as well as convert the given array to JSON using the json_encode PHP function:
```php
return response()->json([
    'name' => 'Abigail',
    'state' => 'CA',
]);
```

## File Downloads
The download method may be used to generate a response that forces the user's browser to download the file at the given path. The download method accepts a filename as the second argument to the method, which will determine the filename that is seen by the user downloading the file. Finally, you may pass an array of HTTP headers as the third argument to the method:
```php
return response()->download($pathToFile);

return response()->download($pathToFile, $name, $headers);
```
## File Responses
The file method may be used to display a file, such as an image or PDF, directly in the user's browser instead of initiating a download. This method accepts the path to the file as its first argument and an array of headers as its second argument:
```php
return response()->file($pathToFile);

return response()->file($pathToFile, $headers);
```
## Service provider
برای ابجاد یک 
provider 
جدید باید دستور زیر را در ترمینال وارد کنیم
```php
php artisan make:provider TestProvider
```
برای اضافه کردن یک 
provider 
جدید به سیستم لاراول باید اون رو در مسیر
config/App.php
و در قسمت 
providers 
به صورت زیر اضافه کنیم 
```
App\Providers\TestProvider::class
```

برای اینکه یک متغیر را به چند صفحه بفرستیم باید از 
provider 
استفاده کنیم 
برای اینکار سه روش وجود دارد که باید در کلاس 
provider 
در متد 
boot
بخ صورت زیر اضافه کنیم 
```php 
View::share("count" , 5);
```
که در این روش باید 
facade
را به پروژه اضافه کنیم برای این کار فقط کافیست بنویسیم
```php
use Illuminate\Support\Facades\View;
```
روش دوم 
:
در این روش میتوانیم صفحاتی که متغیر را می فرستیم محذوذ کنیم 
```php
view()->composer(["app.category", "app.posts"] , funciton($view){
    $view->with("count" , 5);
})
```

روش سوم استفاده از کلاس هاست برای این کار باید در مسیر
App/Http
یک پوشه به نام 
View 
و درون پوشه  ی
View
پوشه ای دیگر به نام 
Composers 
ایجاد کنیم سپس درین پوشه کلاس های خود را ایجاد می کنیم برای مثال یک فایل به اسم 
TestComposer.php 
ایجاد می کنیم 
```php

namespace App\Http\View\Composers;

use Illuminate\Contracts\View\View;

class TestComposer{
    public function compose(View $view){
        $view->with("count",25);
    }
}
```
و مقادیر بالا را می نویسیم 
حالا در فایل 
TestProvider
و متد 
boot
فقط کافی است بنویسیم
```php
view()->composer(["app.category", "app.posts"] ,App\Http\View\Composers\TestComposer::class });
```




