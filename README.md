# Laravel Paginator

Make the pagination for arrays or Collections

## Installation

### Composer require
``` bash
$ composer require amamarul/laravel-paginator
```
### Add Provider into config/app.php
``` php
Amamarul\Paginator\PaginatorServiceProvider::class,
```
### Usage
#### In Controller

- Array

``` php
use Amamarul\Paginator\Paginator;
use Illuminate\Http\Request;

public function index(Request $request)
{
    $currentPage  = isset($request['page']) ? (int) $request['page'] : 1;
    $perPage      = 1;
    $path         = $request->path();

    $items = array_map(function ($value) {
        return [
        'name' => 'User #' . $value,
        'url'  => '/user/' . $value,
        ];
        }, range(1,1000));

        $paginator = new Paginator($items);
        $paginator = $paginator->paginate($currentPage,$perPage, $path);

    return view('index')->with('paginator', $paginator);
}
```

- Collection

``` php
use App\User;
use Amamarul\Paginator\Paginator;
use Illuminate\Http\Request;

public function index(Request $request)
{
    $currentPage  = isset($request['page']) ? (int) $request['page'] : 1;
    $perPage      = 1;
    $path         = $request->path();

    $items = User::with('profile')->get()->sortBy('profile.name');

    $paginator = new Paginator($items);
    $paginator = $paginator->paginate($currentPage,$perPage, $path);

    return view('index')->with('paginator', $paginator);
}
```
#### In Blade View (index.blade.php)

``` php
@foreach ($paginator->items() as $element)
    <a href="{!!$element['url']!!}"><h3>{!!$element['name']!!}</h3></a>
@endforeach

{!! $paginator->render() !!}
```
#### Customize Page Name
By default the url has `page` name
  `http://127.0.0.1:8000/?page=3`
  If you´d like to change the page name yo must only add a fourth parameter with the name.
  Like this

  ``` php
  use App\User;
  use Amamarul\Paginator\Paginator;
  use Illuminate\Http\Request;

  public function index(Request $request)
  {
      $currentPage  = isset($request[$pageName]) ? (int) $request[$pageName] : 1;
      $perPage      = 1;
      $path         = $request->path();
      $pageName     = 'custom-name';

      $items = User::with('profile')->get()->sortBy('profile.name');

      $paginator = new Paginator($items);
      $paginator = $paginator->paginate($currentPage,$perPage, $path, $pageName);

      return view('index')->with('paginator', $paginator);
  }
  ```

### Feel free to send improvements
Created by [Maru Amallo-amamarul][760a7857]

  [760a7857]: https://github.com/amamarul "https://github.com/amamarul"
