- [Anonymous Global Scopes](#anonymous-global-scopes)
- [Creating Global Scope](#creating-global-scope)
- [Removing Global Scopes](#removing-global-scopes)
- [Dynamic Local Query Scopes](#dynamic-local-query-scopes)
- [Local query scopes for relationship](#local-query-scopes-for-relationship)

### Anonymous Global Scopes

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Builder;
use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    /**
     * The "booted" method of the model.
     *
     * @return void
     */
    protected static function booted()
    {
        static::addGlobalScope('ancient', function (Builder $builder) {
            $builder->where('created_at', '<', now()->subYears(2000));
        });
    }
}

```


### Creating Global Scope
```php
//App/Scopes/AncientScope
<?php

namespace App\Scopes;

use Illuminate\Database\Eloquent\Builder;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Scope;

class AncientScope implements Scope
{
    /**
     * Apply the scope to a given Eloquent query builder.
     *
     * @param  \Illuminate\Database\Eloquent\Builder  $builder
     * @param  \Illuminate\Database\Eloquent\Model  $model
     * @return void
     */
    public function apply(Builder $builder, Model $model)
    {
        $builder->where('created_at', '<', now()->subYears(2000));
    }
}
```

```php
//Models
<?php

namespace App\Models;

use App\Scopes\AncientScope;
use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    /**
     * The "booted" method of the model.
     *
     * @return void
     */
    protected static function booted()
    {
        static::addGlobalScope(new AncientScope);
    }
}
```

### Removing Global Scopes

```php
User::withoutGlobalScope(AncientScope::class)->get();
```
```php
User::withoutGlobalScope('ancient')->get();
```
```php
// Remove all of the global scopes...
User::withoutGlobalScopes()->get();

// Remove some of the global scopes...
User::withoutGlobalScopes([
    FirstScope::class, SecondScope::class
])->get();
```

### Dynamic local query scopes 

```php
//app/Models/Post.php 

class Post extends Model
{
    public $table = "posts";
       
   
    protected $fillable = [
        'id', 'title', 'body', 'status'
    ];
    
    public function scopeDraft($query)
    {
        return $query->where('published', false);
    }
    
    public function scopeType($query, $type)
    {
        return $query->where('type', $type);
    }
    
}

```
Controller
```php
$draftSpecialOfferPosts = Post::draft()->type('special_offers')->get();
```


### Local query scopes for relationship

```php
//Model
class Post extends \Eloquent
 {
    public function category()
    {
        return $this->belongsTo('Category');
    }
 public function scopePublished($query)
    {
        return $query->where('published', 1);
    }
 }
```
Controller
```php
$categories = Category::with(['posts' => function ($q) {
  $q->published();
}])->get();
```
