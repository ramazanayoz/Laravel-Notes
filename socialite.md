

- [Installation](#installation)
- [Configuration](#configuration)
- [Routing](#routing)
- [Retrieving User Details](#retrieving-user-details)
- [Retrieving User Details From A Token OAuth2](#retrieving-user-details-from-a-token-oauth2)
- [Retrieving User Details From A Token And Secret OAuth1](#retrieving-user-details-from-a-token-and-secret-oauth1)
- [Stateless Authentication](stateless-authentication)
- [Optional Parameters](optional-parameters)
- [Access Scopes](access-scopes)

#### Tutorials 
https://codesource.io/how-to-setup-facebook-login-with-socialite-in-laravel/
https://blog.hashvel.com/posts/laravel-socialite-tutorial/



![optional-description-here](https://lh3.googleusercontent.com/pw/ACtC-3eiWTjMc5vSqbhItQK_-8Ku-hj10tRhN0G9iCYwU2xrujv-jy-fPYhAiFFf6B0YA0-DTJjmg1sXqw7sV46R1JkiTBbqDqKsw6tulX4knpyMPox_ndw3IEQyA-28ZhL6QZ8xDy0TjZBv1BAigM0STcuF=w918-h464-no?authuser=0)



### Installation
```
composer require laravel/socialite
```


### Configuration

```php
'github' => [
    'client_id' => env('FACEBOOK_CLIENT_ID'),
    'client_secret' => env('FACEBOOK_CLIENT_SECRET'),
    'redirect' => 'env('FACEBOOK_REDIRECT')',
],

```

env file
```
FACEBOOK_APP_ID=XXXXXXXXXXXXXXXXXXX
FACEBOOK_APP_SECRET=XXXXXXXXXXXXXXXXXXXXXXXXXXXX
FACEBOOK_REDIRECT=http://localhost:8000/auth/callback
```

app.php providers section add these
```php
Laravel\Socialite\SocialiteServiceProvider::class,
```

app.php aliases section add these
 ```php
'Socialite' => Laravel\Socialite\Facades\Socialite::class,
```

### Routing

```php
Route::get('/auth/redirect', function () {
    return Socialite::driver('facebook')->redirect();
});

Route::get('/auth/callback', function () {
    $user = Socialite::driver('facebook')->user();

    // $user->token
});

```


### Retrieving User Details
```php
Route::get('/auth/callback', function () {
    $user = Socialite::driver('github')->user();

    // OAuth 2.0 providers...
    $token = $user->token;
    $refreshToken = $user->refreshToken;
    $expiresIn = $user->expiresIn;

    // OAuth 1.0 providers...
    $token = $user->token;
    $tokenSecret = $user->tokenSecret;

    // All providers...
    $user->getId();
    $user->getNickname();
    $user->getName();
    $user->getEmail();
    $user->getAvatar();
});
```

### Retrieving User Details From A Token OAuth2
```php
use Laravel\Socialite\Facades\Socialite;

$user = Socialite::driver('github')->userFromToken($token);
```


### Retrieving User Details From A Token And Secret OAuth1
```php
use Laravel\Socialite\Facades\Socialite;

$user = Socialite::driver('twitter')->userFromTokenAndSecret($token, $secret);
```
### Stateless Authentication

```php
use Laravel\Socialite\Facades\Socialite;
return Socialite::driver('google')->stateless()->user();
```

### Optional Parameters
```php
use Laravel\Socialite\Facades\Socialite;

return Socialite::driver('google')
    ->with(['hd' => 'example.com'])
    ->redirect();
```

### Access Scopes
```php
use Laravel\Socialite\Facades\Socialite;

return Socialite::driver('github')
    ->scopes(['read:user', 'public_repo'])
    ->redirect();
```

```php
return Socialite::driver('github')
    ->setScopes(['read:user', 'public_repo'])
    ->redirect();
```
