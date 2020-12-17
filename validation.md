

## Validate 

```php

//1.way
$this->validate($request, [
    'title' => 'required|unique:posts|max:255',
    'author.name' => 'required',
    'author.description' => 'required',
]);


//2.way Manually Creating Validators
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
