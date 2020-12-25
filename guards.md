- [GUARDS](#guards)
  - [Creating Custom Auth](#creating-custom-auth)
  - [Creating Custom Guard](#creating-custom-guard)
  - [Creating Middleware For Guard Protect](#creating-middleware-for-guard-protect)
  - [Guard Usage In Routes](guard-usage-in-routes)

## GUARDS 
https://dev.to/niyiojeyinka/learn-to-use-laravel-guard-by-creating-an-ads-network-2ifp

### Creating Custom Auth  


```php
use Illuminate\Contracts\Auth\MustVerifyEmail;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;

class Advertiser extends Authenticatable
{
    use Notifiable;

    
    protected $fillable = [
        'name', 'email', 'password',
    ];

   
    protected $hidden = [
        'password', 'remember_token',
    ];

   
    protected $casts = [
        'email_verified_at' => 'datetime',
    ];
}

```


### Creating Custom Guard
```php
//config/auth.php

'guards' => [
        'web' => [
            'driver' => 'session',
            'provider' => 'users',
        ],

        'api' => [
            'driver' => 'token',
            'provider' => 'users',
            'hash' => false,
        ],
        
        //here we add new guard type
        'advertiser' => [
            'driver' => 'session',
            'provider' => 'advertisers',
        ],
],


'providers' => [
    'users' => [
        'driver' => 'eloquent',
        'model' => App\Models\User::class,
    ],
    
    //we add custom privder
    'advertisers' => [
          'driver' => 'eloquent',
          'model' => App\Advertiser::class,
    ],
],
    
```


### Creating middleware for guard protect
to create a middleware for each of our guards to protect each user types resources

```php artisan make:middleware AdvertiserAuth```

```php
//app/Http/middleware
namespace App\Http\Middleware;
use Auth;
use Illuminate\Support\Facades\Session;
use Closure;
class AdminAuth
{
   
    public function handle($request, Closure $next)
    {
        if(Auth::guard('advertiser')->check()){
           return $next($request);
         }
         return route('advertiser.login');
    }
}
```php

```php
protected $routeMiddleware = [...
  "advertiser.auth" =>\App\Http\Middleware\AdvertiserAuth::class,
  "publisher.auth" =>\App\Http\Middleware\PublisherAuth::class,
  "admin.auth" =>\App\Http\Middleware\AdminAuth::class,
]
```

### Guard Usage In Routes
```php
 Route::middleware([advertiser.auth])->group(
        function(){
//protected advertiser routes here
});

 Route::middleware([publisher.auth])->group(
        function(){
//protected publisher routes here
});

 Route::middleware([admin.auth])->group(
        function(){
//protected admin routes here
});
```
