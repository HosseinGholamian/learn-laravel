
پیکربندی filesystem در فایل config/filesystems.php قرار دارد که اگر نگاهی به این فایل بیندازیم، خواهیم دید که سه دیسک local ، public و s3 به‌صورت پیش‌فرض وجود دارند:
```php
'disks' => [
    'local' => [
        'driver' => 'local',
        'root' => storage_path('app')
    ],
    'public' => [
        'driver' => 'local',
        'root' => storage_path('app/public'),
        'url' => env('APP_URL').'/storage',
        'visibility' => 'public'
    ],
    's3' => [
        'driver' => 's3',
        'key' => env('AWS_ACCESS_KEY_ID'),
        'secret' => env('AWS_SECRET_ACCESS_KEY'),
        'region' => env('AWS_DEFAULT_REGION'),
        'bucket' => env('AWS_BUCKET'),
        'url' => env('AWS_URL'),
        'endpoint' => env('AWS_ENDPOINT')
    ]
]
```
هر دیسک بیانگر یک درایور برای اتصال به یک Storage خاص است. s3 برای اتصال به سرویس آمازون (Amazon S3)، local برای ذخیره‌سازی فایل‌ها در پوشه‌ی storage/app و public برای ذخیره‌سازی فایل‌ها در پوشه‌ی storage/app/public است.

تفاوت local و public در این است که لاراول به‌صورت پیش‌فرض از دیسک local استفاده می‌کند. این دیسک به‌ صورت خصوصی است و از طریق مرورگر قابل دسترسی نیست، در حالی که دیسک public با اضافه کردن symlink به‌ صورت‌ زیر قابل دستیابی است:
```php
php artisan storage:link
```
با استفاده از متد put می‌توان فایل زیر را در Storage ذخیره و محتوای دلخواهی برای آن قرار داد:
```php
Storage::put('test.txt', 'Text In test.txt File');
```
با استفاده از متد disk می‌توان دیسک موردنظر را انتخاب کرد (همان‌طور که گفته شد، دیسک پیش‌فرض local است):
```php
Storage::disk('public')->put('test.txt', 'Text In test.txt File');
```
از متد get برای دریافت محتویات فایل‌های Storage استفاده می‌شود:
```php
Storage::get('test.txt');
```
با استفاده از متد download می‌توان یک فایل را دانلود کرد:
```php
Storage::download('test.txt');
```

#### اپلود فایل
پس از گرفتن اینپوت فایل موردنظر، از متد store برای آپلود آن استفاده کنید
```php
$path = $request->file('image')->store('images');
```

این متد فایل را در آدرس storage/app/images ذخیره کرده، نامی منحصر‌ به‌ فرد برای فایل قرار داده و پس از ذخیره‌سازی، مسیر آن را بر می‌گرداند. هم‌چنین توجه داشته باشید که می‌توان دیسک موردنظر را به‌عنوان آرگومان دوم به این متد پاس داد.

اگر بخواهیم برای فایلی که قصد آپلود آن را داریم نام خاصی در نظر بگیریم، باید از متد storeAs استفاده کنیم که سه آرگومان مسیر، نام فایل و دیسک را به‌عنوان ورودی می‌گیرد:
```php

$path = $request->file('image')->storeAs(
    'images', 'IMAGE_NAME.png', 'public'
);
```
یا
```php

$path = Storage::disk('public')->putFileAs(
    'images', $request->file('image'), 'IMAGE_NAME.png'
);
```
