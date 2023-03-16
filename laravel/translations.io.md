# Translating a Laravel Site with Translation.io

The current preferred way to translate a web platform is to link it to translation.io.
 - This is a free service for public / open source repositories.
 - Currently, it is used by the ccrp-soils and coffee-smallholder sites.


## Setup platform to use translation.io

Follow the instructions (copied from here: https://translation.io/laravel/usage)

1. `composer require tio/laravel`
2. Sign into https://translation.io/ and create a project.
3. Invite d.e.mills@stats4sd.org, ciara@stats4sd.org and dan@stats4sd.org.
   1. (We don't have a default stats4sd account on the service)
4. copy the snippet below into a new config file at `/config/translation.php`


(from https://github.com/translation/laravel#installation)
```php
<?php
return [
    'key' => env('TRANSLATIONIO_KEY'),
    'source_locale' => 'en',
    'target_locales' => ['fr', 'nl', 'de', 'es']
];
```

5. Add the API key for the project into the .env file.
6. initialise the project and push existing translations: `php artisan translation:init`


## Make strings translatable with GetText
See https://github.com/translation/laravel#gettext

In any PHP file, (including blade templates), any strings can be made translatable with the `t()` function, e.g.:

**Inside a PHP file**

```php

## Not translatable:
$string = "This is a string";

## Translatable:
$string = t("This is a string");

```


**Inside a Blade template**

```blade

{{-- Not translatable --}}

This is some text that would be rendered onto the page.

{{-- Translatable --}}
{{ t("This is some text that would be rendered onto the page") }}

```
(Note the `t()` must go inside `{{   }}`, the same as any other php function in a blade file.)


## Sync strings between the platform and translation.io

See the php artisan commands listed here:
- https://github.com/translation/laravel#usage


## Set and Store 'locale' for each session

To use the translations, we must change the locale that Laravel uses. We can use the default 'set.locale' Middleware that comes with the `Tio\Laravel` package.

To make the entire site translatable, go into the `\App\Http\Kernel` class, and add `\Tio\Laravel\Middleware\SetLocaleMiddleware::class` to `$middlewareGroups['web']`.

To make only certain routes translatable, add the `set.locale` middleware to the route groups that should be translatable. (For example, do this if you want the front-end to be translated, but not the back-end / admin panels).

## Add Menu items for users to change locale



