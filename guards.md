- [GUARDS](#guards)
  - [Creating Custom Auth](#creating-custom-auth)
  - [Creating Custom Guard](#creating-custom-guard)
  - [Creating Middleware For Guard Protect](#creating-middleware-for-guard-protect)
  - [Guard Usage In Routes](#guard-usage-in-routes)
  - [Specifying A Guard in Controller](#specifying-a-guard-in-controller)
  - [Accessing Specific Guard Instances](#accessing-specific-guard-instances)

## GUARDS 
https://dev.to/niyiojeyinka/learn-to-use-laravel-guard-by-creating-an-ads-network-2ifp
https://morioh.com/p/6ac297d15e53
https://www.codecheef.org/article/laravel-multi-authentication-using-guard

### Creating Custom Auth  


```php
use Illuminate\Contracts\Auth\MustVerifyEmail;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;

class Advertiser extends Authenticatable
{
    use Notifiable;

    protected $guard = 'advertiser';

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



'passwords' => [
      'users' => [
          'provider' => 'users',
          'table' => 'password_resets',
          'expire' => 60,
      ],
        
        //add custom 
      'advertisers' => [  // And here also
          'provider' => 'advertisers',
          'table' => 'password_resets',
          'expire' => 60,
],
    
```


### Creating middleware for guard protect
to create a middleware for each of our guards to protect each user types resources

```
php artisan make:middleware AdvertiserAuth
```

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
```

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

### Specifying A Guard in Controller
```php
public function __construct()
{
    $this->middleware('advertiser.auth');
}
```

### Accessing Specific Guard Instances
```php 
if (Auth::guard('advertiser')->attempt($credentials)) {
    //
}
```

You may specify the guard instance you would like to use:
```php
Auth::guard('advertiser')->login($user);
```

