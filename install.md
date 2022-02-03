
## نصب کامپوزر <br>

برای لینوکس و مک‌اواس: دستورات زیر را به‌ترتیب در ترمینال اجرا کنید:
 ```

php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php composer-setup.php
php -r "unlink('composer-setup.php');"
mv composer.phar /usr/local/bin/composer
ShellCopy
```
پس از نصب کامپوزر، با اجرای دستور زیر در خط فرمان (ترمینال در لینوکس و مک‌اواس یا CMD در ویندوز) می‌توانید از نسخه‌ی کامپوزر خود مطلع شوید:

```
composer --version
ShellCopy
```
اگر مراحل نصب به درستی طی شده باشند، با اجرای دستور بالا، خروجی مشابه خروجی زیر نمایش داده خواهد شد:

```
Composer version 2.1.5 2021-07-23 10:35:47
```
فایل composer.json در واقع شناسنامه‌ی پکیج است. محتویات این فایل پس از نصب این پکیج به‌صورت زیر است:
```
{
    "require": {
        "nesbot/carbon": "^2.44"
    }
}
```

در بخش require، نام پکیج مشاهده می‌شود. مقدار این کلید برابر با ^2.44 است. در این‌جا 2.44 نسخه‌ی پکیج است. عدد 2 در این‌جا به معنای نسخه‌ی اصلی (major) و عدد 44 به معنای نسخه‌ی فرعی (minor) است. ممکن است نسخه‌ی یک پکیج یک بخش دیگر تحت عنوان پچ (patch) داشته باشد (نظیر 8.0.1). این سه بخش در شرایط زیر تغییر می‌کنند:

نسخه‌ی اصلی یک پکیج در صورتی افزایش می‌یابد که تغییرات عمده‌ای در پکیج اعمال شده باشد. این تغییرات ممکن است backward incompatible باشند، یعنی ممکن است برنامه‌نویس‌هایی که از نسخه‌ی قبلی پکیج استفاده می‌کردند، برای به‌روزرسانی این پکیج باید تغییراتی در کدهای خود نیز اعمال کنند.
نسخه‌ی فرعی یک پکیج در صورتی افزایش می‌یابد که تغییراتی جزئی در پکیج اعمال شده باشند. این تغییراتی معمولاً backward compatible هستند، به این معنا که برنامه‌نویس‌ها برای به‌روزرسانی پکیج نیازی به تغییر کدهای خود ندارند، یا تغییراتی که باید در کدهایشان اعمال کنند جزئی است.
نسخه‌ی پچ یک پکیج در صورتی افزایش می‌یابد که باگ‌هایی از پکیج رفع شده باشند. این تغییرات backward compatible هستند.
به نحوه‌ی نسخه‌گذاری فوق، Semantic Versioning گفته می‌شود.

عملگر ^ که قبل از نسخه‌ی پکیج Carbon آمده است، به این معنی است که هنگام نصب این پکیج، نسخه‌ی اصلی حتماً نسخه‌ی ذکرشده باشد، اما نسخه‌های فرعی و پچ صرفاً بزرگ‌تر یا مساوی نسخه‌ی ذکرشده باشند.

