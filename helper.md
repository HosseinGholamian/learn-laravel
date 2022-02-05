# Arr:has:
چک می‌کند که آیا مقدار ورودی در آرایه هست یا نه:
```php
$array = ['post' => ['id' => 1, 'title' => 'Quera']];
$contains = Arr::has($array, ['post.id', 'post.description']);
// false
```

# Arr:plunk:
همه مقادیر برای کلید ورودی در آرایه را برمی‌گرداند:
```php
$array = [
    ['developer' => ['id' => 1, 'name' => 'ali']],
    ['developer' => ['id' => 2, 'name' => 'hassan']],
    ['developer' => ['id' => 3, 'name' => 'hossein']]
];
$names = Arr::pluck($array, 'developer.name');
// ['ali', 'hassan', 'hossein']

```
# Str:contains
 این متد مشخص می‌کند که آیا رشته دلخواه شامل مقدار ورودی هست یا نه:
 ```php
 $contains = Str::contains('This is my name', ['my', 'foo']);
// true
 ```
 در این مثال اگر رشته شامل هرکدام از مقادیر آرایه باشد مقدار true برگشت داده خواهد شد. اگر بخواهیم بفهمیم رشته شامل همه موارد داخل آرایه باشد باید از متد Str::containsAll استفاده نماییم.
 # optional
 این تابع یک شی را به عنوان آرگومان دریافت کرده و این اجازه را می‌دهد که به ویژگی‌های آن دسترسی داشت و یا متد‌های آن شی را صدا زد. اگر شی ورودی null باشد، دسترسی به ویژگی‌ها و متد‌های آن به جای ارور null بر‌می‌گرداند:
```php
$id = optional($request->user())->id;
```
هم‌چنین، می‌توان یک Closure را به‌عنوان آرگومان دوم به این تابع پاس داد. اگر مقدار آرگومان اول null نباشد، خروجی Closure به‌عنوان خروجی تابع optional برمی‌گردد:
```php
$id = optional(User::find($id), function ($user) {
    return $user->id;
});
```
# config
این تابع، مقدار یکی از کلیدهای موجود در فایل‌های پوشه‌ی config را برمی‌گرداند. مثلاً اگر فایل config/app.php را در نظر بگیریم که بخشی از محتوای آن به‌صورت زیر است:

```php
return [
    'name' => env('APP_NAME', 'Laravel'),
];
```

در این‌صورت، می‌توان با استفاده از config('app.name') به مقدار کلید name موجود در این فایل دسترسی داشت.

# response
ز این تابع برای ارسال پاسخ به کاربر استفاده می‌شود.
```php
return response('Hello World', 200, $headers);
```
