# How to fix Undefined Variable/Index Notice?

A common Notice when working with PHP can be:

```
Notice: Undefined variable: my_var in C:\xampp\htdocs\index.php on line 14
```

or

```
Notice: Undefined index: my_index C:\xampp\htdocs\index.php on line 24
```

## Why does this happen?

You have some lines to set a Variable or get posted Data from a Form like this:

```php
<?php
  $my_var = $_POST['myPostData'];
?>
```

Now PHP does not have $_POST['myPostData'] if there is no POST Data being sent to the Page. This is when these Errors come into Play.


## How to fix "Undefined variable"?

```php
<?php
  $my_var = ""; // Or $my_var = 0; for numbers to make sure the Variable is initialised
  //now use isset()
  $my_var = isset($_POST['myPostData']) ? $_POST['myPostData'] : "";
  //or empty()
  $my_var = !empty($_POST['myPostData']) ? $_POST['myPostData'] : "";
?>
```

## How to fix "Undefined index"?

```php
<?php
 //again we have two options here
 //via array_key_exists()
 $my_var = array_key_exists('my_index', $my_array) ? $my_array['my_index'] : "";
 //or via isset()
 $my_var = isset($my_array['my_index']) ? $my_array['my_index'] : "";
?>
```

Note that `array_key_exists()` returns `true` when array has the specified key ignoring its value
while `isset()` returns `true` only if the specified key exists and is not `null`.

```php
<?php
$my_array = [];
$my_array['exists'] = null;
var_dump(array_key_exists('exists', $my_array));
var_dump(isset($my_array['exists']));
```

So, you should `array_key_exists()` only in situations which `null` values are relevant,
otherwise use `isset()` because it is much faster than `array_key_exists()`.

### Output
```
bool(true)
bool(false)
```


## Null Coalescing Operator

Since PHP 7 there is also the ?? (or Null Coalescing) Operator available:

```php
<?php
  $my_var = $_POST['myPostData'] ?? "";
?>
```
