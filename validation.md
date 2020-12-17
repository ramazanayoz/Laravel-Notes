

## Validate 

### 1-way creating validator
```php
$this->validate($request, [
    'title' => 'required|unique:posts|max:255',
    'author.name' => 'required',
    'author.description' => 'required',
]);

```

2-way Manually Creating Validators
```php
$validator = Validator::make($request->all(), [
    'title' => 'required|unique:posts|max:255',
    'body' => 'required',
]);

if ($validator->fails()) {
    return redirect('post/create')
                ->withErrors($validator)
                ->withInput();
}

```
