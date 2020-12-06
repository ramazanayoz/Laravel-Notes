
- [OneToOne Ex1](#video-tutorials)
- [OneToMany Ex1](#doc-tutorials)
- [ManyToMany Ex1](#manytomany-ex1)
- [ManyToMany Ex2](#manytomany-ex2)
- [ManyToMany Ex3](#manytomany-ex3)

- [Laravel Polymorphic Relationship](#laravel-polymorphic-relationship)
    - [Laravel Polymorphic Relationship](#one-to-many-example-1)


### OneToOne Ex1
Bir kullanıcının sadece bir telefonu olabilir ve bir telefon sadece bir kullanıcının olabilir
```php
public function user()
{
    return $this->belongsTo('App\Models\User', 'foreign_key', 'owner_key');
}

```
![optional-description-here](https://miro.medium.com/max/2340/1*uipLrynjxPjivTONTcAaRA.png)


### OneToMany Ex1
bir oda sadece bir oda tipine sahip olabilir ve bir oda tipi birkaç farklı odada kullanılabilir
![optional-description-here](https://miro.medium.com/max/1250/1*M-qvOcP9I2XFVn2i5j4GQQ.png)


### ManyToMany Ex1
bir ürünün birkaç kategorisi olabilir ve bir katogori birkaç farklı üründe olabilir
![optional-description-here](https://miro.medium.com/max/1250/1*GBrEZzJP3T5omjcQBFCcvg.png)

### ManyToMany Ex2
bir mağazada birkaç farklı ürün olabilir ve bir ürün birkaç farklı mağazada olabilir
![optional-description-here](https://miro.medium.com/max/1250/1*qcLiYJ_kpXFok5qEnoK-mQ.png)


### ManyToMany Ex3

User model
```php
class User extends Model
{
    /**
     * The roles that belong to the user.
     */
    public function roles()
    {
        return $this->belongsToMany('App\Models\Role', 'role_user', 'user_id', 'role_id');

    }
} 
```

not:: #Retrieving Intermediate Table Columns (pivot table attributune ulaşma)
#Retrieving Intermediate Table Columns (pivot table attributune ulaşma)
```php
return $this->belongsToMany('App\Models\Role')
    ->withTimestamps()
    ->withPivot('nameCol1', 'column2');
```

Role Model
```php
class Role extends Model
{
    /**
     * The users that belong to the role.
     */
    public function users()
    {
        return $this->belongsToMany('App\Models\User');
    }
}
```

Accessing from user to roles
```php
$user = App\Models\User::find(1);
foreach ($user->roles as $role) {
    //
} 
$roles = App\Models\User::find(1)->roles()->orderBy('name')->get();
```

controllerda pivot table record etme ve attribute ulaşma
```php
$user = User::find(1);
$user->roles()->sync(1,['nameCol1'=>'test name'] //record etme pivot table

foreach ($user->roles as $role) {
   echo $role->pivot->nameCol1
}
```



### Laravel Polymorphic Relationship
#### One to Many Example 1
https://www.itsolutionstuff.com/post/laravel-one-to-many-polymorphic-relationship-tutorialexample.html
Videonun bir çok yorumu olabilir
Postun bir çok yorumu olabilir
burada yorumlar tablosu ortak
Tablo yapısı
![optional-description-here](https://miro.medium.com/max/875/1*vsWqvRVK8Lci26zfJBnPrg.png)
![optional-description-here](https://miro.medium.com/max/875/1*aGuF_jhd9jxStoYr98dcVQ.png)
![optional-description-here](https://miro.medium.com/max/2300/1*o6zZOnQw__CGm-tL1KumEg.png)



