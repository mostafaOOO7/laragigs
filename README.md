# LaraGigs
Traversy Media - Learn the Laravel PHP framework from scratch by building a job listing application with Laravel 10 and MySQL.

## Task Description
This project is segregated step by step task follow this.

### 🛠 Setting Up Laravel
A new project create from laravel.com
```
composer global require laravel/installer
```
```
laravel new laragigs
```

### 🔗 Routing & Responses
```
// Custom Header
Route::get('/hello', function(){
    return response('<h1>Hello World</h1>', 200)
    ->header('Content-Type', 'text/plain')
    ->header('foo', 'bar'); //Custom Header
});

// Wilecard
Route::get('/posts/{id}', function($id){
    // ddd($id); // Dump, Die, Debug
    return response('Post '.$id);
})->where('id', '[0-9]+');

// Request and Query String/Params
Route::get('/search', function(Request $request){
    return $request->name .' '.$request->city;
});
```
### 📱 API Routing
```
Route::get('/posts', function(){
    return response()->json([
        'posts' => [
            [
                'title' => 'Post One'
            ]
        ]
    ]);
});

// Access API endpoint form URL or Postman
http://laragigs.test/api/posts
````
### 👁 Basic View & Passing Data
```
Route::get('/', function () {
    return view('listings', [
        'heading' => 'Latest Listing',
        'listings' =>  [
            [
              'id' => 1,
              'title' => Title One,
              'description' => 'Some text some text some text some text some text'
            ],
            [
              'id' => 2,
              'title' => Title Two,
              'description' => 'Some text some text some text some text some text'
            ]
        ]
    ]);
    // return view('listing.php');
});
```
Create a view page in the resource folder `resources > views > listings.php`
```
<h1><?php echo $heading; ?></h1>
<?php foreach($lisitng as $listing): ?>
    <h2><?php echo $listing['title]; ?></h2>
    <p><?php echo $listing['description]; ?></p>
<?php endforeach; ?>
```

### 🥽 Blade Templates & Basic Directives
Rename listing.php to `resources > views > listings.blade.php`
```
<h1>{{$heading}}</h1>

@unless(count($listing) == 0)

@foreach($lisitng as $listing)
  <h2> <a href="/listings/{{$listing['id']}}"> {{$listing['title]}} </a> </h2>
  <p>{{$listing['description]}}</p>
@endforeach

@else
<p>No listing found</p>
@endunless
```

### Creating a Basic Model
Create a new file in Models folder `app > Models > Listings.php`

```
<?php
namespace App\Models;

class Listing {

  // All Listing
  public static function all(){
    return [
      [
        'id' => 1,
        'title' => Title One,
        'description' => 'Some text some text some text some text some text'
      ],
      [
        'id' => 2,
        'title' => Title Two,
        'description' => 'Some text some text some text some text some text'
      ]
    ]
  }

  // Single Listing
  public static function find(){
    $listing = self::all();

    foreach($listing as $listing) {
      if($listing['id] == $id){
        return $listing;
      }
    }
  }

}
```

Modify in routes folder `routes > web.php`
```
use App\Models\Listing;
.....
.......
// All Listing
Route::get('/', function () {
    return view('listings', [
        'heading' => 'Latest Listing',
        'listings' =>  Listing::all() 
    ]);
});

// Single Listing
Route::get('/listings/{id}', function($id) {
  return view('listing', [
    'listing' => Listing::find($id)
  ]);
});
```

Create a new file in the View folder `resources > view > Listing.blade.php` for details listing
```
<h2>{{$listing['title]}}</h2>
<p>{{$listing['description]}}</p>
```

### Create a Database & User
We can db connection setup in `config > database.php` or `.env` files modify

Use MySQL Cheat Sheet [Brad Traversy](https://gist.github.com/bradtraversy/c831baaad44343cc945e76c2e30927b3)

### Creating Database Migration