توجه داشته باشید که خود پکیج Carbon نیز یک فایل composer.json برای خود دارد که در مسیر vendor/nesbot/carbon قرار گرفته است. بیایید محتویات این فایل را نیز بررسی کنیم:
```
{
    "name": "nesbot/carbon",
    "type": "library",
    "description": "An API extension for DateTime that supports 281 different languages.",
    "keywords": [
        "date",
        "time",
        "DateTime"
    ],
    "homepage": "http://carbon.nesbot.com",
    "support": {
        "issues": "https://github.com/briannesbitt/Carbon/issues",
        "source": "https://github.com/briannesbitt/Carbon"
    },
    "license": "MIT",
    "authors": [
        {
            "name": "Brian Nesbitt",
            "email": "brian@nesbot.com",
            "homepage": "http://nesbot.com"
        },
        {
            "name": "kylekatarnls",
            "homepage": "http://github.com/kylekatarnls"
        }
    ],
    "prefer-stable": true,
    "minimum-stability": "dev",
    "bin": ["bin/carbon"],
    "require": {
        "php": "^7.1.8 || ^8.0",
        "ext-json": "*",
        "symfony/polyfill-mbstring": "^1.0",
        "symfony/translation": "^3.4 || ^4.0 || ^5.0"
    },
    "require-dev": {
        "doctrine/orm": "^2.7",
        "friendsofphp/php-cs-fixer": "^2.14 || ^3.0",
        "kylekatarnls/multi-tester": "^2.0",
        "phpmd/phpmd": "^2.9",
        "phpstan/extension-installer": "^1.0",
        "phpstan/phpstan": "^0.12.54",
        "phpunit/phpunit": "^7.5.20 || ^8.5.14",
        "squizlabs/php_codesniffer": "^3.4"
    },
    "autoload": {
        "psr-4": {
            "Carbon\\": "src/Carbon/"
        }
    },
    "autoload-dev": {
        "files": [
            "tests/Laravel/ServiceProvider.php"
        ],
        "psr-4": {
            "Tests\\": "tests/"
        }
    },
    "config": {
        "process-timeout": 0,
        "sort-packages": true
    },
    "scripts": {
        "test": [
            "@phpunit",
            "@style-check"
        ],
        "style-check": [
            "@phpcs",
            "@phpstan",
            "@phpmd"
        ],
        "phpunit": "phpunit --verbose",
        "phpcs": "php-cs-fixer fix -v --diff --dry-run",
        "phpstan": "phpstan analyse --configuration phpstan.neon",
        "phpmd": "phpmd src text /phpmd.xml",
        "phpdoc": "php phpdoc.php"
    },
    "extra": {
        "branch-alias": {
            "dev-master": "2.x-dev",
            "dev-3.x": "3.x-dev"
        },
        "laravel": {
            "providers": [
                "Carbon\\Laravel\\ServiceProvider"
            ]
        },
        "phpstan": {
            "includes": [
                "extension.neon"
            ]
        }
    }
}
```
در بخش name، نام پکیج نوشته شده است.
مقدار بخش type برابر با library است، به این معنا که این پکیج یک کتاب‌خانه است (در پکیج laravel/laravel این مقدار برابر با project است).
در بخش require، وابستگی‌های پکیج (dependencies) ذکر شده‌اند. اگر به مقدار ext-json در این بخش توجه کنید، خواهید دید که مقدار آن برابر با * است. این مقدار به این معناست که هر نسخه‌ای از این اکستنشن (extension) قابل قبول است. از این عملگر در نسخه‌ای نظیر 1.2.* نیز می‌توان استفاده کرد، به این معنا که نسخه‌ی اصلی باید دقیقاً 1 و نسخه‌ی فرعی باید دقیقاً 2 باشد، اما نسخه‌ی پچ می‌تواند هر عددی باشد. کامپوزر هنگام نصب چنین نسخه‌ای، آخرین نسخه‌ی پچ را دریافت می‌کند. برای کسب اطلاعات بیش‌تر درباره‌ی نسخه‌گذاری کامپوزر، این صفحه را مطالعه کنید.
در بخش require-dev، نیازمندی‌هایی ذکر شده‌اند که هنگام توسعه‌ی پکیج و دیباگ کردن (debugging) کاربرد دارند.
در بخش autoload، یک بخش تحت عنوان psr-4 وجود دارد که این بخش، یک آرایه‌ی انجمنی است. کلیدهای این آرایه، فضای نام‌های مختلفی (namespaces) هستند و مقادیر متناظر با آن‌ها، مسیر پوشه‌هایی است که این فضای نام‌ها قرار است به آن‌ها نگاشت (map) شوند. در این مثال، هرگاه از فضای نام Carbon\ استفاده کنیم، کلاس موردنظر از مسیر src/carbon/ لود می‌شود.


## دستور composer update
فرض کنید یک کد از یک برنامه‌نویس دریافت کرده‌اید که شامل فایل composer.json است. با اجرای دستور composer update، کامپوزر پکیج‌ها را بر اساس محتویات composer.json نصب می‌کند. همچنین، ممکن است نسخه‌ی جدید بعضی از پکیج‌هایی که تاکنون نصب شده‌اند منتشر شده باشد. در این‌صورت، کامپوزر این پکیج‌ها را به‌روزرسانی می‌کند.

 ## دستور composer install
علاوه بر فایل composer.json، فایلی با نام composer.lock در مسیری که Carbon نصب شده است وجود دارد. این فایل شامل نسخه‌ی دقیق پکیج‌هایی است که در حال حاضر نصب شده‌اند. برای جلوگیری از بروز مشکل backward incompatibility ، بهتر است تا پکیج‌ها با توجه به نسخه‌هایی که در فایل composer.lock ذکر شده‌اند نصب شوند. با اجرای دستور composer install، کامپوزر پکیج‌ها را بر اساس محتویات composer.lock نصب می‌کند.


## تولید فایل autoload.php
composer.json:
```
{ "autoload": {
        "psr-4": {
            "QueraCollege\\StringUtils\\": "src"
        }
    }}
    
```
  
پس از ذکر مقادیر autoload در فایل composer.json، با اجرای دستور زیر می‌توان فایل autoload.php را تولید کرد:

```
composer dump
```
دستور فوق، فایل autoload.php را در مسیر vendor ایجاد می‌کند.

هم‌اکنون می‌توانیم از پکیجی که نوشته‌ایم استفاده کنیم. می‌توانیم فایلی با نام دلخواه در مسیر پکیج ایجاد کنیم و فایل vendor/autoload.php را require کنیم:
```
<?php
require __DIR__.'/vendor/autoload.php';
use \QueraCollege\StringUtils\Str;
var_dump(Str::contains('abcd', ['ab', 'x']));
```



## نصب لاراول با استفاده از کامپوزر
با اجرای دستور زیر در ترمینال، می‌توانید لاراول را نصب کنید:
```

composer create-project laravel/laravel .
```
