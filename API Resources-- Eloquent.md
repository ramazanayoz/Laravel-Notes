
- [Video Tutorials](#video-tutorials)
- [Doc Tutorials](#doc-tutorials)



### Video Tutorials
#### Orginal Doc
https://laravel.com/docs/8.x/eloquent-resources#pagination

#### Api resource
https://laracasts.com/series/whats-new-in-laravel-5-5/episodes/20

#### API Resource: Loading relationship the right way
https://www.youtube.com/watch?v=__DyyzHgYGg

### Doc Tutorials
#### 1
https://laravel.com/docs/8.x/eloquent-resources#pagination

#### 2
https://vegibit.com/laravel-api-resource-tutorial/
![optional-description-here](https://lh3.googleusercontent.com/pw/ACtC-3euGsQc8UkCaDp4F3CTz4hQQjs0OvAxdd2ZTKWT_qVzYKI7-AgVTwtznek4H5zGX6CAMrM_1yMLKQ9lfSbMY_b5-74qQAqqvu2A9coGDwQ09EFFxnsKrAtAa6ne4niRMveyv1GVH3f_3iTD33Vt5otD=w1622-h943-no?authuser=0)

##### With Usage
```php
 
class Post extends JsonResource
{
    public function toArray($request)
    {
        return [
            'id' => $this->id,
            'title' => $this->title,
            'body' => $this->body
        ];
    }
    
    public function with($request)
    {
        return [
            'version' => '1.0.0',
            'api_url' => url('http://lpgvue/api')
        ];
    }
}

```

```json
{
  "data": {
    "id": 5,
    "title": "Nam velit doloribus cumque qui cumque.",
    "body": "Voluptates culpa quidem omnis soluta animi eaque. Nihil voluptates voluptas quis cum adipisci tenetur. Eveniet voluptatem quo velit ea occaecati ducimus sed."
  },
  "version": "1.0.0",
  "api_url": "http://lpgvue/api"
}

```

#### 3
https://blog.pusher.com/build-rest-api-laravel-api-resources/

![optional-description-here](https://miro.medium.com/max/1890/1*dvryEZh62iLKL1nC-PErYA.png)

#### 4
https://dev.to/tngeene/how-to-create-an-api-with-laravel-resources-19ee
#### 5
https://medium.com/@kitloong/create-crud-rest-api-with-laravel-api-resource-3146d91b38b6
#### 6
https://sam-ngu.medium.com/avoiding-infinite-nested-relationship-loop-in-laravel-api-resource-35685898b360
