    
    
    
    ### 1.way Using Direct
    
    
    ### 2.way Using With Mixin Class
    
    
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
