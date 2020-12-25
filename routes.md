

- [Create Custom Route File](#create-custom-route-file)
- [NameSpace](#namespace)

### Create Custom Route File

```php
//app/Providers/RouteSericeProvider
$this->routes(function () {
    Route::prefix('api')
        ->middleware('api')
        ->namespace($this->namespace)
        ->group(base_path('routes/api.php'));

    Route::middleware('web')
        ->namespace($this->namespace)
        ->group(base_path('routes/web.php'));
        
   //added these route as a custom
    Route::middleware('admin')
        ->namespace($this->namespace)
        ->group(base_path('routes/adminRoute.php'));
        
});

```
routes/adminRoute.php
```php
<?php

use Illuminate\Support\Facades\Route;


Route::get('/test',function(){
    return "test";
});
```


### NameSpace

Another common use-case for route groups is assigning the same PHP namespace to a group of controllers. You may use the namespace parameter in your group attribute array to specify the namespace for all controllers within the group:

```php
Route::group(['namespace' => 'Admin'], function()
{
    // Controllers Within The "App\Http\Controllers\Admin" Namespace

    Route::group(['namespace' => 'User'], function()
    {
        // Controllers Within The "App\Http\Controllers\Admin\User" Namespace
    });
});
```
