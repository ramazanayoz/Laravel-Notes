
- [1. Way Using Direct](#1-way-using-direct)
- [2. Way Using With Mixin Class](#2-way-using-with-mixin-class)
  
 ```
//kaynak https://www.larashout.com/laravel-macros-extending-laravels-core-classes
 ```
 
### 1-Way Using Direct
         
```php
     class AppServiceProvider extends ServiceProvider
    {
        /**
         * Register any application services.
         *
         * @return void
         */
        public function register()
        {
            //
        }

        /**
         * Bootstrap any application services.
         *
         * @return void
         */
        public function boot()
        {
            Str::macro('isLength', function ($str, $length) {

                return static::length($str) == $length;
            });
        }
    }
```
##### controller part
```php
use Illuminate\Support\Str;
dd(Str::isLength('This is a Laravel Macro', 23)); // true
```


### 2-Way Using With Mixin Class

First Create mixin class in App\Mixins\StrMixin

```php
namespace App\Mixins;

class StrMixin
{
    /**
     * @return \Closure
     */
    public function isLength()
    {
        return function($str, $length) {
            return static::length($str) == $length;
        };
    }

    /**
     * @return \Closure
     */
    public function appendTo()
    {
        return function($str, $char) {
            return $char.$str;
        };
    }
}
```

##### AppServiceProvider part
```php
namespace App\Providers;

use App\Mixins\StrMixin;
use Illuminate\Support\Str;
use Illuminate\Support\ServiceProvider;

class AppServiceProvider extends ServiceProvider
{
    /**
     * Register any application services.
     *
     * @return void
     */
    public function register()
    {
        //
    }

    /**
     * Bootstrap any application services.
     *
     * @return void
     */
    public function boot()
    {
        Str::mixin(new StrMixin);
    }
}

```

##### controller part
```php
use Illuminate\Support\Str;
dd(Str::isLength('This is a Laravel Macro', 23)); // true
```
